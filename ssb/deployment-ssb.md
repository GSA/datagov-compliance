# SSB Deployment

Figure 10-1 Deployment Diagram
![SSB typical deployment interactions](../out/ssb/deployment-ssb/SSB%20deployment%20interactions.svg)

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Deployment.puml

LAYOUT_WITH_LEGEND()
title SSB deployment interactions

note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note

Deployment_Node(aws, "AWS GovCloud", "Amazon Web Services Region") {
    Deployment_Node(aws_alb, "Public-facing TLS termination", "AWS ALB") {
        Deployment_Node(cloudgov, "cloud.gov", "Cloud Foundry PaaS") {
            Deployment_Node(cloudgov_router, "cloud.gov router", "Cloud Foundry service") {
                Boundary(cloudgov_endpoints, "cloud.gov endpoints") {
                    System_Ext(cloudgov_logdrain, "logs.fr.cloud.gov", "ELK")
                    System_Ext(cloudgov_controller, "cloud.gov controller", "Cloud Foundry orchestration")
                    System_Ext(cloudgov_dashboard, "cloud.gov dashboard", "Cloud Foundry web UI")
                    System_Ext(cloudgov_sshproxy, "cloud.gov SSH proxy", "Cloud Foundry application inspection")
                    System_Ext(cloudgov_uaa, "cloud.gov authentication", "Cloud Foundry service")
                }
                Boundary(atob, "ATO boundary") {
                    Deployment_Node(organization, "SSB organization") {
                        Deployment_Node(space, "SSB space") {
                            System(app, "application", "SSB component")
                            ContainerDb(cloudgov_services, "cloud.gov data services", "AWS RDS, S3, etc", "Stores persistent data for apps")
                        }
                    }
                }
            }
        }
    }
}

' Application output (not critical here, so commented out)
' Rel(app, cloudgov_services, "reads/writes data", "data protocols")
' Rel(app, cloudgov_logdrain, "logs to", "stdout/stderr")

' cloud.gov internals
Rel(cloudgov_dashboard, cloudgov_controller, "manipulates", "https (443)")
Lay_D(cloudgov_dashboard, cloudgov_controller) 
Rel(cloudgov_dashboard, cloudgov_uaa, "authenticates", "https (443)")
Rel(cloudgov_controller, cloudgov_uaa, "authenticates", "https (443)")
Rel(cloudgov_logdrain, cloudgov_uaa, "authenticates", "https (443)")
Rel(cloudgov_sshproxy, cloudgov_uaa, "authenticates", "https (443)")
Rel(cloudgov_sshproxy, app, "creates shell")
Rel(cloudgov_controller, space, "provisions/inspects/operates apps")
Rel(cloudgov_controller, cloudgov_services, "provisions/inspects services")

Person(team, "SSB team member")
Deployment_Node(computer, "Computing Device", "MS Windows, OS X, or Linux"){
    System(browser, "Web Browser", "any modern version")
    System(git_cli, "git CLI", "local version control command")
    System(cf_cli, "cf CLI", "local Cloud Foundry command")
}
Rel(team, browser, "uses")
Rel(team, cf_cli, "uses")
Rel(team, git_cli, "uses")

Boundary(gsa_saas, "GSA-authorized SaaS") { 
    Deployment_Node(gsuite, "GSA G Suite", "Collaboration SaaS") {
        System(ggroup, "datagovhelp@gsa.gov", "Google Group")
    }
    Boundary(deploymentservices, "Deployment services") {
        System_Ext(snyk, "Snyk", "Dependency analysis SaaS")
        Deployment_Node(github, "GitHub", "VCS SaaS"){
            System(github_repo, "GSA/datagov-ssb", "Code repository")
        }
    }
    System_Ext(newrelic, "New Relic", "Monitoring SaaS")
    System_Ext(secureauth, "GSA SecureAuth", "SAML Identity Provider")
}
Rel_(cloudgov_uaa, secureauth, "proxies authentication requests", "SAML/https (443)", "..>")


'Team interactions
Rel_Back(team, newrelic, "reports problems", "email")
Rel(browser, cloudgov_logdrain, "reviews logs", "https (443)")
'Lay_D(app, cloudgov_logdrain)
Rel(browser, github_repo, "makes pull-request, approves PRs", "https (443)")
Rel(git_cli, github_repo, "commits code", "ssh (22)")
Rel(cf_cli, cloudgov_controller, "interacts with cloud.gov API", "https (443)")
Rel(cf_cli, cloudgov_sshproxy, "establishes session", "ssh (22)")
Rel(browser, cloudgov_dashboard, "interacts with cloud.gov dashboard", "https (443)")
Rel(team, ggroup, "provides assistance", "email")
Rel(team, secureauth, "authenticates", "https (443)")

' Deployment automation
Rel_Up(snyk, github_repo, "watches for changes, reports vulnerable dependencies", "GitHub API")
Rel(github_repo, cloudgov_controller, "pushes code, invokes tasks", "https (443)")

' Non-functional, just helps with layout
Lay_R(cloudgov_endpoints, atob)  

@enduml
```

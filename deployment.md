## Deployment
Configuration management and deployment for every data.gov component follows the pattern described unless otherwise specified.

Figure 10-1 Deployment Diagram
![data.gov typical deployment interactions](http://www.plantuml.com/plantuml/svg/hLTTRnkx4Ntk_egfNc81jeJcvgaFHHJRTOmgZk6rcnP58N2vOuaDbznok5AKml-zdFsWLp9X-EBUHG9pHpo7CyF36Uv7yiBvfjBP7vMHkYaHDjxNzEVvt8dTRAtyfYaQGYUjyMZyJDfgBagdXDdgfcxcvn_ERhKm_k7BSgw5H_B1jBh0MjjznO2QoRFbufzV7-vNtwxlFw-MbrUNDnSdqymhhn5AuSLiRRVWzxMIGaCPmQ2CHoUaLzPGbXdh4GJ1fP5kNxFn9bYoXTOGt3FOpMEyaXvB40u1sxdyjWknmFqoXp_DtaDZD1A1zHjqEqK8p5tECZHbEyoovn1MDxR44x6ZKvWilkLmPRVdsZRbXDyhyTCQ-8O5vEYsIYB17Qw37vCf_2_xGu9b9NGHaBTDePKyUnHIcNNhbqTNAICyW_kb5ijFBG_qpp6TP3z2xiBqTiXvP6ph7_pLDgPqUxWL8Z-WORUIF2jd6uzkJ0UTwIKhTV5sSORd4zk5sp-JeIbhguod0xve7RiKdtnF7glLvG__pAFjkdH2cK0JXZHxTBE3Q2-NVvjCtyJ4AdPMwwCGepaHfdLoW-HTjozlMwOKj2ciSEN1Aj7wSf4T5l1m_KPseatjx8_z0NcUVuRM-f9SrBLMilKVbA4QvQy8fH7YO1tH-0qQt_EzeefM4qylrZYtnWjbq5qKeuWw41sbouk1ZeO3SNUHdqB-oocWbspBlNL8KACZHLoWEjYZTHms9OACCXNU5inw_nMAtfPGPFhSMRSMHlriSn0uOWaRsoFPMmYf5XB7JAqXHP7OiPCGQi2EzdaoffAsggr1uxji99-dx2rMjhIxol_3lwSiUmUBaRniu-l6mucn7gHJlgtd6tGXDBBi24UC9S-R1kWTwixtb18SYfBcky22rExg41Jht5jfDKsEIDBr0hnbAFaoB3iFVz2vW0pGABQkqHYXAMEwr45Erv1A65KtMlXEectlX9CF7twPXXMMOh-wU2lP55vRUdmOun5Chyei2VxVH1Cp-bj8OjtwVKWwqKk7Z0BQeDQlHJ-Si83UAkABmhmlXZItDJfcOKvwXIIXqoHXB5iiirjqPCs9Hr6DJoM_GuLLWIp6uth0PxRlnEVja2y95yYadA0lENnJfhHy9_cQmpzEmJfOAjFyc4ovOiJAKBWmfwNXAygdxeqPXDb3PKjq1hReA3Q40RXMVYMr2ZF32CwNrmpILWezp1-wPdkiXIaFyVAnXyl7G_H1knXXdpZhVPQUlMw8apZwgUDD_S8U7_oKnLA_9h4Y8NYthlB56UlDElKJIyZxgn6ylAgjgL4UEmXSGSvlRR-pMel2TXU2I11Auf2vjRDDtMzrsEaDwlfZy62uhLnPkzO8LpmhUDdtoEXh6TqOQUpPjoYjTj5KvrPEQfIAFMs7vzUjrzVpyulUsnICpFuxzzFmPz8jYAP48_SWZD1xKdGCFivNzwt00Ih_kIcOvU_dUO-AhexMxH4hXxNjqZp_L-nLO4I5_-xoNIBm75BUSe_E3jeJjwHH7AMesVt5xQJJVEU-LpBwFvvjSETGAndctk0ExdZSdYzhU34-T0aKeMmSiefwTUIjOH4C3CuNNvPmNN95ytkuvV9GeXialZgwMnsGSMbJI4UtB73utmR9qy0-7nL8jircVs6zP-_kKLI77qsyuYSXl_U7P1GmfzHvucvQQAnQSgo46gfeF8HTIXrk5Uu8EbYwJ4TCgXT7Xc1AmDzrtJBToO_O3uLJYUzy1MoqFkiJnSs1ot8mtzwbNOq5xeYC2vBo1DAMo42Y3PoyVz-ZObbBtmQ6p5FxiJcwNYnkhvDUHBxKvHx9Yq8hsY018N7DJNWKavAuERpYKPoKzgjJIonbTITY0a6ao0iZyLWkZ3XIy-jzFSXrL4DOyhPgvTvgzg6EbUnuYtR2Iyu9N_BbHfWrbyX1pHx9XnTgjAT9B4Rj1am7OgvrlufqssY3JXGQeHmAey8a_yZhj4RgXZROwUiKbDbQ5gyNz3rr9jZXYnHb_4vtZI6WAh2qeHDO0mztovTamFc-iURiiJ5jRH3w5FxJa0TkKBq-jDW7udVTORnBVidpPzCK0jl7O6mg_Ny0)
```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/latest/C4_Deployment.puml

LAYOUT_WITH_LEGEND()
title data.gov typical deployment interactions

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
                ContainerDb(cloudgov_services, "cloud.gov data services", "AWS RDS, S3, etc", "Stores persistent data for apps")
                Boundary(atob, "ATO boundary") {
                    Deployment_Node(organization, "data.gov organization") {
                        Deployment_Node(space, "data.gov space") {
                            System(app, "application", "data.gov component")
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

Person(team, "data.gov team member")
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
        System_Ext(circleci, "CircleCI", "CI/CD SaaS")
        System_Ext(snyk, "Snyk", "Dependency analysis SaaS")
        Deployment_Node(github, "GitHub", "VCS SaaS"){
            System(github_repo, "GSA/[component name]", "Code repository")
        }
    }
	System_Ext(dap, "DAP", "Web analytics SaaS")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
	System_Ext(secureauth, "GSA SecureAuth", "SAML Identity Provider")
}
Rel_(cloudgov_uaa, secureauth, "proxies authentication requests", "SAML/https (443)", "..>")


'Team interactions
Rel_Back(team, newrelic, "reports problems", "email")
Rel(browser, dap, "reviews reports", "https (443)")
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
Rel_Up(circleci, github_repo, "watches for changes, reports test results", "GitHub API")
Rel_Up(snyk, github_repo, "watches for changes, reports vulnerable dependencies", "GitHub API")
Rel(circleci, cloudgov_controller, "pushes code, invokes tasks", "https (443)")
'Rel_D(circleci, cloudgov_router, "runs smoke tests on URLs", "https (443)") 

' Non-functional, just helps with layout'
'Lay_R(cloudgov_endpoints, atob)  

@enduml
```

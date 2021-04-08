dashboard.data.gov boundary view
![dashboard.data.gov boundary view](../out/apps/dashboard.boundary/dashboard.data.gov%20boundary%20view.svg)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml

LAYOUT_WITH_LEGEND()
title dashboard.data.gov boundary view


Person_Ext(public, "Public Dashboard Viewer", "A member of the public or agency personnel")

note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note

Boundary(aws, "AWS GovCloud") {
    
    Boundary(cloudgov, "cloud.gov") {
    	System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(dashboard, "data.gov dashboard") {
                Container(dashboard_app, "<&layers> Web Application", "PHP 7.4.3, CodeIgniter 3.1.11", "Generates reports and delivers static HTML/CSS and forms")
                ContainerDb(dashboard_db, "MySQL Database", "AWS RDS", "Stores agency information and reports")
                ContainerDb(dashboard_s3, "Blob Store", "AWS S3", "Caches crawled data.json and digitalstrategy.json files")
            }
        }
    }
}

Boundary(agency, "Agency ATO boundary") { 
	System_Ext(agencysites, "<&layers> Agency websites", "Main (public) website for the monitored agencies")
}

Boundary(gsa_saas, "GSA-authorized SaaS") { 
	System_Ext(dap, "DAP", "Analytics collection")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}

public -> dap : **reports usage** \n//[https (443)]//
Rel(dashboard_app, newrelic, "reports telemetry", "tcp (443)")


Rel(public, aws_alb, "views static content and validates data.json files", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, dashboard_app, "proxies requests", "https GET/POST (443)")
Rel_Neighbor(dashboard_app, agencysites, "crawls data.json and digitalstrategy.json files", "https GET/HEAD (443)") 
Rel(dashboard_app, dashboard_db, "reads agency info, reads/writes reports, ", "mysql (3306)")
Rel(dashboard_app, dashboard_s3, "stores crawled files", "https GET/POST(443)")
@enduml
```

WordPress boundary view
![www.data.gov boundary view](out/wordpress.boundary/www.data.gov%20boundary%20view.svg)

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title www.data.gov boundary view
Person(personnel, "Data.gov PMO", "A member of the data.gov PMO")
Person_Ext(public, "Public", "Member of the public")
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS GovCloud") {
    Boundary(cloudgov, "cloud.gov") {
    	System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(www, "data.gov Inventory") {
                Container(www_app, "<&layers> WWW Application", "PHP 7.4.3, Wordpress 5.2.6", "Presents content about the data.gov program")
            }
        }
        ContainerDb(www_db, "PostgreSQL Database", "AWS RDS", "Contains content and configuration for the Wordpress site in www-app")
        ContainerDb(www_s3, "AWS S3 bucket", "AWS RDS", "Stores static file assets")
    }
}
System_Ext(OMB, "OMB MAX", "Authentication As a Service")
Boundary(gsa_saas, "GSA-authorized SaaS") {
	System_Ext(dap, "DAP", "Analytics collection")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
personnel -> dap : **reports usage** \n//[https (443)]//
public -> dap : **reports usage** \n//[https (443)]//
Rel(www_app, newrelic, "reports telemetry", "tcp (443)")
Rel(personnel, aws_alb, "manage data.gov program information", "https GET/POST (443)")
Rel(public, aws_alb, "consume data.gov program information", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, www_app, "proxies requests", "https GET/POST (443)")
www_app <-> OMB : **authenticates** \n//[SAML 2.0]//
'Rel(www_app, OMB, "authenticates", "SAML 2.0")
Rel(personnel, OMB, "verify identity", "https GET/POST (443)")
Rel(www_app, www_db, "reads/writes local dataset records", "psql (5432)")
Rel(www_app, www_s3, "reads/writes data content", "psql (5432)")
@enduml
```

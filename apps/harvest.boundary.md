Catalog boundary view
![catalog.data.gov boundary view](../out/apps/harvester.boundary/harvester%20boundary%20view.svg)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title catalog.data.gov boundary view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor")
Person_Ext(public, "Public", "Member of the public")
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS GovCloud") {
    Boundary(cloudgov, "cloud.gov") {
        System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(harvest, "data.gov Harvest") {
                Container(harvest_proxy, "<&layers> Harvest Proxy", "NGINX", "NGINX proxy protecting harvest application")
                Container(harvest_app, "<&layers> Harvest Application", "Python 3", "Presents interface for harvest sources and job information")
                Container(harvest_jobs, "<&layers> Harvest Jobs", "Python 3", "Processes a harvest source, inserting, updating, and deleting metadata for the catalog")
                ContainerDb(harvest_db, "PostgreSQL Database", "AWS RDS", "Holds the records of known datasets")
                ContainerDb(catalog_openSearch, "AWS OpenSearch", "AWS RDS", "Holds the metadata from harvest jobs for the catalog search")
            }
        }
    }
}
Boundary(gsa_saas, "GSA-authorized SaaS") {
    System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
System_Ext(login, "login.gov", "Authentication As a Service")
harvest_app <-> login : **authenticates** \n//[SAML 2.0]//
Rel(personnel, login, "verify identity", "https GET/POST (443)")
Rel(harvest_app, newrelic, "reports telemetry", "tcp (443)")
Rel(personnel, aws_alb, "manage data harvest sources", "https GET/POST (443)")
Rel(public, aws_alb, "search and download federal open data", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, harvest_proxy, "proxies requests", "https GET/POST (443)")
Rel(harvest_proxy, harvest_app, "proxies requests", "https GET/POST (443)")
Rel(harvest_app, harvest_db, "reads/writes local dataset records", "psql (5432)")
Rel(harvest_app, catalog_openSearch, "reads/writes data content", "psql (443)")
Rel(harvest_jobs, harvest_db, "reads/writes local dataset records", "psql (5432)")
Rel(harvest_jobs, catalog_openSearch, "reads/writes data content", "psql (443)")
Rel(harvest_app, harvest_jobs, "starts a cloud.gov task per job", "https GET/POST (443)")

@enduml
```

Catalog boundary view
![catalog.data.gov boundary view](../out/apps/catalog.boundary/catalog.data.gov%20boundary%20view.svg)
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
            System_Boundary(catalog, "data.gov Catalog") {
                Container(catalog_app, "<&layers> Catalog Application", "Python 3.8.3, CKAN 2.8", "Presents a search engine for metadata about government open data. Schedules and runs through a queue of harvesting jobs to refresh records of known datasets")
                ContainerDb(catalog_db, "PostgreSQL Database", "AWS RDS", "Holds the records of known datasets")
                ContainerDb(catalog_s3, "Redis Queue", "AWS RDS", "Holds the state of the queue of harvest jobs for the main application")
            }
        }
    }
}
System_Ext(OMB, "OMB MAX", "Authentication As a Service")
Boundary(gsa_saas, "GSA-authorized SaaS") {
    System_Ext(dap, "DAP", "Analytics collection")
    System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
personnel -> dap : **reports usage** \n//[https (443)]//
public -> dap : **reports usage** \n//[https (443)]//
Rel(catalog_app, newrelic, "reports telemetry", "tcp (443)")
Rel(personnel, aws_alb, "manage data harvest sources", "https GET/POST (443)")
Rel(public, aws_alb, "search and download federal open data", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, catalog_app, "proxies requests", "https GET/POST (443)")
catalog_app <-> OMB : **authenticates** \n//[SAML 2.0]//
Rel(personnel, OMB, "verify identity", "https GET/POST (443)")
Rel(catalog_app, catalog_db, "reads/writes local dataset records", "psql (5432)")
Rel(catalog_app, catalog_s3, "reads/writes data content", "psql (5432)")
Boundary(solrb, "Solr Service Boundary") {
  ContainerDb(solr, "Solr", "indexed search provider")
}
Rel(catalog_app, solr, "loads indexes, runs searches", "https (443)")
@enduml
```

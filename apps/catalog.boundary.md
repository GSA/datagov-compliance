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
                Container(catalog_proxy, "<&layers> Catalog Proxy", "NGINX", "NGINX proxy protecting catalog application")
                Container(catalog_app, "<&layers> Catalog Application", "Python 3", "Presents a search engine for metadata about government open data.")
                ContainerDb(catalog_db, "PostgreSQL Database", "AWS RDS", "Holds the records of known datasets")
                ContainerDb(catalog_openSearch, "AWS OpenSearch", "AWS RDS", "Holds the metadata from harvest jobs for the catalog search")
            }
        }
    }
}
Boundary(gsa_saas, "GSA-authorized SaaS") {
    System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
Rel(catalog_app, newrelic, "reports telemetry", "tcp (443)")
Rel(personnel, aws_alb, "manage data harvest sources", "https GET/POST (443)")
Rel(public, aws_alb, "search and download federal open data", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, catalog_proxy, "proxies requests", "https GET/POST (443)")
Rel(catalog_proxy, catalog_app, "proxies requests", "https GET/POST (443)")
Rel(catalog_app, catalog_db, "reads/writes local dataset records", "psql (5432)")
Rel(catalog_app, catalog_openSearch, "reads/writes data content", "psql (5432)")

@enduml
```

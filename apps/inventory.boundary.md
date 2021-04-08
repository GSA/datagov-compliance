Inventory boundary view
![inventory.data.gov boundary view](../out/apps/inventory.boundary/inventory.data.gov%20boundary%20view.svg)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title inventory.data.gov boundary view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor")
Person_Ext(harvester, "catalog Harvester", "catalog.data.gov")
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS GovCloud") {
    Boundary(cloudgov, "cloud.gov") {
        System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(inventory, "data.gov Inventory") {
                Container(inventory_app, "<&layers> Inventory application", "Python 3.8.3, CKAN 2.8", "Presents a UX for agency users to publish government open data and add metadata. Presents a harvest target for the catalog app to query")
                Container(datapusher_app, "<&layers> DataPusher application", "Python 3.8.3, CKAN 2.8", "Process uploaded open data for use in DataStore API")
                ContainerDb(datapusher_db, "DataPusher database", "AWS RDS (PostgreSQL)", "Stores job queue for processing uploaded open data resources for DataStore API")
                ContainerDb(inventory_db, "Inventory database", "AWS RDS (PostgreSQL)", "Stores agency dataset metadata")
                ContainerDb(datastore_db, "DataStore database", "AWS RDS (PostgreSQL)", "Stores JSON records of dataset resources uploaded by agency users")
                ContainerDb(inventory_s3, "Inventory filestore", "S3", "Stores agency uploaded open data resources (PDF, CSV, XSLX, etc)")
            }
        }
    }
}
System_Ext(Login.gov, "Login.gov", "Authentication As a Service")
Boundary(gsa_saas, "GSA-authorized SaaS") {
    System_Ext(dap, "DAP", "Analytics collection")
    System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
personnel -> dap : **reports usage** \n//[https (443)]//
Rel(inventory_app, newrelic, "reports telemetry", "tcp (443)")
Rel(inventory_app, datapusher_app, "queues datastore upload", "http (80)")
Rel(datapusher_app, inventory_app, "inserts data for datastore", "https (443)")
Rel(personnel, aws_alb, "publish open data and manage metadata", "https GET/POST (443)")
Rel(harvester, aws_alb, "ingest metadata", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, inventory_app, "proxies requests", "https GET/POST (443)")
inventory_app <-> Login.gov : **authenticates** \n//[SAML 2.0]//
'Rel(inventory_app, Login.gov, "authenticates", "SAML 2.0")
Rel(personnel, Login.gov, "verify identity", "https GET/POST (443)")
Rel(inventory_app, inventory_db, "reads/writes dataset metadata", "psql (5432)")
Rel(inventory_app, datastore_db, "reads/writes JSON dataset records", "psql (5432)")
Rel(inventory_app, inventory_s3, "reads/writes dataset resources", "https (443)")
Rel(datapusher_app, datapusher_db, "reads/writes job tasks", "psql (5432)")
Boundary(solrb, "Solr Service Boundary") {
    ContainerDb(solr, "Solr", "indexed search provider")
}
Rel(inventory_app, solr, "loads indexes, runs searches")
@enduml
```

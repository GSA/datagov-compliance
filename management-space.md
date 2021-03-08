![PlantUML diagram of management app interactions](out/management-space/data.gov%20management%20space%20interactions.svg)

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Deployment.puml
LAYOUT_WITH_LEGEND()
title data.gov management space interactions
note as EncryptionNote
  All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Deployment_Node(cloudgov, "cloud.gov", "Cloud Foundry PaaS") {
    System_Ext(cloudgov_loggregator, "cloud.gov logging system", "Loggregator")
    System_Ext(cloudgov_logs, "logs.fr.cloud.gov", "ELK")
    Boundary(atob, "ATO boundary") {
        Deployment_Node(organization, "data.gov organization") {
            Boundary(ms_staging_boundary, "management testing area") {
                Deployment_Node(ms_space, "management-staging space") {
                    System(ms_dumper, "backup service", "data.gov component")
                    System(ms_drain, "log shipper", "Fluent Bit")
                    ContainerDb(ms_es_logs, "Log Storage", "AWS ES", "Stores, monitors, and alerts on logs")
                    ContainerDb(ms_services, "stand-in services", "AWS RDS, S3, etc", "Stores (stand-in) data for apps")
                    ContainerDb(ms_backups, "stand-in backup repository", "AWS S3", "Stores backups of stand-in services' persistent data")
                }
            }
            Boundary(ms_prod_boundary, "live spaces") {
                Deployment_Node(management_space, "management space") {
                    System(management_dumper, "backup service", "data.gov component")
                    ContainerDb(manangement_backups, "backup repository", "AWS S3", "Stores backups of target services' persistent data")
                    System(management_drain, "log shipper", "Fluent Bit")
                    ContainerDb(es_logs, "Log Storage", "AWS ES", "Stores, monitors, and alerts on logs")
                }
                Deployment_Node(target_space, "target space") {
                    System_Ext(target_app, "application", "data.gov component")
                    ContainerDb_Ext(target_services, "target services", "AWS RDS, S3, etc", "Stores persistent data for apps")
                }
            }
        }
    }
}

' app->service usage
Rel(target_app, target_services, "reads/writes data", "data protocols")

' Backups flow
Rel(ms_dumper, ms_backups, "reads/writes backups", "S3 protocol")
Rel(ms_dumper, ms_services, "make/restore backups", "data protocols")
Rel(management_dumper, manangement_backups, "reads/writes backups", "S3 protocol")
Rel(management_dumper, target_services, "make/restore backups", "data protocols")

' Logging
Rel_Up(management_dumper, cloudgov_loggregator, "drain logs", "stdout/stderr")
Rel_Up(target_app, cloudgov_loggregator, "log", "stdout/stderr")
Rel_Up(ms_dumper, cloudgov_loggregator, "log", "stdout/stderr")

' The drains themselves also log, but this just gets confusing if we show it
'Rel_Up(management_drain, cloudgov_loggregator, "log", "stdout/stderr")
'Rel_Up(ms_drain, cloudgov_loggregator, "log", "stdout/stderr")

'Log draining
Rel_Down(cloudgov_loggregator, management_drain, "drain logs")
Rel_Down(cloudgov_loggregator, ms_drain, "drain logs")
Rel_Right(cloudgov_loggregator, cloudgov_logs, "drain logs")

'Log shipping
Rel(management_drain, es_logs, "ship logs", "stdout/stderr")
Rel(ms_drain, ms_es_logs, "ship logs", "stdout/stderr")
@enduml
```

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
    System_Ext(cloudgov_logdrain, "logs.fr.cloud.gov", "ELK")

    Boundary(atob, "ATO boundary") {
        Deployment_Node(organization, "data.gov organization") {
            Boundary(ms_staging_boundary, "management testing area") {
                Deployment_Node(ms_space, "management-staging space") {
                    System(ms_app, "management application", "data.gov component")
                    System_Ext(ms_test_app, "application", "data.gov component")
                    ContainerDb(ms_services, "production stand-in", "AWS RDS, S3, etc", "Stores (stand-in) persistent data for apps")
                    ContainerDb(ms_replica_services, "staging stand-in", "AWS RDS, S3, etc", "Stores replica of (stand-in) persistent data for apps")
                    ContainerDb(ms_backups, "backup stand-in", "AWS S3", "Stores backups of production stand-in's data")
                }
            }
            Boundary(ms_prod_boundary, "live spaces") {
                Deployment_Node(staging_space, "staging space") {
                    System_Ext(staging_app, "application", "data.gov component")
                    ContainerDb(staging_services, "cloud.gov staging services", "AWS RDS, S3, etc", "Stores replica of persistent data for apps")
                }
                Deployment_Node(management_space, "management space") {
                    System(management_app, "management application", "data.gov component")
                    ContainerDb(manangement_backups, "backup repository", "AWS S3", "Stores backups of production apps' persistent data")
                }
                Deployment_Node(production_space, "production space") {
                    System_Ext(production_app, "application", "data.gov component")
                    ContainerDb(production_services, "cloud.gov production services", "AWS RDS, S3, etc", "Stores persistent data for apps")
                }
            }
        }
    }
}

' Staging flow
Rel(ms_app, ms_replica_services, "restore backups", "data protocols")
Rel(ms_app, ms_backups, "reads/writes backups", "S3 protocol")
Rel(ms_app, ms_services, "make/restore backups", "data protocols")
Rel(ms_app, ms_test_app, "monitors app environment", "CF API")
Rel(ms_app, cloudgov_logdrain, "log events", "stdout/stderr")


' Backups flow
Rel(staging_app, staging_services, "reads/writes data", "data protocols")
Rel(management_app, manangement_backups, "reads/writes backups", "S3 protocol")
Rel(management_app, production_services, "make/restore backups", "data protocols")
Rel(management_app, staging_services, "restore backups", "data protocols")
Rel(production_app, production_services, "reads/writes data", "data protocols")

' Logs and monitoring flow
Rel(management_app, cloudgov_logdrain, "log events", "stdout/stderr")
Rel(management_app, staging_app, "monitors app environment", "CF API")
Rel(management_app, production_app, "monitors app environment", "CF API")
@enduml
```

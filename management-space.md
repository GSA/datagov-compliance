![PlantUML diagram of management app interactions](http://www.plantuml.com/plantuml/png/jLHHJzim47wcl-BM5uh46ctiQK-Kjcne7I1IXFPKkV4rM3YsPJljkcd_VRus9AOA1QJD3wdlVFVTT_vyFNMUMLyMilDEg4oM7E7UU-C-9ubbgtuk_7qvBntQJ2kFolSpNIICMy7KKfQcJ8QVtbzBflpjzqaYcKVdWsisGYFrkg04G-2JmO-hs-dixcBwTJOPVnbVZdf77I-yHE3Citwkbr0mnNAa7726PGWYLBGiyq8hrr7Q8p07OvNPjI7VPV1q00PIGY2dS1i878t8F79W5W6tqS5QXKvWEadXG_yZb4gYSw3zFTgLS0Y4pliTL1mKmHuq3SmkDSTU9dN90yjZw6wsnBWRZ24PSAvBnUqQhXbBkqVmkmFLIjVEOp4R__Grmapgd5icLCWEMzTVsFuJoF7aM_UeHXXgvKCmsj6yvppBXSfd3krIPEXYCb0TmkCXGGtkKhWPfSUGdXm3-enygTSM7HYqJZYITAC0BBG5PenxfZI9exRgnENdB7ie3LWqsea0lAwgXe9HhMsK0xq0OpKlDqfjgXtkydYcV9FMtdnp_jh-A-P4OW-7CnAMsNMFUJqdmEaLp2jV8tIrTj-CjZbJuXSZMY6tVkYnlmRP1rJhJcCHa6gfoTuW7D1g7bkT4TeByU4hHRQj5nC9isoq2cMgZkFrvqLMpRpMn1fNExSefw8MPVmVbl7BgLWshZqt6ENzcqiaAzxHzn3Egj5OIBtgtA1ygbpBm5jat2KhApowxMkjoD0CU9rfIIDCIBjt7OzmEs0zgOIPdjI8pm2s3sN17Z2nw6ZCOhoNEROt_5geNS4Y-ytNTmWJdJjWYaEXbGZr0wbAenRMRN_jLPu3-OY4ov2muUyyrwLFmWzQk_yco8ZHWWcebi9gHR64DZo7mVN5ongz0UaK5IyB-HS0)

```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/latest/C4_Deployment.puml
LAYOUT_WITH_LEGEND()
title data.gov management space interactions
note as EncryptionNote
  All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
	Deployment_Node(cloudgov, "cloud.gov", "Cloud Foundry PaaS") {
        System_Ext(cloudgov_logdrain, "logs.fr.cloud.gov", "ELK")
        ContainerDb(staging_services, "cloud.gov staging services", "AWS RDS, S3, etc", "Stores persistent data for apps")
        ContainerDb(manangement_services, "backup repository", "AWS S3", "Stores backups of production apps' persistent data")
        ContainerDb(production_services, "cloud.gov production services", "AWS RDS, S3, etc", "Stores backup apps persistent data")
        Boundary(atob, "ATO boundary") {
            Deployment_Node(organization, "data.gov organization") {
                Deployment_Node(staging_space, "staging space") {
					System_Ext(staging_app, "application", "data.gov component")
                }
                Deployment_Node(management_space, "management space") {
					System(management_app, "management application", "data.gov component")
                }
                Deployment_Node(production_space, "production space") {
					System_Ext(production_app, "application", "data.gov component")
                }
            }
        }
    }
' Backups flow
Rel(staging_app, staging_services, "reads/writes data", "data protocols")
Rel(management_app, manangement_services, "reads/writes backups", "S3 protocol")
Rel(management_app, production_services, "make/restore backups", "data protocols")
Rel(management_app, staging_services, "restore backups", "data protocols")
Rel(production_app, production_services, "reads/writes data", "data protocols")
' Logs and monitoring flow
Rel(management_app, cloudgov_logdrain, "monitors logs and events", "stdout/stderr")
Rel(management_app, staging_app, "monitors app environment", "CF API")
Rel(management_app, production_app, "monitors app environment", "CF API")
@enduml
```

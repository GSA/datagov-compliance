
![SMTP Service boundary view](../out/smtp.boundary/SMTP%20Service%20boundary%20view.svg)

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title SMTP Service boundary view
Boundary(client, "Client") {
  Person(client_team, "Client Team")
  System_Ext(osbapi_client, "OSBAPI Client", "Service orchestration")
  System_Ext(service_client, "Service Client", "Use provisioned services")
  Rel(osbapi_client, service_client, "provide service credentials", "any")
  Rel(client_team, osbapi_client, "request service instance for service client", "any")
}
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS us-west-2") {
    Boundary(iaas, "SSB managed boundary") {
        System(aws_ses, "AWS SES", "SMTP email send-only service")
    }
}
Boundary(aws_govcloud, "AWS GovCloud") {
    Boundary(cloudgov, "cloud.gov") {
        System_Ext(aws_cg_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "SSB application boundary") {
            System(ssb_app, "SMTP broker", "Open Service Broker API, Go+Terraform")
            ContainerDb(ssb_db, "Broker State", "MySQL", "Store state of provisioned instances")
        }
    }
}
Rel(ssb_app, aws_ses, "provisions", "AWS API")
Rel(osbapi_client, aws_cg_alb, "broker service instances (OSBAPI)", "https GET/POST (443)")
Rel(aws_cg_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, ssb_app, "proxies requests", "https GET/POST (443)")
Rel(ssb_app, ssb_db, "store and read state", " port (3306)")
Rel(service_client, aws_ses, "send mail", "SMTP+STARTTLS (587)")
@enduml
```
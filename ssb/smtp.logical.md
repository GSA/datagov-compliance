## SMTP Service Logical View

![SMTP Service logical view](out/ssb-service-scratchpad/SMTP%20Service%20logical%20view.svg)


```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
LAYOUT_WITH_LEGEND()
title SMTP Service logical view
Boundary(client, "Client") {
  Person(client_team, "Client Team")
  System_Ext(service_client, "Service Client", "Use provisioned services")
}
Boundary(smtp_app, "SMTP service boundary") {
    System(smtp_service, "SMTP Broker", "Open Service Broker API, Go+Terraform")
    System(ses_instances, "<&layers> AWS SES Instances", "hosted mail-sending service")
}
Rel(service_client, ses_instances, "use service instance", "SMTP+STARTTLS (587)")
Rel(client_team, service_client, "provide credentials for service instance", "any")
Rel(client_team, smtp_service, "provision/configure service instances (OSBAPI)", "https GET/POST (443)")
Rel(smtp_service, ses_instances , "manages", "AWS API")
@enduml
```


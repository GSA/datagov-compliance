## Solr Service Logical View

![Solr Service logical view](out/ssb-service-scratchpad/Solr%20Service%20logical%20view.svg)


```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
LAYOUT_WITH_LEGEND()
title Solr Service logical view
Boundary(client, "Client") {
  Person(client_team, "Client Team")
  System_Ext(service_client, "Service Client", "Use provisioned services")
}
Boundary(Solr_app, "Solr service boundary") {
    System(Solr_service, "Solr Broker", "Open Service Broker API, Go+Terraform")
    System(solr_instances, "<&layers> Solr Instances", "open-source enterprise-search platform")
}
Rel(service_client, solr_instances, "use service instance", "service-dependent protocol")
Rel(client_team, service_client, "provide credentials for service instance", "any")
Rel(client_team, Solr_service, "provision/configure service instances (OSBAPI)", "https GET/POST (443)")
Rel(Solr_service, solr_instances , "manages", "AWS API")
@enduml
```

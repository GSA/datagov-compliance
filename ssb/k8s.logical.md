## K8S Service Logical View

![K8S Service logical view](out/ssb-service-scratchpad/K8S%20Service%20logical%20view.svg)


```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
LAYOUT_WITH_LEGEND()
title K8S Service logical view
Boundary(client, "Client") {
  Person(client_team, "Client Team")
  System_Ext(service_client, "Service Client", "Use provisioned services")
}
Boundary(k8s_app, "K8S service boundary") {
    System(k8s_service, "K8S Broker", "Open Service Broker API, Go+Terraform")
    System(eks_instances, "<&layers> AWS EKS Instances", "hosted kubernetes clusters")
}
Rel(service_client, eks_instances, "use service instance (kube API)", "HTTPS (443)")
Rel(client_team, service_client, "provide credentials for service instance", "any")
Rel(client_team, k8s_service, "provision/configure service instances (OSBAPI)", "https GET/POST (443)")
Rel(k8s_service, eks_instances , "manages", "AWS API")
@enduml
```

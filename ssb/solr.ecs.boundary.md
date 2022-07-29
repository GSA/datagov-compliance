# SOLR Service on ECS Boundary View

![Solr ECS Service boundary view](../out/ssb/solr.boundary/Solr%20ECS%20Service%20boundary%20view.svg)

```
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title Solr Service boundary view
Boundary(client, "Client") {
  Person(client_team, "Client Team")
  System_Ext(osbapi_client, "OSBAPI Client", "Service orchestration")
  System_Ext(solr_client, "Solr Client", "Use provisioned services")
  Rel(osbapi_client, solr_client, "provide service credentials", "any")
  Rel(client_team, osbapi_client, "request service instance for service client", "any")
}
note as EncryptionNote
  All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note

Boundary(aws_west, "AWS us-west-2") {
  Boundary(iaas, "SSB managed boundary") {
    Boundary(vpc, "VPC for Solr service") {
      System(solr_ingress, "solr-<instance>.ssb.data.gov", "AWS Load Balancer")
      System(solr_efs, "Solr-<instance>", "EFS drive")
      Boundary(b_solrcloud, "ECS") {
        Container(solr_leader, "Solr leader", "Apache Solr", "open-source distributed enterprise-search platform")
        Container(solr_followers, "Solr follower <&layers>", "Apache Solr", "open-source distributed enterprise-search platform")
        Container(solr_admin_init_service, "Solr admin init service", "Username password management for Solr", "")
      }
    }
  }
}
Boundary(aws_govcloud, "AWS GovCloud") {
  Boundary(cloudgov, "cloud.gov") {
    System_Ext(aws_cg_alb, "cloud.gov load-balancer", "AWS ALB")
    System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
    Boundary(atob, "SSB application boundary") {
      Container(solr_app, "Solr broker", "Open Service Broker API, Go, Terraform, Helm", "Brokers SolrCloud as a service for applications")
      ContainerDb(solr_app_db, "Broker State", "MySQL", "Store state of provisioned resources")
    }
  }
}
Rel(osbapi_client, aws_cg_alb, "broker service instances (OSBAPI)", "https GET/POST (443)")
Rel(aws_cg_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, solr_app, "proxies requests", "https GET/POST (443)")
Rel(solr_app, solr_app_db, "store and read state", "port (3306)")
Rel(solr_client, solr_ingress, "use SolrCloud instance", "https GET/POST (8193)")
Rel(solr_ingress, solr_leader, "proxies requests", "http POST (8983)")
Rel(solr_ingress, solr_followers, "proxies requests", "http GET (8983)")
Rel(solr_admin_init_service, solr_leader, "config initialization", "http POST (8983)")
Rel(solr_app, solr_leader, "provisions", "https GET/POST (443)")
Rel(solr_app, solr_ingress, "provisions", "https GET/POST (443)")
Rel(solr_app, solr_admin_init_service, "provisions", "https GET/POST (443)")
Rel(solr_app, solr_efs, "provisions", "https GET/POST (443)")
Rel(solr_leader, solr_followers, "distributes updates", "http POST (8983)")
Rel(solr_leader, solr_efs, "mounts", "nfs")
Rel(solr_followers, solr_efs, "mounts", "nfs")
@enduml
```

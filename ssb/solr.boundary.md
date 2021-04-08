## Solr Service Boundary View

![Solr Service boundary view](out/ssb-service-scratchpad/Solr%20Service%20boundary%20view.svg)

```plantuml
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
    Boundary(eks, "EKS service instance") {
      System_Ext(solr_operator, "Solr Operator", "manages custom SolrCloud resources")
      Boundary(k8s_ingress, "k8s ingress controller") {    
        System(solr_ingress, "<instance>.solr.ssb.data.gov", "k8s ingress")
      }
      Boundary(b_solrcloud, "SolrCloud instance") {
        ContainerDb(solr_instances, "<&layers> Solr instances", "Apache Solr", "open-source distributed enterprise-search platform")
        ContainerDb(zookeeper_instances, "<&layers> ZooKeeper instances", "Apache ZooKeeper", "distributed cluster manager")
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
Rel(solr_instances, solr_instances, "shards and routes queries", "https GET/POST (8193)")
Rel(solr_instances, zookeeper_instances, "delegates cluster management", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster configuration", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster replication", "port 2888 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "leader election", "port 3888 plain text?")
Rel(solr_app, solr_operator, "provisions SolrCloud", "Terraform (helm provider)")
Rel(solr_operator, b_solrcloud, "provisions", "https GET/POST (443)")
Rel(solr_ingress, solr_instances, "proxies requests", "http GET/POST (8193)")
@enduml
```

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
'note left of personnel : In java, every class\nextends this one.
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

![EKS service boundary view](out/ssb-service-scratchpad/EKS%20Service%20boundary%20view.svg)

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title EKS service boundary view
Boundary(client, "Client") {
  Person(client_team, "Client Team")
  System_Ext(osbapi_client, "OSBAPI Client", "Service orchestration")
  System_Ext(k8s_client, "k8s Client", "Manages cluster")
  Rel(osbapi_client, k8s_client, "provide k8s credentials", "any")
  Rel(client_team, osbapi_client, "request EKS instance for client", "any")
}
Boundary(external_services, "External Services") {
  System_Ext(docker_official_images, "Docker Official Images", "verified upstream images")
}
note as EncryptionNote
  All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws_west, "AWS us-west-2") {
  Boundary(iaas, "SSB managed boundary") {
    Boundary(eks_instance, "EKS instance") {
      Boundary(vpc, "AWS VPC") {
        System_Ext(aws_eks, "EKS control plane")
        System_Ext(aws_ecr, "AWS Elastic Container Registry (ECR)")
        Boundary(aws_fargate, "AWS Fargate") {
          Container(solr_operator, "Solr Operator", "k8s service", "manages custom SolrCloud resources")
          Container(admission_controller, "Admission Controller", "k8s service", "pulls and verifies images")
          Container(alb_ingress, "AWS Load Balancer Controller", "k8s service", "manages ALB")
          System(sys_eks_nginx_ingress, "<&layers> nginx", "Kubernetes nginx ingress controller")
          System_Ext(client_app, "<&layers> client application", "k8s service")    
        }
        Boundary(aws_public_subnet, "AWS Public Subnet") {
          System_Ext(aws_eks_alb, "EKS ALB", "application load balancer")
        }
      }
    }
  }
}
Boundary(aws_govcloud, "AWS GovCloud") {
  Boundary(cloudgov, "cloud.gov") {
      System_Ext(aws_cg_alb, "cloud.gov load-balancer", "AWS ALB")
      System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
      Boundary(atob, "SSB application boundary") {
        Container(eks_app, "EKS broker", "Open Service Broker API, Go, Terraform", "Brokers EKS as a service")
      }
	ContainerDb_Ext(eks_app_db, "Broker State", "MySQL", "Store state of provisioned resources")
    }
}
Rel(eks_app, eks_instance, "provisions", "Terraform (AWS, k8s providers)")
Rel(osbapi_client, aws_cg_alb, "broker EKS instances (OSBAPI)", "https GET/POST (443)")
Rel(aws_cg_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, eks_app, "proxies requests", "https GET/POST (443)")
Rel(eks_app, eks_app_db, "store and read state", "port (3306)")
Rel(k8s_client, aws_eks, "manipulate k8s cluster", "http GET/POST (8193)")
Rel(aws_eks_alb, alb_ingress, "proxies requests", "https GET/POST (443)")
Rel(alb_ingress, sys_eks_nginx_ingress, "proxies requests", "https GET/POST (443)")
Rel(sys_eks_nginx_ingress, client_app, "proxies requests", "https GET/POST (443)")
Rel(aws_eks, aws_fargate, "orchestrates workloads", "https GET/POST (443)")
Rel_Up(admission_controller, docker_official_images, "pulls images/verifies signatures", "https GET/POST (443)")
Rel(client_team, docker_official_images, "pushes images", "https GET/POST (443)")
Rel_Up(admission_controller, aws_ecr, "pushes verified images", "https GET/POST (443)")

' The following hidden line constrains the "External Services" boundary below the "Client" boundary, narrowing the 
' diagram
k8s_client -[hidden]- docker_official_images
@enduml
```

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
    }
    ContainerDb_Ext(solr_app_db, "Broker State", "MySQL", "Store state of provisioned resources")
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

![Solr Service Logical View](http://www.plantuml.com/plantuml/png/RPD1ZzfE3CNlV0h_z_0lg40EvRIggWMAA18jg0bQLQeKpIO6fftCH6DFTb7Lxrw795IKlO77_Zs_PtWWLfv3ONh_QPkPa2CScKkwZoAldiS7pSVm50XzvYoZvN7cYaZbNYjRcL26Q3uPROsolFkyZenY99PIEg-xNtXSrkJrzDjcbwIFg-HJkbui5ry-zWSzrcmGOcSynEWhdI4OTz2PCb1fVExDNB2vygT-PhJe3k5kVlvmDu1VFO0jUdAsVKmPLN7fW4I-tGsaAJuHOv4kNhXFZKPweNMYBLKgEq8elQiqQMSnXtQ41FMxzyOwHZ2uPt1xAC_g5WtSmyh23rMf8M25_WIPKKJVhPX7cnFmKHC8RlpNKEqvLMKfabVsfya6djgcrcLdePbf5-hcPjxzH5zllYdHTfYs3DFjQWXBzpP1xzNU-IQH2v1geBR4oef5ORp_twWJUlhGt6JLFTLy9_mHkU05ZvRHbruJZWYLpuvG6iMDWEJq5SrDrZTQWR1B5xfwhTFMHZcAO2wpzJtOPSu8MyZNvxuvwVbuEKBciHxMoX38OgzAANjwZVbFpfUVGvGvkzU7u6yN9-Xlufd4FQYvvpyFB1T9jDt42VGdatU3LkYQVHqAo6YXh3eqWKyVOn2Y33v84A4mVm00)
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
Boundary(Solr_app, "Solr Service ATO boundary") {
    System(Solr_service, "Solr Broker", "Open Service Broker API, Go+Terraform")
    System(solr_instances, "<&layers> Solr Instances", "open-source enterprise-search platform")
}
Rel(service_client, solr_instances, "use service instance", "service-dependent protocol")
Rel(client_team, service_client, "provide credentials for service instance", "any")
Rel(client_team, Solr_service, "provision/configure service instances (OSBAPI)", "https GET/POST (443)")
Rel(Solr_service, solr_instances , "manages", "AWS API")
@enduml
```

![Solr Service Network/Boundary view](http://www.plantuml.com/plantuml/png/hPVVRnkt3y2Vwx-2tpvyvm3viJJ5a0t3LYTrqw9kayqkYkt5qEbeMwWitIHT76_8_pvHbdLM56C8sZUTH7v8aHIbUsKTCwvTojx_XEAohH4MpZNsfvCJmrR7S-4MRTLQD5mhXyeTSxqyQIHJf7DaNIr5TNBvykY6fZvz69qicNLe_DJqqcimeT0SDyJ_3bhbbPSU0cw1CDDIwfLGSv1U29Yg8LiMnlgH1cySfEPCohLd92zB0wF17zUV9jFFxoPlfwFXrV3Zw_v1pmad4SPQ6XYZkHKSeTAjgfbPmwt0LUyYVlMv5Dxs8Jo_38Fd1_1F3-06ZTKgBauTicMIW8d_UdxWXSPhwt0v7TwvlhOLQyGqmQx75uERTn2PVcBhXJPyWTOPveHM3obs8vGmIQlZVB88ZT6tmdfzh26gs83w7UL3JmfaKAvnkm3SOErN19EM-4oj4ohR_SCT6lohHUiIHoZhcEAKODF1WtJ7lUyfxH2OXQ7YPjrG43xwcTv0Ik1QAUGqPQ76Hd27DJ23W46MlbQ-Bc4o6iFfyGjebKHhGRi5cfMm2CIkZtkewZ3iSinMrhim-3o6AtrxANLRnpG391d160czOsoSWWg3oNMgcwJI9Op0KwkbcJ9PUULGRuFHHTXmqqZ1Gvf1QbP3nIJ5nuJm0cJ6YODVoFxm_PW2uOoMGESDWzbdfLsI9xDHPwZgHWlbWldEmXwz9LDiZeOC_ln_oTP--0iG9Yw0qZNQ7SxzupxFc9apXp6uRy9Nz3Wlx7MCaveBTJSLQcxGvjR32WNkVLkXKUZGRkOWIc_38Yc4n4-UL87DAQyn2I77gIIx4ARMyRhg1t-s8halX4ZQuLWC6iON61ReKpUeZgnk3KUeXNL6L2tL9og7fZ72uf55PlY2SkXcsb33A3puM-ilY0tkS-DFhT_J-kE-f5MQstM2ovPwSSnb3DRzWrJUvzKVWZVNjnuLXiT-7CAsfuZuFDPUaWv5VfIAF9P4hCO2aWnEZVPEcspV7NEp67QzIUaRSjYimHascmc-FQcqnwumdIRFikPV7EHdEwdeXwFKDF4eLKP_sUpWka6L41TX6dnRFuGhVGWJD8PHQaboisY1z9a5rZcMMwAYYwR8loVREeItA3EBgTmpkw4BxxIW_2m_swb2oaPoQrfN2GrZnnoIgG_hyMyZ6eoTDWYMva7Fidl8uEOuaA42J_7zLlHOlVSzkg5Iuhe6cjGiCLFWe4_b62y_S-0vfD-b8--ecwDZmUQD80TsZIhdbfTmTc8sAI_kJGlzpQFXWCo4nnXS3ISdDzVZ2VHVlZpRidTXvIdodjm93ulNi_qFM0584VrgKehjar79XwnkR2WFfgWQM0rsMp-DDWxwPsVV_r1cDOoUJYWUOW_k-TPYaR_jddQsT7xwutPFnMrJt3vsmKnjWtSXWXPyh8n0cy24sqDx_2QfKUAS4Ig_4kYndVRzulJyb6ufeS3XdVirMYY9UyriuLohcPYtyMdxxVa6KplhwEVdvrz7byXg3qU9F2EVFKxE-aJnd2fBFQgLajdpAJMSdPwoqeOQV57JMIq-iT-LRkHl9yiNMBUIEfgEnPZUmfwwbxBtVPU0IRLpVh_QlYZkaIyEKwc__t2-GbNJR-I_)

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
  System_Ext(service_client, "Service Client", "Use provisioned services")
  Rel(osbapi_client, service_client, "provide service credentials", "any")
  Rel(client_team, osbapi_client, "request service instance for service client", "any")
}
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS GovCloud") {
    Boundary(iaas, "IaaS services ATO boundary") {
    System_Ext(aws_solr_alb, "Solr ALB", "application load balancer")
        Boundary(aws_eks, "EKS control plane") {
	    System_Ext(aws_eks_alb, "EKS Endpoint ALB")
            System_Ext(aws_eks_managers, "<&layers> EKS manager nodes")
        }
        Boundary(aws_fargate, "AWS Fargate") {
	  System_Ext(sys_solr_nginx_ingress, "<&layers> nginx", "Kubernetes nginx ingress controller")
	  Boundary(b_solrcloud, "SolrCloud instance") {
	    ContainerDb(solr_instances, "<&layers> Solr instances", "Apache Solr", "open-source distributed enterprise-search platform")
	    ContainerDb(zookeeper_instances, "<&layers> ZooKeeper instances", "Apache ZooKeeper", "distributed cluster manager")
	  }
        }
    }
    Boundary(cloudgov, "cloud.gov") {
        System_Ext(aws_cg_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
	Boundary(atob, "Solr Service ATO boundary") {
	  Container(eks_app, "EKS broker", "Open Service Broker API, Go, Terraform", "Brokers EKS as a service")
	  Container(solr_app, "Solr broker", "Open Service Broker API, Go, Terraform, Helm", "Brokers SolrCloud as a service for applications")
        }
	ContainerDb_Ext(solr_app_db, "Broker State", "MySQL", "Store state of provisioned resources")
	ContainerDb_Ext(eks_app_db, "Broker State", "MySQL", "Store state of provisioned resources")
    }
}
Rel(eks_app, aws_eks, "provisions", "Terraform (AWS provider)")
Rel(solr_app, aws_eks_alb, "manages solr instances", "Terraform (Kubernetes provider)")
Rel(osbapi_client, aws_cg_alb, "broker service instances (OSBAPI)", "https GET/POST (443)")
Rel(aws_cg_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, eks_app, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, solr_app, "proxies requests", "https GET/POST (443)")
Rel(eks_app, eks_app_db, "store and read state", "port (3306)")
Rel(solr_app, solr_app_db, "store and read state", "port (3306)")
Rel(service_client, aws_solr_alb, "use service instance", "http GET/POST (8193)")
Rel(solr_instances, solr_instances, "shards and routes queries", "http 8193")
Rel(solr_instances, zookeeper_instances, "delegates cluster management", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster configuration", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster replication", "port 2888 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "leader election", "port 3888 plain text?")
Rel(aws_eks_alb, aws_eks_managers, "proxies requests")
Rel(aws_eks_managers, aws_fargate, "manages Kubernetes workers")
Rel(solr_app, aws_solr_alb, "provisions", "Terraform (AWS provider)")
Rel(aws_eks_managers, b_solrcloud, "schedules pods and services for")
Rel(aws_eks_managers, sys_solr_nginx_ingress, "schedules")
Rel(aws_solr_alb, sys_solr_nginx_ingress, "proxies requests")
Rel(sys_solr_nginx_ingress, solr_instances, "proxies requests", "http GET/POST (8193)")
@enduml
```

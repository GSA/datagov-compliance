![Solr Service Logical View](http://www.plantuml.com/plantuml/png/RPD1ZzfE3CNlV0h_z_0lg40EvRIggWMAA18jg0bQLQeKpIO6fftCH6DFTb7Lxrw795IKlO77_Zs_PtWWLfv3ONh_QPkPa2CScKkwZoAldiS7pSVm50XzvYoZvN7cYaZbNYjRcL26Q3uPROsolFkyZenY99PIEg-xNtXSrkJrzDjcbwIFg-HJkbui5ry-zWSzrcmGOcSynEWhdI4OTz2PCb1fVExDNB2vygT-PhJe3k5kVlvmDu1VFO0jUdAsVKmPLN7fW4I-tGsaAJuHOv4kNhXFZKPweNMYBLKgEq8elQiqQMSnXtQ41FMxzyOwHZ2uPt1xAC_g5WtSmyh23rMf8M25_WIPKKJVhPX7cnFmKHC8RlpNKEqvLMKfabVsfya6djgcrcLdePbf5-hcPjxzH5zllYdHTfYs3DFjQWXBzpP1xzNU-IQH2v1geBR4oef5ORp_twWJUlhGt6JLFTLy9_mHkU05ZvRHbruJZWYLpuvG6iMDWEJq5SrDrZTQWR1B5xfwhTFMHZcAO2wpzJtOPSu8MyZNvxuvwVbuEKBciHxMoX38OgzAANjwZVbFpfUVGvGvkzU7u6yN9-Xlufd4FQYvvpyFB1T9jDt42VGdatU3LkYQVHqAo6YXh3eqWKyVOn2Y33v84A4mVm00)
```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/latest/C4_Context.puml
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

![Solr Service Network/Boundary view](http://www.plantuml.com/plantuml/png/hPVVRnf74CUVmrynpKCBIWrouafkLRN13d6ikB5LYABs1Irt0woyx5vdzanevV-zDyknnt91LUI-BRSxd_antvjRNhDNvCkLwNoZRMRA767fVS4_3GQarls5zijoLZ9Ivgn7w_kPMmqAewpOdB3FZPuDBizExkJHnzzk1Yl57gbwDBsiB9IsIFr2-Dz1QIlZLGK1lqIOEsFSMji566qHbCqXsTR4rSf1oGZ6PSgOJSM8KROTt0p_kFqucNwwdhoRteokHX_UT7iThxr163j3C4Pwq1d2p9KsLxI11uthpaNzgviPZTP_3yylm-9v3_xf0DmXiRFrvjIZMiKJCAb-FUzLXyORzhYQZZxxhkEPAlGqmcx75yExQwYPrODT58wo9R8dvRMpXnJU7egOQDLmFZ92GUv1SsMFETGc752_epcCf8KCnZdkDY0ZpD5whGmBNzbDH2NP7sP8-5U9x2D7M_RAPj9WQk3XTCDzx5Zd4HJ3o6QqAQG87vp7pj0Oo9orcCaZXXmBdNdCGH42XhFoQwtz4YOtOpZjluJI6cG6vvT8Qyq8mixx7RHvM3OzLckkGXX-6iELUxWqhiphDWF4CrefENIjr3WM5OQJswYRQD8qJC1JTeQcoimguv-_DMg3nBzibJUykGYf5uNHMMWu6AToc2aZbQ9GQ80a327YlKGoUZ-MadXo1kJDmn30itO4Sbu2g6r6DY-SjZwuRpmSiLifgnP8d4Glc7e3hCkHzpYFNuvvhcYXFDPbVXj-nOZZoLceLYOT47LNwz2DA9wTGNp5tyoweRwx0zokSjoIMhi2xGcxaZA4NBCdFIj5E6WzKa6QyOHHKRQKalgve-fD5eUfox-TkqSiy9ZVFvrx7_PJv_iECrEoHzgLSUlbyQ2AZwa4GrqMxg52XMM_MeUA7EbVjgZR7ay7VPr4VTNTg8NGWaI7KtAbHqfoR9ZRpP3Xjbjl9M3QW2Srd-jizxf8Zeqcl9F8aWcSlasfcBj1n4LHYtX6xdwRm6s1DX8kmcEGqNhbNaoGICKUHbHGJCqI_ry9U_4EpIeH_BCzRKZTeeDfBZdMdB4NnOjFxmY1vIUuUJAj2KMROLotgLA1fqAt0JzsP7p7YZOp9PfnhGt9lZAJuqtLqfcoLJ03jrwsz-KCoQ97ZfZMzoVHwROHhKy6GtVxlUm9DTn3u6eq6TpTZYVGFJjxrQlP-x2MTYMvpnePwYyJ_mUi1OZrUZ8fblAhKJ66H0mScg-iz5hbm3jH58uyT7yuU_MorsfYM7qzeNK7EVZ4bOoj_krosalf_FJ7c5DAICVvyGeTentOjtcyL9Hpo3NqWw7YaiO6BBWZj2zF_XmDBfIWqh4kjzPOnPUdvwVoLT4MF7xslzOUMiJZRdRmpDcvNfJr7V7_vnF62qb3Fpy_VnhTeCgH00rc2VdLbyd9a6dVHjgogCrQ9vFRHvnMUxDfxUWUwHWWLNXhHkvnP5nUsmKXy-vYPf2EOjEx3cTBp4k338KBwaokxg_Hvl9Vvby0)

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
    System_Ext(aws_solr_alb, "<&layers> Solr ALB", "application load balancer")
        Boundary(aws_eks, "EKS control plane") {
	    System_Ext(aws_eks_alb, "EKS Endpoint ALB")
            System_Ext(aws_eks_managers, "<&layers> EKS manager nodes")
        }
        Boundary(aws_fargate, "AWS Fargate") {
	  Boundary(b_solrcloud, "SolrCloud instance") {
	  ContainerDb(solr_instances, "<&layers> Solr instances", "open-source distributed enterprise-search platform")
	    ContainerDb(zookeeper_instances, "<&layers> ZooKeeper instances", "distributed cluster manager")
	  }
        }
    }
    Boundary(cloudgov, "cloud.gov") {
        System_Ext(aws_cg_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
	Boundary(atob, "Solr Service ATO boundary") {
	    System(eks_app, "EKS broker", "Open Service Broker API Go+Terraform")
	    System(solr_app, "Solr broker", "Open Service Broker API Go+Terraform+Helm")
        }
	ContainerDb_Ext(solr_app_db, "Broker State", "Store state of provisioned resources")
	ContainerDb_Ext(eks_app_db, "Broker State", "Store state of provisioned resources")
    }
}
Rel(eks_app, aws_eks, "provisions", "AWS API")
Rel(solr_app, aws_eks_alb, "manages solr instances", "Kubernetes API")
Rel(osbapi_client, aws_cg_alb, "broker service instances (OSBAPI)", "https GET/POST (443)")
Rel(aws_cg_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, eks_app, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, solr_app, "proxies requests", "https GET/POST (443)")
Rel(eks_app, eks_app_db, "store and read state", "port (5432)")
Rel(solr_app, solr_app_db, "store and read state", "port (5432)")
Rel(service_client, aws_solr_alb, "use service instance", "http GET/POST (8193)")
Rel(aws_solr_alb, solr_instances, "proxies requests", "http GET/POST (8193)")
Rel(solr_instances, solr_instances, "shards and routes queries", "http 8193")
Rel(solr_instances, zookeeper_instances, "delegates cluster management", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster configuration", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster replication", "port 2888 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "leader election", "port 3888 plain text?")
Rel(aws_eks_alb, aws_eks_managers, "proxies requests")
Rel(aws_eks_managers, aws_fargate, "manages Kubernetes workers")
Rel(aws_eks_managers, aws_solr_alb, "provisions", "Kubernetes AWS Ingress controller")
Rel(aws_eks_managers, b_solrcloud, "schedules pods and services")
@enduml
```

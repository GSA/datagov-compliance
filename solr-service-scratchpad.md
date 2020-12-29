![Solr Service Logical View](http://www.plantuml.com/plantuml/png/RPD1Zzem48Nl-HNJFRIW0ZcKqwgg5YYYIBIW9cXLgL9aaW7SEdRacR2Bgltl7KD2HTaRZFs-D_CcVY1MdklIH6-qpKrT8EoPAxgFOw-U1ZlD-tfJ4_hSMKRBWzoLSMMK3Pe-SM7q9fuC-wjGMdzTnAKYHY-bR18Kppoe0dqn-h5SfzdZFFsIBQQpwRVFdMx4cWr2ueo71Fr1vmZ6xNIk31mqFaLZLzj2-MCdDrgyUt0tEVsuwyAV2625dfnj3ZD6LLvkG2h_xhfoAJaIOvbDdxb3Pu_iGcjD6wfKreHGUNVGf9t50Xe92UebUcST8nZSChWjL2TtYmRkOMxXbpge7k01_H5oeuX-MXaURK70UqqWkC6_WSBCcQegiRmQVvGkOTDSQgPiHpXhcePQtTYxt-X3vyiAROjfoZ1QpNimS-zJz5vjdJydSW5I06fBhAoCA8oFRuqwegTFvvtCswF0TyBlawkzuD4o-ifhmZwXylaUAgEuCNY9lgEvoVh6goPiquMs7doQMhz0CIn46_R1BdT6s4A-NlVDIa_BAn1oZq6ibI6GnbwrKlRu6lE_d2-VGvmxkzMxsjysJj1P9cE9knkufmS4isaQhvP92ftXy4EtCRfcNuS28YsLLRjpuAF718GemWS98hoXlm00)
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

![EKS service network/boundary View](http://www.plantuml.com/plantuml/png/bLLHRnit37wkVmNNXyq14djjWg4RXg5EwcP5tIRRkIZsTD3TqOuGMRg9kZZ6aFy-IYVBTtQyeKy-qVm-yYCfwYqvOLspKeFlf2vLKo7SELVJB-En5UlHKhgxfcW8RMcqG-r6fLcDQoMqnvoHgvGinbSNPxVUzENJRBmIvD2oARzYX90QxQZs_3z0encyOX9mTmWBevHPIxq4nKuWT0MzlwKb_ZB0mK6PKYYrOOwKvM60sUIlco_p_Ek7-U_vR7ez_VnkU39mqYc4wSSCChGFiaGeJAChOJVm874zk8odOQaaXpw5brVXu-K9_3C0k4LBHiS_SuTYbJnWpgUN9-oKRLZoAfy-kg6XGjGoJsGtsUNazWD4JZPiip2sl4DoLZXfz3xB_HlQKV2XW_yaj5WY0HV1rpaW_qIr7xZ7K5lp82lqHYWjLcoLGf6d4tgJA7eYzuLO_Bj1Sg6IKfCJklGjjD1wxBYU1jeu144mrQNTr5xVPxPma8bIK1gjiVH6WWfhMJgiG5W43Dx-jEQPW_aiWrUZrz1eXKHWU2JiMXA2Pwz60zHL-1piEYZMn5bClcPmRHwkb6cgs4I0v2E5y4vPTWchKCaAgEq8mMH-auOZ8djOlATyAvqvkfNekFSQoIdb7YPK4H6JsMMeLLqhMORcWpAYWa8e9eeDFSOJEVnLj4Q1luRO0NGhaI-4NGg7iIBlmodbkG-8phasbKT4RrWRUuyMl9Nse03fzeKCu_2mBiR_-hqI6xJqMnnSOBDNFTcfzaUgiPGBYQn-4-UeemRWQN3urTDODmNpvTGK6brKUXjia0NRCRsjiAtSwTNhhjG-A3LYI9kscNefzIDFmj8YKKzj-CSh-zWKQ3a-Kck3wBrjdVADzjJ7rQRVf_u4btwmb-Q1euJF4N__r_YLopXzoJiCt5aQk5Wt7indPoy5pArf7DgUu1rd-oTvjd3ruBrFs6x0MR5Oo79xpJZ6YrqFdIdYRUoCnk55VD6Pjd0DUPxYNIYikMz5tDIeqvA_36RWtNiAr-OKvcWv3sDNthFzay3Z1O7evBPhmOiKyLqHwX3ZvbMHA21pmc5OovliZrdOxyvO1F9sC0i8gvTO55PWaKnZIt-LOcEV1dxl9a5x2oPXGrMJ0XXokyA6XxZOBPqmvJElGA__RQLw2vnWs3vG9pv2UFVXUZeVtzvaSnXUN9nlURj4X_F0QJnA9ejl1FqFsG51BC2tCoLWhpSKMY2qhxYeWBOzgerrC3m___6dYE-zixiLkn9QrerYb2zoUcbZAfrCthpwEQMIu7khTFVE8sstS5oeUy3-JkhXU44zXsdxSMmjVMCjZz3qzleXvT6Ql4LTDIlrBm00)

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
note as EncryptionNote
  All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note

Boundary(aws, "AWS GovCloud") {
    Boundary(iaas, "SSB managed services ATO boundary") {
      Boundary(eks_instance, "EKS instance") {
        System_Ext(aws_eks_alb, "EKS ALB", "application load balancer")
        System_Ext(aws_eks, "EKS control plane")
        Boundary(aws_fargate, "AWS Fargate") {
          Boundary(fargate_node, "Fargate worker nodes") {
            Container_Ext(client_app, "<&layers> Client app", "Application", "specified by client")
          }
        }
        Boundary(aws_public_subnet, "AWS Public Subnet") {
          Boundary(worker_node, "EC2 worker node") {
            System(sys_eks_nginx_ingress, "<&layers> nginx", "Kubernetes nginx ingress controller")    
          }
        }
      }
    }
    Boundary(cloudgov, "cloud.gov") {
        System_Ext(aws_cg_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
	Boundary(atob, "SSB Application ATO boundary") {
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
Rel(aws_eks, fargate_node, "orchestrates Fargate nodes")
Rel(aws_eks, worker_node, "orchestrates EC2 nodes")
Rel(aws_eks_alb, sys_eks_nginx_ingress, "proxies requests", "https GET/POST (443)")
Rel(sys_eks_nginx_ingress, client_app, "proxies requests", "http GET/POST (8193)")
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

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

![EKS service network/boundary View](http://www.plantuml.com/plantuml/svg/dLPHRnit37wkVmMNXyq6MdjTYw4RXg6nusP1tSRhkIlsTD3TqOuGdNGJTNQC8VzzaYphBATUa3wTH94VoO-KU6_9C6tQMlI-ux8KROLmOqn3luv6ccs6Asvksg8brAMI1gKPbgeUDO99P_E2J2LuCPgyVZ5teiyVPgEQaK5jHVd4MZ0kKGyRX_y3jD8QrnO4p0t2KWcXDbokG5WbOBA2v9XhiYi5rZa8LJ8XjXOZHldGmUpyx-lFY_pBrUAFV3Qzd7wyw0zwXXk1C7sV0Q5UynAXKAsicDx2ckEcDmwxVYcuTVqSpYP-SJQ0Vti0SzIaP3ZC3R8wQi32xiu6LYdRai4wdzwPlgA2DJoFODVP-7n-1G7J2h8GXTBb3PBHp70bZr5ktr07OJS7zX-OP2iai2Gudhtb9nJ7ZXE4Hgirhz09eTHOMIbdWXmSazi8aIHvd8Z6VreauvdaaWoJfIkXXfr6XtNVKOft1hLa8W_KarMQ1XbaGUQ8JhElL7cBEbVB9I-vjUOrMtdR2ty0r-40hlo1Sxr6pPSSAsWRCjec03iZ7uzK1e4HJ6MfjutZ-wELM9_dGa2ffCJI2GagR7XfB0hJ2EYrtMvZxm0iPXcy7Fu4hHH81CgsgDvmGd3erR27ilBBNfS-sxYWpxzaSAdM4w7QAkGA47KuOqufoyPG-yfMiJ-3SgAEjvJlsRTcXyM8wed1kYb337_D9ubAGhaDDRVO0T9TTQq4k6jkSV_7fDG1VIeO6Lv2l9AserQSZDv2VphvD4XG3XdAbqolcC60yswtEueKEjm-AQ5prQ1cHcb7dTt3TTYVXViI6ivjw_s5QSce6fowhmLe9DLgtu4dNB6gvaHSoJmm8j3dknV398g_uhXfXI3_h8NkfD2Kf_s9mbPtfP4eq35Jh88n4wx0-gIx9C_psJWuI8lMfwslSIvNNDuT-FdjUy6sgEbty2SEwtrRe9Pea7Oo2DhGyP3uI1eZF2EiQHBmdHYiMF3Ilth7IGmSq07g_QcMQTh2OkJK5ZR6mDJSoo3pijW-frizjsI7TdUCsUL1P20SxqNWFQOQWafNuNkVtklIjTXAhIskNmxjsaTr8g9o5GAAsZw65p66a6PNtHGaEiorQWtgbFg8kJiaXxQx1EzSm7eBHZFtgdQrw3thA3Ug26_K0KVfE_Nii8yzlKqJw2sqkjqbO6-fXFq077insEdu72xLSrYWjZ4eNJlDtI61isS4x22kZlvdqUD5uJa8VlEgY12G6MRG3yvjzkVCJs2ZD08vEQWb-E7eRZC-V1BkxVHmap4cTFJ-HblFQCm0-hPKVWP36BsQt0lubJcTr7x7L39Y2VgxNuY1y-3_pE1okXZDhxC5z5-_VhN7FGGwxWKVw1stO66AqoDW3m020Kz7YeP9RSYNW4d7EAk0zZLgb3RGV_Ngnv-3VV8dr0sfcadUjC9QEPBZlr08vI2IDoz_YQ4a5pzzQf-OL6ASFAlVZda29dbCdmWP-Kh7RFVdQQ4sIj-wX-Onh0GYZFumuy9OUmJXn30zyQ-N8e_Y02M-aioq-jky9O6J9G42SFLu2czHLcqj_WC0)

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

Boundary(aws, "AWS GovCloud") {
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
Rel(aws_fargate, aws_ecr, "pulls images", "https GET/POST (443)")
Rel(admission_controller, docker_official_images, "pulls images/verifies signatures", "https GET/POST (443)")
Rel(admission_controller, aws_ecr, "pushes verified images", "https GET/POST (443)")
@enduml
```

![Solr Service Network/Boundary view](http://www.plantuml.com/plantuml/png/jLNVRzis47wE_ufR7ZO7IEob2GPl6BewgPSMTPjiSb1iBm8bNMmY5Adng3ZUaFzzHycYfSX5K0nxiadUVVVTZ-_qXfomhipLu1kfKrLc22ld2lffFBPYFLfAjogJaj2cHZlKRfIQV5meeRtF2RbCoMHyUNvouwzkFyx7kI27bg_YI_OGKgCT5Hx_EoWrE-SC0cw5S6UKCckfbw3O28JEeFCiBV4_0nmSb4c5KXl622px0URJFwvl5_7dzujtyNnsDVlqTdWqSD8fXCWe2n7Q1vaY9ARKcR0RU92u7bniJyDKIOvz38UNrP_38_Xd0723bepUFiOEHHuiOC6dmoCsYZRaC8zdZsveA165Z0FOTNGnlNaFMqo-Q5WOcwwGd1LE6lqSXPZl3iEpRo7S4a9XpOCaziGCg4Qa2kHtLCyvTC4gpmmRBqWjPlmYXI8FBlGcu7IIVfwOnRzAvXzmf2OdTEhxQiDbRRt3VHfeun04mKodTbFut3_n3GUSAWMfqHfJVqcGOI5JXna8Yu2rDP_MF92mc4TmEde5fLP818RdnQub8NZqR3H0dLL_1xlkYZKnY-dd2AxCmwKoPRPjC42maK9ueoYwW5neiMpLDWnDy6hvuRrtcts8UiLeMVVxQmgqmXdRTFXwU_RbgkCJf2KvauD_hqY3HJAbRRhzZCZzX68M5DjuGdo2xGcyUgrH2gsdv9tOkqki9hLp_xb9ulN8luo8ab4cd1WjpSDX1xxDvQdFAeczV-hfirj8PN-H08AgtoODfTgk8lMj4XkqzBgMT7ZoXAQ5O4bL3_ve2jGdTR4Wa-IiJ4e_GQWTsi9AmXD2mIg4GWd7K-krl9_6tyRS8nRu9Iv_6lF1l-yd55xzNPj9gag_AxV3PZjLRFy-TSUqgYGtWT6glrL3zW-Q7_iuNSP29MrhK4Pa9ubGdYhJgeKndLymXJv821XRmxnj9_KTPlrO9LxtzrTFs6x0MN5t9zD671pZO2TBPvAjv4HHA9bMwx2hjeDMC-g9ODD6D8arztKAB1yTTln5TGsyUezPyCUmG6k5x_4nl4ELU_lQX5hQ4WGYI9YDsvIwCtxGcWvVea0hpf803P4J3dseZvlejtcr_9sn2EJlmTorjtXFs4-yBFSiyqv3w-HxMuTWM7zsZdpCwdCELxF5-EOwMi3m_FpiY6Dux3POlydCxb4os7RJqmjWFO3Ggg-6sdbswqfL-OJsrH8PK5FVmbW7mxEpxtze8uIQFLji9I7qjz0-IfFJ7pkSslBlhIPQ2PjHpSvNW81pjHBfQv7tRviC5Iw5X-skZHor2pLuTJevzUjCQd3uw7xPHWY8BuTfm5EZx-IojDNm_m_u5eEqTkYJoUI_eIiK6OEZmhI3VDP3xizPvuCSbDdQ3nujx14Ohb3bjO8vPEXb5wVxwGkWByz_6DZ-f7rPJNlcwmtghCpLlm00)

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

Boundary(aws, "AWS GovCloud") {
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

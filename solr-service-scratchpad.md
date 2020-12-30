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

![EKS service network/boundary View](http://www.plantuml.com/plantuml/svg/dLPHRzis47wEVyKj3viD9FQw14Ct34FjrCsAkaqskIZs953IsI52aHf9sJ66_FSTAPesxQPh-cJoTFVTVTyTIRyobcdRrgBt7PUbQ2k4EsiRy-jefDbwkEJshYrQWxfKqgAqmrBLeqOmwMBEZQq4BqPNb-UtplJn_MnKCsDHaoc_eWZ69UfXu_1_W5PIS4qWOEyG5ae8jUPo2OASWCaAaizS6refeEGWLCc4s116hF8umMpyzyt7UVxfxVoFV3QzddvutH_qBBS2OVek0-AmuYL2eLfPCRs15STrRn9s_L9mIdq6frT-SJg0Vti0jwYDak5ZRf7LqGFcj3iTa5Es8SfrFdsmVMKArl0yWjraa_7jMmYOPCX25KgNTsYiPfOhUOXo_zBi86Yp5_-UIRP40oI2qzb7_eNYC762q6YruYIPGoerLcJbJ1W7n-GcGYGa3ube_AUbUhsIN1hBPEbQgA7ps64zzgIo2Cp0L9PwqpX-7yX2ISP208sHnD8P3LJOyD9Y1KmZOETDkpND7Cnd6RmO_WIj56WCA1e9lUO6mQ5NmnxAoYzxkmwojQ4gnfyok5QhAw7QAZGH8FfmnfnJbasWzafMsxamC9xVnE48aKaitfjyIvqmzfNOSqyQIINb51QytH7IIe0xFkXbJpDuzmNJIsOnC7dZToa-G1pylb52vwf1pQpIZXZjuIRiJyFq18PkMs_7XwP7rU3SlLAWqQXMamonhZGH4mMHNvBRLkIPOXLCc73yjVSaRWA_b7KyctZuLAc-sNYLShdayc4los_V2xP1RNu7_yKXlMiBr18jciu6mHlAc3_D47kn7M_MD0bqPmOo2rxwqtX8OE208kRZvzlMj0N5vwOjgBgWqAstGUPjeOLF3qbE4eT1SJhHSgyc44xj8gXD59DoukzZEksbQ-rIhGZBBuUq_jAebijGHFJsUSzZta0izl4092RCjMejwbJeYDbzD0wj6xuthc2z0QlPOi7BdVAzavt8LXNXv8vtkXmVsfFzgVMoDasGjT3glYD19qJ2zYgUU3FGFNq6r-eCvgYf3gLhvzbzDE3YcG4MQzjlmKdC-BhmEeIyULL42CWii-Yly4tsvymjCgiqWd5sK0lmrxGXKdXy71_fNdLtT2HqS1d5MAzgP01zQfT_3I0y0jeC2F8pBqRI_qwfvB8tqEyUiu7BuFyZmFLqFhgzoURGlxoysEBk0ntF0vNnm0ail2VcVy2E0880pqUAWKblZ6y1aqvnLe7PzgXHsaB_ukB7dqDyyYRlhlMQITwqWgASoF5L3gNiLVBonI-nbEJ0fnVhCqabmUar-isOJy1qt9-hVKBuwH9t_uRmwx7If--RC4Y_vrVn2cNLrk8_)

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
        System_Ext(aws_eks, "EKS control plane")
        Boundary(aws_fargate, "AWS Fargate") {
          Container(solr_operator, "Solr Operator", "k8s service", "manages custom SolrCloud resources")
          Container(alb_ingress, "AWS Load Balancer Controller", "k8s service", "manages ALB")
          System(sys_eks_nginx_ingress, "<&layers> nginx", "Kubernetes nginx ingress controller")
          System_Ext(client_app, "<&layers> client application", "k8s service")    
        }
        Boundary(aws_public_subnet, "AWS Public Subnet") {
          System_Ext(aws_eks_alb, "EKS ALB", "application load balancer")
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
Rel(aws_eks_alb, alb_ingress, "proxies requests", "https GET/POST (443)")
Rel(alb_ingress, sys_eks_nginx_ingress, "proxies requests", "https GET/POST (443)")
Rel(sys_eks_nginx_ingress, client_app, "proxies requests", "https GET/POST (443)")
Rel(aws_eks, sys_eks_nginx_ingress, "orchestrates", "https GET/POST (443)")
Rel(aws_eks, solr_operator, "orchestrates", "https GET/POST (443)")
Rel(aws_eks, alb_ingress, "orchestrates", "https GET/POST (443)")
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
    Boundary(iaas, "SSB managed services boundary") {
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

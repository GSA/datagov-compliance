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

![Solr Service Network/Boundary view](http://www.plantuml.com/plantuml/png/hPPVRoCr4C3Vyoc6Uu1KN1Fr3r9122xjvKfrvLgngKxmidBiIMhLiHVRcpIWVdV6hjVRxIOW16zUU-OtVsOytdVEC-lhbHfy8JLNjK2uzxvotut7bcr6I-dlwtdjq7AZFMe_ucOrPi9AfjUghkhnnUdXhMBQt_ryFLunvz7ILdb1qangjACgiB-2MfFYYW3Wxn4MHYcpaNe9YeI0QG6TOsaThGoGOL26CwMsnCWUzWrSJtwzkPkLdwzcFvNNqylff_V3Wu6NNY4KHbaeqAubHvYRMWjcjx2Mk1cSfwyXLv9iluKt5t7nvW3-703SedL6fyFI8rjb2PZHrvi34YgspkEgd3xweN5pLiaomswAyydj5IGcRJHU6CllqNdBl3JwDSKz2sLCrcevTmwXicOj7UcZWAJY8keNLAyzwI6Zii3c0BX5GIUIAHVuJ6ypgXFzwmWj_bwZyvaZjVDCyr1WsyAZTCjz6cZZ4PY3gUPsMuKaVAATmKGfu4Phv67BWS1ASey2c4N0A1k-DjIJCBiku6Xq3BLMw1mOQXkxaGuXiCLeW5h4PLjZjd7amkHp0PTcVQ5CBLAP0RACP2m8NJ5MvAJ2P7QJ-oQh07IAHl1omUoIUIoPcXCZjjta-Zp6NLLAybXjK8O9c3CLqcGf6tjW-E0IQFgne4I9oaZjatp7voIT9CCrjKP1HGQmzNQ_aHNJR8asw7__fM9RMlu0WPGEG1k1xeNXfzqkfEXJYZ_4htqE39rHjcoQfMiwvksgEGhPCnNgGsTgoqCNUBILbGuF7JAwHY5GJzr6bx5ZAC-7z_FX7yOy85Qup-HlndoCvrsxGZflvRmEFSTL7KPRavHUEfwwVSH3UotDcYXnEQBrtxKENwPYP-dO9uUvJr9QgPzsjamsM5f3_jfEU2tp-J065oy0V0WEsoruonOBoPkUtzbctWG7EzEqVpFwfGuA9QkgHlVSceVdW6wehWqgRGFDoxTqFx-UeRKibxNDTB-uaI-2QmbH-D1umPmt5i65Jp2Bpi3CVP3mJpIKmh1hdUtUmUUokrI6u2KfxXYqdLhEep-zsUXW-Fmm70HSV6pXSZeRtzuKCnYUdfuS9FPBMB--5CwZ95WQmUuVO3r03lLVerhDDlSkvfffGH0cm3KrWCfO3yDlJa-EathlVUkFpjfXBskDJozSEZlwDllKWtGdpBuWzmEx-ZjuCOyE26ObjhJ0s8FOFOO4AWnkkrSpPfNUp9Z0uwEpep3ff0QFZ_x7PA57t6-cWNEZ5t9Pfr-F_vzlCJzrBVtixEo_qHKoGN1Ko3laaztapkNjFtIzNiXgNSbERxPJ03R6qiKEMkzGY_3x-HS0)

```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/master/C4_Container.puml
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
        System_Ext(aws_fargate_alb, "Solr ALB", "application load balancer")
        System_Ext(aws_eks_alb, "EKS Endpoint")
        Boundary(aws_eks, "EKS control plane") {
            System_Ext(aws_eks_managers, "<&layers> EKS manager nodes")
        }
        Boundary(aws_fargate, "AWS Fargate") {
            System(solr_instances, "<&layers> Solr Instances", "open-source enterprise-search platform")
            ContainerDb(zookeeper_instances, "<&layers> ZooKeeper Instances", "distributed cluster manager")
        }
    }
    Boundary(cloudgov, "cloud.gov") {
        System_Ext(aws_cg_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "Solr Service ATO boundary") {
            System(Solr_app, "Solr Broker", "Open Service Broker API, Go+Terraform")
        }
        ContainerDb(Solr_db, "Broker State", "Store state of provisioned instances")
    }
}
Rel(Solr_app, aws_eks_alb, "manages", "AWS API")
Rel(osbapi_client, aws_cg_alb, "broker service instances (OSBAPI)", "https GET/POST (443)")
Rel(aws_cg_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, Solr_app, "proxies requests", "https GET/POST (443)")
Rel(Solr_app, Solr_db, "store and read state", " port (5432)")
Rel(service_client, aws_fargate_alb, "use service instance", "http GET/POST (8193)")
Rel(aws_fargate_alb, solr_instances, "proxies requests", "http GET/POST (8193)")
Rel(solr_instances, solr_instances, "routes queries", "http 8193")
Rel(solr_instances, zookeeper_instances, "delegates cluster management", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster configuration", "port 2181 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "cluster replication", "port 2888 plain text?")
Rel(zookeeper_instances, zookeeper_instances, "leader election", "port 3888 plain text?")
Rel(aws_eks_alb, aws_eks_managers, "proxies requests")
Rel(aws_eks_managers, aws_fargate, "manages workers")
@enduml
```

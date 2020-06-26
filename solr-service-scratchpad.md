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

![Solr Service Network/Boundary view](http://www.plantuml.com/plantuml/png/bPLHRo8t4CUVZqynpKCBwW7gNVfILTMH74sZqYCgHAS-hOmz2TOPUpip2uUgVFVQkuiNiWzLUVAEPtx-pup7V61H9DNE3RwnNhlA86n5IlvfEYLrc3nRsLQRYf5qy89U9ZhifigGLNxlghAQtbwF7vpoylZ7OhfJB4ZHLDn6RsKzqgHCxE-WyZ5m5m4WMuIdu5muMFyCBZg1yWOkjYrnN0Me6C45hPmxHaPMs3zWCVjh-RWkFj-lVoiMyxlvfu_3qK2iE8HLS0GhfBtL29jGUQFe27kBXy5D-pNKpgANjt1rMo-kHl3F0E01YODlDmj1jSiUi8vVLwFej3gouAwOVvLXu8qgRP5XozNDxE4UMcOqd5G4qbja8IKs-DSKRfmo9aTrd4T6A2diBSTuDD26S8tw4zrh9Jra7Mpmj06QqCGTgnmdllB7ZBh8_dM6X7zNo98vrhCehrE3AHiRxuxxCl1141J3t6iwbga8dw9bC7CETF0UTJ8n62oj5ZIW204Rt_XriBA5zM85FqpUGUKTCaEGBTB1Ca9Ycya0lQcNNO_LWQE4sUSLt8NzhGkLQTiCSEvJu9UJt_ptLUtofkdEa8EZufHYSlhvMwUEIFnBSydkJrifrr2Y7tEeI2VjWbIIPHmpgjXyA9sIMAFc2W6yNChGITrps4TClPp4THRxfhihjLxbDkT-u88ouuroIGbTjQdC5ZVfgFRN4V9H1OLAa2wIwfZDPfrNNJduDKcb8mYffoUhJxtEPvpNL48IU366i_KoZs9ExFpNLZm55QeiJ_4t5BuqIIrBz1dNc24EszlOtE_NIAJEgjjLEFtoo_5nq_1DajSYLg84wr6LG0YSFY4yNKnRlW6vUI-3D2cTsEuQxPHNpyXj6vB26DIVKEWQkQc5z2QAOTWy9wF4gfzek9klfm_BrHg6rzVlHoqua_gTZKbyjP7KJYt_1wa7o0d-RrGNsLMS9L3pz1Cg0togF9I118O_Nhz_ruRtdiJU85QClOebLciR6opHcnYR6Yb11nV97w8f_NVy2m00)

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
    Boundary(aws_eks, "AWS EKS") {
	System(solr_instances, "<&layers> Solr Instances", "open-source enterprise-search platform")
    }
    Boundary(cloudgov, "cloud.gov") {
    	System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "Solr Service ATO boundary") {
                System(Solr_app, "Solr Broker", "Open Service Broker API, Go+Terraform")
        }
        ContainerDb(Solr_db, "Broker State", "Store state of provisioned instances")
    }
}
Rel(Solr_app, aws_eks, "manages", "AWS API")
Rel(osbapi_client, aws_alb, "broker service instances (OSBAPI)", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, Solr_app, "proxies requests", "https GET/POST (443)")
Rel(Solr_app, Solr_db, "store and read state", " port (5432)")
Rel(service_client, solr_instances, "use service instance", "service-dependent protocol")
@enduml
```

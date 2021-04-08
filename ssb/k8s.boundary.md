## K8S Service Boundary View

![K8S service boundary view](../out/ssb/k8s.boundary/K8S%20Service%20boundary%20view.svg)

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title K8S service boundary view
Boundary(client, "Client") {
  Person(client_team, "Client Team")
  System_Ext(osbapi_client, "OSBAPI Client", "Service orchestration")
  System_Ext(k8s_client, "k8s Client", "Manages cluster")
  Rel(osbapi_client, k8s_client, "provide k8s credentials", "any")
  Rel(client_team, osbapi_client, "request EKS instance for client", "any")
}
Boundary(external_services, "External Services") {
  System_Ext(docker_notary, "Docker Hub Notary ", "Store trust information")
  System_Ext(docker_official_images, "Docker Official Images", "verified upstream images")
  System_Ext(github_container_registry, "GitHub Container Registry", "bespoke images")
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
      	ContainerDb(eks_app_db, "Broker State", "MySQL", "Store state of provisioned resources")
      }
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
Rel_Up(admission_controller, docker_official_images, "pulls images", "https GET/POST (443)")
Rel_Up(admission_controller, github_container_registry, "pulls images", "https GET/POST (443)")
Rel_Up(admission_controller, docker_notary, "verifies images", "https GET/POST (443)")
Rel(client_team, github_container_registry, "pushes images", "https GET/POST (443)")
Rel(client_team, docker_notary, "pushes trust information", "https GET/POST (443)")
Rel_Up(admission_controller, aws_ecr, "pushes verified images", "https GET/POST (443)")

' The following hidden line constrains the "External Services" boundary below the "Client" boundary, narrowing the 
' diagram
k8s_client -[hidden]- docker_official_images
@enduml
```

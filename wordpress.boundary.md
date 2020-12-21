WordPress boundary view
![www.data.gov boundary view](http://www.plantuml.com/plantuml/png/fLLjRzis4Fuy_ufRFgnEq4XRd6v0KHHLNfOMi1ElSk4Dss2SnRD2Z29LahBg3VtlEqgsBCS3YcBwa53IFI_dvtn-upnQNnLgyAtKcQe4mQFtfNiPnnRhA9V-iLfMZcncj2VjeymKCGehKQzLLLRnzUNvLA7s7oRZk43doVAZnJLNezHaep9mFuDACx1W0l2F12kZbAcbpa5n4Q0MSF1QMiSd0omCocIer8OvEeV70kFajuSFiyNytUpjOdnxTtj_CpmTUEaLGLtNaK2FKMxMi3IL5cWti9PK3wPadT73ilcdIJs7avjTvNJoSCBt2HHKBCc2MJNMHF_zwPPXSVl93yjggMJ6a6bp2E390RGjO90sdW0TtEhCRaelZRxd9uD4ASY2ZomySY2eb9ad0MW9gAtbkveZWTauXH-Y2wYq8kV0CB-jfIC8t28Qa1RDSN2rRNY8jGljp5EuC-jhPIfnSWh_382lhYOBZxavBcoEeT6kwfjqupmLJQzCja2rxDU1CYZEbwXGPsHFjbh9-8eRXksrP-Ya5jPKdYoNl_fEuOQiUmrxplQb2soDPVWvMENulCNLIcRWoAvbHZsDVSFU18DTOCdieOk_Q-g9imxCGzF7ljDhqjuS0vkhcyK0Ms1P7dGpdyyXAKkE7aEkePVfsody55r6e-SmDrQKDcJu8hg8VcpUyYshETX-vG3PjZ-SljAQt6BHzTrUdmV7fyxSpRAn9vP1mZYVMqf_6KCO-2KwsYNs_YODnosiPqABS5x9lB9D8x0oDfZgjU2a9v0QMESSox9pT-p2ZRPowGYMLVON-IVwgJTC2SwpLWOhgGZGEVAk9UK6Fmzwu_GmkM8G_uL9ycj3LR4rpTZMQ-808Us6fSivTxXmY8uHTsboZemoLlvD0bB4j4cxF_S2GxOtoRIHqAWsh10-8wNQRorpz-irrPRQhN1FDRodtM8mMdeMqlbMXxlfjX2SlmOMWfTmTcQfDDOxg1pcT7O6V-Wu_hrPq32yl1oT_Xd7WtQlV2sAlUp7jUTpX_Mag21lDy6lpyeMoKuPsD-O-rLGeCRyU4P1QfwKeXl-riVTxIoUFgIpGzgcapudJvohYlz5sf4Thnmc-oJ9WQMF5Jdlli1qHD1zVb_3j0N1AmwC1xO93FVJIcuNM9fCnd0HVH_IUdOGLpljVL0Zk0CSXzG2rcJbQWDI19JVVA7RJgtR6fPGkBYsalNQd-Kc5qSUB6N6YiP5wJug6Bwu75ty9vCRFMKA7BirSqpmXhIe2lKl)

```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title www.data.gov boundary view
Person(personnel, "Data.gov PMO", "A member of the data.gov PMO")
Person_Ext(public, "Public", "Member of the public")
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS GovCloud") {
    Boundary(cloudgov, "cloud.gov") {
    	System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(www, "data.gov Inventory") {
                Container(www_app, "<&layers> WWW Application", "PHP 7.4.3, Wordpress 5.2.6", "Presents content about the data.gov program")
            }
        }
        ContainerDb(www_db, "PostgreSQL Database", "AWS RDS", "Contains content and configuration for the Wordpress site in www-app")
        ContainerDb(www_s3, "AWS S3 bucket", "AWS RDS", "Stores static file assets")
    }
}
System_Ext(OMB, "OMB MAX", "Authentication As a Service")
Boundary(gsa_saas, "GSA-authorized SaaS") {
	System_Ext(dap, "DAP", "Analytics collection")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
personnel -> dap : **reports usage** \n//[https (443)]//
public -> dap : **reports usage** \n//[https (443)]//
Rel(www_app, newrelic, "reports telemetry", "tcp (443)")
Rel(personnel, aws_alb, "manage data.gov program information", "https GET/POST (443)")
Rel(public, aws_alb, "consume data.gov program information", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, www_app, "proxies requests", "https GET/POST (443)")
www_app <-> OMB : **authenticates** \n//[SAML 2.0]//
'Rel(www_app, OMB, "authenticates", "SAML 2.0")
Rel(personnel, OMB, "verify identity", "https GET/POST (443)")
Rel(www_app, www_db, "reads/writes local dataset records", "psql (5432)")
Rel(www_app, www_s3, "reads/writes data content", "psql (5432)")
@enduml
```

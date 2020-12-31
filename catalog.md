Catalog boundary view
![catalog.data.gov boundary view](http://www.plantuml.com/plantuml/png/bLPRRnit4tt-z2lcyqDh6xQsjLmW288WwqkTehAjU1MaHLi853cIs5BaXiELl2totplSoteL8JMo3nRDdNFcCcT6UaD1-52ipU3_safJA8HL23czJ18ljiEb3gjYNX1wwMn06uRIhHEXl1PsOugyI2xFJoT6sF3-Tfoi1GNqV3MxP6kXBVfX7hc_XS8oSCq445O82sUCssgx1CD68Ao2dTVQ4vySi6CmJWfZIkRe8jntC4v_lNy_dNtuQVfsDhw-kRwxEZmQ11qCWXH16BSSAluSBjq6vgwmIlWIDXgtWmbwSdPs_HWEy-feqPp0GRf4AqkOj5S7yGuMgD0B0xZEZIiHarWLBsHm_k1eXwgO6oqPCwaE4NsBwpbwS8ignTg0GTO5145mRQKlyw2TlUERGMeCoEXPnYi2XRcM0HK8Zu2rBV-tvVx0T9p1zyCphh518d3CxxUQ42At6WxGgkeuk6WoFnHRYlbyoE36RIwDAzJ14Vmp07uw6nclkLviM1rZxJgh_sKb1Lp7N2FPJ9XvtmwC4-fqBbWO4lr1uoiTNt32q3nF39shcNT5GC_chxuneaHFh-69itv9aQqA6NwCeN8VWnUBXPP0w3TQOi_7KyB1nG0x3QJJ-qu7NLAVHTQ16mNryPVrrHuqFfqqM-1Cv7a_fnODQPwp24JiSCngKeQLip0QlXYEJk3ov_GEpeOlgZSU2MqW440el5m1sgMsSP0yh36861W8pYW0XuVULhFaShHGnGoPNA4g358rRBwmn3BaYYvNpFcnm0AZCbV2Rv127Cm_tPnD77XSiFCLVqhd5KMhlwpRrho4WUfoTy-dmVwfgyZLlAk9ciUy78Mbn-pT6AxuViw4hLWUhh9uVEkCYg7YCm5ysH6DcEO1bIPuntb-sG454R2PpxsIrFM8vOvlr-m4HAzxCOIOyQT1JzRtjnViWF_2RVfBvRPWi0qD2DBOpwmJRQUt9OaP2K6CkCdIKy4ev_NVg20J8giqrvy_9QAwhj99vS8AKx870kcCGTb4rxUtkFLORwSxtC83jWlAMHsSv-OtVZYRRYN2wMjWH_0IZeyzviu7WeB44e-FuNUR9BzLtnfmU7u-EleZIGRrVljQ5CUoEpIzM5jyG8CiUL_6c8FCQpH7o-3-2dzQIsjXsMMbbwwTv0elaI977SFDzJIPt6VJNRegYpvNCtvnY1IBCEwvxbkX6xTdM3ks_RsNU_UeaS0Z2v32S_7j4VIAztLiFI2yugv5rSQkYIV98hLTozBRCQ-cxvgMzQjUQtw3NYzAq2f2G_bS5lsWT_Q3Hw4esNhDpkiV0ktKj-iWKkVqqS3X3-UZi_zafD5dZBKYcXys-qJTF98pFaQJyMSxht2n--tHtppHlZ6FhDegV4JLRcvkoeOBu-DyxGVROAEk26eadTJxkiOZCV0DMbMipRy0)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title catalog.data.gov boundary view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor")
Person_Ext(public, "Public", "Member of the public")
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS GovCloud") {
    Boundary(cloudgov, "cloud.gov") {
    	System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(catalog, "data.gov Catalog") {
                Container(catalog_app, "<&layers> Catalog Application", "Python 3.8.3, CKAN 2.8", "Presents a search engine for metadata about government open data. Schedules and runs through a queue of harvesting jobs to refresh records of known datasets")
            }
        }
        ContainerDb(catalog_db, "PostgreSQL Database", "AWS RDS", "Holds the records of known datasets")
        ContainerDb(catalog_s3, "Redis Queue", "AWS RDS", "Holds the state of the queue of harvest jobs for the main application")
    }
}
System_Ext(OMB, "OMB MAX", "Authentication As a Service")
Boundary(gsa_saas, "GSA-authorized SaaS") {
	System_Ext(dap, "DAP", "Analytics collection")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
personnel -> dap : **reports usage** \n//[https (443)]//
public -> dap : **reports usage** \n//[https (443)]//
Rel(catalog_app, newrelic, "reports telemetry", "tcp (443)")
Rel(personnel, aws_alb, "manage data harvest sources", "https GET/POST (443)")
Rel(public, aws_alb, "search and download federal open data", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, catalog_app, "proxies requests", "https GET/POST (443)")
catalog_app <-> OMB : **authenticates** \n//[SAML 2.0]//
Rel(personnel, OMB, "verify identity", "https GET/POST (443)")
Rel(catalog_app, catalog_db, "reads/writes local dataset records", "psql (5432)")
Rel(catalog_app, catalog_s3, "reads/writes data content", "psql (5432)")
Boundary(solrb, "Solr Service Boundary") {
  ContainerDb(solr, "Solr", "indexed search provider")
}
Rel(catalog_app, solr, "loads indexes, runs searches")
@enduml
```

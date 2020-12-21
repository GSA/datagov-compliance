dashboard.data.gov boundary view
![dashboard.data.gov boundary view](http://www.plantuml.com/plantuml/png/bLL_RoCr4FsR_HH3_u2qQdSfgK0weTEbJMXFIjjmcwD2WA9PUv8OUUqzsvkz1TrtntQIJT80vQogSkoPzzxyyEmRwz2ukfJ9bq8nMNE2fNELVPLb1fjq8TooBcfBXcdbIBcKwJ93RWIgbQohEhkvkfX8LExz_JWhqJeo_cXsuwrHA39f5R2_Xbfvnz83W5iIpBMKkX5g0T8R0IeE1zV2MB_Ju8b1QePIjXwZKtXCa8m7lpo-dywUtaxlPkFHxUXXs3jDd72IWADT5XeDJpawJ1Tw1OMk5KVJmafGaoJ9X8pLQZRww7fLNKZ1pk5a4ZSmt7h3pzwMp8c_6a19PK46z3pgNVk0De0BKgo5Ak8faYUdIQAq8q0B8yLCMpcXrOC_IGPI0Wj6B1nPu5G9veW36WAAjk5Nuoi0qt4Eb-ctK2j9reBsfAOHbY1WypGXnUCsIQutaVMmiK7fKmwtUdKZTSrFJk6l1FoA_pf35kvyKhnrt8O4TQPVvArrLCRCUCGPocBV3gH6Vb6WHCLYQYBXO7pjuuRDsY5qL3EZQqV6c__mbSIMZ7qDEyprfGreKJVy6AIQ5fp1-Lmmi6HMWj4UnovgfuF0hj23wMDNx2wePyewvwvHzX6wmoFtiBest3dFiAeE8dkY0WPL9GN3KEaGr-Hk0j-dLsd_t2DmUhjGmf61VdgPNbu6WrlocEZ8Wg5A6sVZ6-4anSfZWdKUYi7Tr3-wcpoFbtDjIhlBILoVakDT9tbOx8dc8MttRVxJ68Rei41BstA-6-PXcpjjo6uxNAZ05mEAx6kPdly57jltKDTI5n3mEewy7ykDREafcC564eTOWp_iXe6BXN0ehGjvMRJhYxcG54bZY9_s-p_g3F1nSzG9a1pqTZIomf4zgDt6kQ4YtWMHzoZKzY0aFKw0KYiHuk4GaKGKjIzdON5c4MseRJwumDejjH5_UeSSCJyMn34qqN0mYHbIA5idc0McfLmF38-_Rw-eCRGUN0_KmBkmZshNmiAeZKHHrMPUNRm6JmEluEnissMrzVhFpk0tbMM_nWy1zAwk-gU_PrdYCPzt-Hxh5i6Hf9ASQGExOzNQFyp10D2Drzqm2IEuw-ZDLoOMV8LIyFW6TeqGwnsGrzfkHzDiyfXFEv90qK4VZvlAw8yYFgeFDLdt4j8Hm47uduqtUo2nyFv7m-Am-sBxs__V_GUSTwF1SCi9_rIoPq_U4FA3HtqEyIXhZDWRFzxI_vMj_I2Xr-z_yvs7_s_q-D2jqsRteF-jUjiKlI75wrB-3G00)
```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml

LAYOUT_WITH_LEGEND()
title dashboard.data.gov boundary view


Person_Ext(public, "Public Dashboard Viewer", "A member of the public or agency personnel")

note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note

Boundary(aws, "AWS GovCloud") {
    
    Boundary(cloudgov, "cloud.gov") {
    	System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(dashboard, "data.gov dashboard") {
                Container(dashboard_app, "<&layers> Web Application", "PHP 7.4.3, CodeIgniter 3.1.11", "Generates reports and delivers static HTML/CSS and forms")
            }
        }
        ContainerDb(dashboard_db, "MySQL Database", "AWS RDS", "Stores agency information and reports")
        ContainerDb(dashboard_s3, "Blob Store", "AWS S3", "Caches crawled data.json and digitalstrategy.json files")
    }
}

Boundary(agency, "Agency ATO boundary") { 
	System_Ext(agencysites, "<&layers> Agency websites", "Main website for the monitored agencies")
}

Boundary(gsa_saas, "GSA-authorized SaaS") { 
	System_Ext(dap, "DAP", "Analytics collection")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}

public -> dap : **reports usage** \n//[https (443)]//
Rel(dashboard_app, newrelic, "reports telemetry", "tcp (443)")


Rel(public, aws_alb, "views static content and validates data.json files", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, dashboard_app, "proxies requests", "https GET/POST (443)")
Rel_Neighbor(dashboard_app, agencysites, "crawls data.json and digitalstrategy.json files", "https GET/HEAD (443)") 
Rel(dashboard_app, dashboard_db, "reads agency info, reads/writes reports, ", "mysql (3306)")
Rel(dashboard_app, dashboard_s3, "stores crawled files", "https GET/POST(443)")
@enduml
```

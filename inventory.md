Inventory boundary view
![inventory.data.gov boundary view](http://www.plantuml.com/plantuml/png/bLLjRzis4Fuy_ufRFgn9K5jRd055K1HrNfOKSnAlSj0Dss3GujacG9CgITdHXlpttKaMBSV5qlfB28hFov7FNTxx85mebhhplJAPBYJ28eJSdoM94-lUN8L5CIqykioQW2RqChjCSYqCOxe-IAscoSLfTyHR3xV3P2byG4TRamj226NGzNBcVmM58V2II20i46PMQxjMPWwQ3e4m4dO-A-TfPO74GTjCQ5qIHtIvBp0S_77_C9vy-Z2-cGolhg_kBW-FEa45ZQ3CYdYjAtjI1D6RsnLCRM6aS2Mi5Aux8tJUcidLOpZCgwL1_HeE1dCqMGcZPkk0zs264ftGWCjSsn8nuRjn8YE1WwCsrKAu5V9r42mZOMtdSDFi7Mmteok26niGX8Shaxao3ygQEzhf3BI6Z3ralEL1OgwoW1A4Gy3wB4fOqtl1U9Z2JxqJA8n6xy7ITRgryWZCBNiTDB9QTiutTt0erfuh-vJ2jLrTQ5l8WoFujmFqYsSotYQFR9kNx3UU-YujVS0bLyra4w6dxNEWhP3TgT32P5JvHcimFAU2OVFRCaIfYRD5VNjlVz2YHEVVmPQp_kYPhR8Clx1LLq9mOZPJ6NXqAvLXIsDRSB1iCAPXCBwFYOX5FNCMmJ5DROOFpMOR7dyngblmHEJvJcMH0GPvhbKc-AMvkb4P5jP0l_UcrtyD5xyExk2azwRwuj0JneE0XzzXPXs8Eh7SiPwx9o-cMla5a4LqfkekcwC1zbtrd90IbXX4LGYq23VXXI3S74D5pdtPv9ZSC__d0hdadOAVElkhU06NqzOLo2aNONsOEqn_6y8baK-5noOX7ozJNjvOBNsbxZ2pZjPsrio4Rf5pk51M9NaCdjpylwhlVvLgV8_gG4ehF8FQ2al1PY92D30jszVV-7ZgF7LQsHxQkJAzke_Ykb8kg40JDkyE0tw3D2OuXc_knSGBuGbndGwwWb3MgNz8FXKYhSBNRaOfE6INWr4bOOGkIS6JTwthKKBSxVC6rmufV7Jw3jVmaTSClRL61H8oyuqELHKd9NJV0Gd16HmVEyoj2nuAB-PuV0n_cIJvi_gN0eUdf_sZlvEaGwpF-w2bsp04r4Z9T2NhXoolyQHCyFQKtiwR9kox2LyAGrPYo9cjzdDzDKv6z-buXpZEx3OnbKrxNqiHSVjpB7VsKQ47XzGuFlWNcFO8Tcxjs_XseF2MdYp6hteuiSqV-kRXqi7ja8RDZ_nghxxmREqqxn2maGQy_sHjs0gTcfMW96D3-SADFDD_DaeS2kcJjLFae1uGpLHe9WVJv_wpXiEVJ_idBxBw_dFMTkllayKszLOxTfJItwQDuNptFqnxEl7vpN5cLKRY8qhmA5osWDpPbPBekEs-P7Y3vZdaeSRIW725yHi6z0HzZqOMI_qV)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
' uncomment the following line and comment the first to use locally
' !include C4_Container.puml
LAYOUT_WITH_LEGEND()
title inventory.data.gov boundary view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor")
Person_Ext(harvester, "catalog Harvester", "catalog.data.gov")
note as EncryptionNote
All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note
Boundary(aws, "AWS GovCloud") {
    Boundary(cloudgov, "cloud.gov") {
    	System_Ext(aws_alb, "cloud.gov load-balancer", "AWS ALB")
        System_Ext(cloudgov_router, "<&layers> cloud.gov routers", "Cloud Foundry traffic service")
        Boundary(atob, "data.gov ATO boundary") {
            System_Boundary(inventory, "data.gov Inventory") {
                Container(inventory_app, "<&layers> Inventory Application", "Python 3.8.3, CKAN 2.8", "Presents a UX for agency users to publish government open data and add metadata. Presents a harvest target for the catalog app to query")
            }
        }
        ContainerDb(inventory_db, "PostgreSQL Database", "AWS RDS", "Holds the records of locally-uploaded datasets")
        ContainerDb(inventory_s3, "PostgreSQL Database", "AWS RDS", "Inventory DataStore: Holds data content uploaded by agency users")
    }
}
System_Ext(Login.gov, "Login.gov", "Authentication As a Service")
Boundary(gsa_saas, "GSA-authorized SaaS") {
	System_Ext(dap, "DAP", "Analytics collection")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}
personnel -> dap : **reports usage** \n//[https (443)]//
Rel(inventory_app, newrelic, "reports telemetry", "tcp (443)")
Rel(personnel, aws_alb, "publish open data and manage metadata", "https GET/POST (443)")
Rel(harvester, aws_alb, "ingest metadata", "https GET/POST (443)")
Rel(aws_alb, cloudgov_router, "proxies requests", "https GET/POST (443)")
Rel(cloudgov_router, inventory_app, "proxies requests", "https GET/POST (443)")
inventory_app <-> Login.gov : **authenticates** \n//[SAML 2.0]//
'Rel(inventory_app, Login.gov, "authenticates", "SAML 2.0")
Rel(personnel, Login.gov, "verify identity", "https GET/POST (443)")
Rel(inventory_app, inventory_db, "reads/writes local dataset records", "psql (5432)")
Rel(inventory_app, inventory_s3, "reads/writes data content", "psql (5432)")
Boundary(solrb, "Solr Service Boundary") {
  ContainerDb(solr, "Solr", "indexed search provider")
}
Rel(inventory_app, solr, "loads indexes, runs searches")
@enduml
```

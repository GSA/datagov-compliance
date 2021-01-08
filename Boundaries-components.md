
### Boundaries, components, and user interactions
Every data.gov component follows the pattern described below with variance only in the application or data services in use unless otherwise specified.
![data.gov typical application interactions](http://www.plantuml.com/plantuml/png/VLLTJ-j647s-_XNJl1GGn6elJ_UX8f0qhHeWmY1QjLMqiIVEIkjTQtTC21N_lJEsazYanI_iBdFEVCoPoLKWvQagJFITjgcfCeGrKHc-nR5Ncs6kQLqjgu0-TPRGqZ1rHQmohvLzCLLPnJUN5tEZB3tTpcAZ20FnqsACfN7RGW2ba4UpqH_tJu-BvryVVrdC9jF9tVZaD29D1Y5Jf8QvUm7QbZfL1bHP6ZwGTXOqU_KgbNE88ki8GGMOsDHlItcyuvS8O6GCS8GM6ql8aAa8Cr0U0Hjhlcqu7NYS9V33y4UehC4GmD4Q_KO7167FXX7Qh3v6KNH8Ot7dCZnHct0EWz5p0bFtScDSbGtaNgWtP-4Pbv2WVz4f1dZ0d0CPdC8_qPTa6mYBnUILX62Xp991ysh9ILwiLAfjNiT4w0jj5GckzJAQNGzEeoyVmqZ5CrUChUgZL4yWTKJmiwjivhSmLogf0u3swyInOrXuLn7wBX4qJyTyeScD0_gCqBWyyqfRXlCn35T-s8jnClkjmx0_t3XBZ48_NXwuMe-X5vx8PHTBs1NgOPoSG_Br791IUKl8UGnGeWywY6uRsCfvKLVeH70jsIc_FL7abiBsU0_BzgrNFVa-DiBvN5dzLlUCiNibTzyF99yGXLAbs6Me7nhvoDTK-eHZPwFEV0ow61xDqbdcvFJUUoxvrZkzy_-Z1pG7FHuB8g3DGcyMoQlLIjVrhNS4d5nUVZrbPypKXFPVdVEeiX1ll2OCTHTsCKFf7RdK6Mb757qFCzOBh8pR714UoGh82KsWZCED-GzwponCSbC5SWLwK2dx3z4SVN2srcXPZnp3daAZxr6EDjr2MPjODE0yq1gXCMI-ey7Z2hVbkQcFChjZb5I5xpQ1Psqp9rlYFe7VpuLmfctrEZXb3VGdPkdPhgQInN7Tt8H5sIqKxCrRU17zEbltKvAxTLQJy-mrskis3seHb99IJvFHXQfepHPlc47IBW0utY1vg3HX0u4f97Ah9zuPevREDmtV4KHx0UQvTrNPDf3xiaPJNd44k-cUEfSRXAbOoUPwxss_J0bqF9eFsgoLLMPBEWsjgvwrnOt7fc5tk847EGlmK8GTgX5p1t1OiqLZt1FoGS3JoUEnY3j-FPREKm12WmMItmgKqh87sibgNviS6IAL4Miib3OVB0-jhmAA-ji7kBZu2RX8y0tEpdQ-gw1oF3k3lsmS_zcPkx_Zk4cx1NUptlrgXkONx_zoZ-VtIRy0HskWsJYbTwyQPUe-mKTNQBEgCFy2)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Deployment.puml

LAYOUT_WITH_LEGEND()
title data.gov typical application interactions

note as EncryptionNote
  All connections depicted are encrypted with TLS 1.2 unless otherwise noted.
end note


Deployment_Node(aws, "AWS GovCloud", "Amazon Web Services Region") {
	System_Ext(aws_alb, "Public-facing TLS termination", "AWS ALB")
	Deployment_Node(cloudgov, "cloud.gov", "Cloud Foundry PaaS") {
        System_Ext(cloudgov_router, "cloud.gov router", "Cloud Foundry service")
        System_Ext(cloudgov_logdrain, "logs.fr.cloud.gov", "ELK")
        
        ContainerDb(cloudgov_services, "cloud.gov data services", "AWS RDS, S3, etc", "Stores persistent data for apps")
        Boundary(atob, "ATO boundary") {
            Deployment_Node(organization, "data.gov organization") {
                Deployment_Node(space, "data.gov space") {
					System(app, "application", "data.gov component")
}
            }
        }
    }
}

Rel(aws_alb, cloudgov_router, "sends application traffic", "https (443)")
Rel(app, cloudgov_services, "reads/writes data", "data protocols")


' Logs flow
Rel(app, cloudgov_logdrain, "logs to", "stdout/stderr")

' Customer access
Person_Ext(public, "User", "Agency personnel or the public")
Deployment_Node(computer, "Computing Device", "MS Windows, OS X, or Linux"){
    System_Ext(browser, "Web Browser", "any modern version")
}


' Monitoring
Boundary(gsa_saas, "GSA-authorized SaaS") { 
	Deployment_Node(gsuite, "GSA G Suite", "Collaboration SaaS") {
		System(ggroup, "datagovhelp@gsa.gov", "Google Group")
	}
	System_Ext(dap, "DAP", "Web analytics SaaS")
	System_Ext(newrelic, "New Relic", "Monitoring SaaS")
}

Rel(newrelic, aws_alb, "monitors application", "https GET (443)")
Rel(app, newrelic, "reports telemetry", "tcp (443)")
Rel(public, ggroup, "gets help", "email")
Rel(public, browser, "uses")
browser --> dap : **reports usage** \n//[https (443)]//

Rel(browser, aws_alb, "interacts with application", "https GET/POST (443)")
Rel(cloudgov_router, app, "proxies to", "https GET/POST (443)")

@enduml
```

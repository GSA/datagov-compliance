### Establishing a privileged session
The following interactions happen between cloud.gov systems when the data.gov team establishes an authenticated SSH session. See the Cloud Foundry documentation for additional details if necessary (https://docs.cloudfoundry.org/devguide/deploy-apps/ssh-apps.html#other-ssh-access).
![privileged session diagram](http://www.plantuml.com/plantuml/png/tLHDRzim33tNluB8hcjpNj0KxJQW2z13iQxdWP4OMu0jUVf8c_twqV8Y2SQC1iCsO2rWDBRUU-GZALuxZzQ7hgssvXaMZuxi0jxSgRhnxzj0rHbfh_m15YWxfHU03kAl2lYlPweIgaVjGRIg8Ys1PKOfqPeWBsgpPxrRIwIhinAVpq6YQxO6hOpoeaxlimnXjBUcRScE5EpLB1Q3RmOJ0hqoeuE0E84xrq1lpVCXdlDRV9c0iE_Rdp8n0rIQx02M8yjZhNJDg5I8Qhs6p_GjaFDy0gZBR4Y28-S6jU26Opw13tURzRggBEt0xCcEnNuR7QxWtOM4UwlsggMQsKEpu4O85bd15oP7i7BUeZSMob0EdfHlOeHScPlxJHKfS7urz0zMi5oAtU08Bjc6newkbiiAXLTx5eazvYV2iH_mTZG7ju-RZx0pDlDcW0yFdu2hqnl55PN4DMpT4HeG9qW-O0VH2LTSKaw5iHK_J4lTOYvA2Tv4lB7gEyclmP4T0roUfm0-M3tgIIeL1zn4iRVaIB-GJHwOn4nsk8bgTqWfnUc9EUPXAbd5536P-6tRAku7GuxJaiDcR9PWc_bpDf38OobAY_5_EXy7WX4CBEZJkIt1qwnj6AhRTQYbcu5uiloPjPBKd84Z85DasRKCdJfxYPg0vJVM-8rB-ujLnfTOM69ykS_-pRNEuLBjYnouB1htgcPsEOPbwoTmrvYd4fUkxOny2LMQ6wG5hOOBb0Uth4b9FcsnPCB92oPJ_lAYoO5EMpWP_-eNJxPwuWBAXFzi4UN4_yn2kYOjGzV-0000)
```plantuml
@startuml
box "User" #LightBlue
	entity "admin" as admin
    participant cli
end box
box "cloud.gov" #Green
	participant "cloud.gov dashboard" as cgdashboard
	participant "cloud.gov controller" as capi
	participant "cloud.gov authentication" as uaa
	participant "ssh proxy" as sshproxy
	participant "application container sshd" as stagingapp
end box

== Requesting an authenticated session using the CLI==

ref over admin, capi: 9.d Privileged User Access: The administrator authenticates with the cloud.gov API
admin -> cli : //cf ssh <appname>//
activate cli
cli -> capi : request GUID for <appname>
cli -> capi : request SSH endpoint details
cli -> uaa : request SSH client auth code
activate uaa
uaa -> sshproxy : add to authorized_users
cli <- uaa : return code
deactivate uaa
cli -> sshproxy : present SSH client auth code
activate sshproxy
sshproxy -> uaa : verify authorization to application
sshproxy -> stagingapp : establish ssh session
activate stagingapp
sshproxy <- stagingapp : present session
cli <- sshproxy : proxy session
admin <- cli : present authenticated session
admin -> stagingapp : run commands
admin -> stagingapp : terminate session
sshproxy <- stagingapp : session terminated
deactivate stagingapp
cli <- sshproxy : session terminated
deactivate sshproxy
deactivate cli

== Requesting an authenticated session using the cloud.gov dashboard==
create cgdashboard
ref over admin, capi: 9.d Privileged User Access: The administrator authenticates with the cloud.gov dashboard
admin -> cgdashboard : navigate to application
admin <- cgdashboard : show application details
admin -> cgdashboard : application instance: SSH
activate cgdashboard
cgdashboard -> capi : request GUID for <appname>
cgdashboard -> capi : request SSH endpoint details
cgdashboard -> uaa : request SSH client auth code
activate uaa
uaa -> sshproxy : add to authorized_users
cgdashboard <- uaa : return code
deactivate uaa
cgdashboard -> sshproxy : present SSH client auth code
activate sshproxy
sshproxy -> uaa : verify authorization to application
sshproxy -> stagingapp : establish ssh session
activate stagingapp
sshproxy <- stagingapp : present session
cgdashboard <- sshproxy : proxy session
admin <- cgdashboard : present authenticated session
admin -> stagingapp : run commands
admin -> stagingapp : terminate session
sshproxy <- stagingapp : session terminated
deactivate stagingapp
cgdashboard <- sshproxy : session terminated
deactivate sshproxy
deactivate cli

@enduml

```

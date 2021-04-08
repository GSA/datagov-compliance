### Establishing a privileged session
The following interactions happen between cloud.gov systems when the data.gov team establishes an authenticated SSH session. See the Cloud Foundry documentation for additional details if necessary (https://docs.cloudfoundry.org/devguide/deploy-apps/ssh-apps.html#other-ssh-access).
![privileged session diagram](../out/generic/privileged-session/privileged-session.svg)
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

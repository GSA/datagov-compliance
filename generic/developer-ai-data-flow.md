# Developer AI and DevSecOps data flows (Section 10 supplement)

Proposed flows from the request; control configuration has not been verified.

## Developer request and audit flow

![Developer AI request and audit data flow](../out/generic/developer-ai-data-flow/developer-ai-inference-flow.svg)

```plantuml
@startuml
title Developer request and audit flow
box "Team" #LightBlue
    entity "developer" as dev
    participant "Claude Code CLI on GFE" as cli
end box
participant "GSA Okta / PIV-CAC" as idp
participant "Amazon Bedrock API" as bedrock
participant "AWS CloudTrail" as cloudtrail
participant "GSA SecOps SIEM" as siem

dev -> cli : initiate request
cli -> idp : authenticate (AAL2 MFA)
cli -> bedrock : HTTPS / TLS 1.3 / TCP 443 request
bedrock -> bedrock : in-memory inference (Zero Data Retention)
bedrock --> cli : response
bedrock -> cloudtrail : API invocation audit event
cloudtrail -> siem : stream audit event
@enduml
```

## DevSecOps gate flow

![Developer code change and deployment gate](../out/generic/developer-ai-data-flow/developer-ai-deployment-gate.svg)

```plantuml
@startuml
title DevSecOps gate flow
box "Team" #LightBlue
    entity "developer" as dev
    entity "human peer reviewer" as reviewer
end box
participant "GFE / Claude Code" as gfe
participant "GitHub PR" as pr
participant "Snyk / CodeQL" as scans
participant "CI/CD" as cicd
participant "Data.gov (cloud.gov / AWS GovCloud)" as datagov

dev -> gfe : develop locally with Claude Code assistance
gfe -> pr : open pull request
pr -> scans : run automated scans
scans --> pr : report results
pr -> reviewer : request mandatory peer review
reviewer -> pr : approve
pr -> cicd : approved change
cicd -> datagov : deploy
@enduml
```

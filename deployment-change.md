### Deploying a change to production code

The following interactions make a change happen in the production environment. A narrative explanation follows the diagram.
![CI/CD process](http://www.plantuml.com/plantuml/png/nLP1Z-mc33uFlyAQ7dgQzhvw-_JKNQcgr4FLxHzWY2T1Io05C-ty-n8dLeAIZRgdTqg0ppR-FcFubaX7ohrJr_0ldFz2tP_XXzzjsz4lBgCweITB3pX7l5ly1-CPT08PBCiwKnUcnUbNeM-W-5Jgnp8HVEwl1Q-Ud-6FY8EErhT07OBfj6yHdf_LP0UNhr-XjTJbwnCCENLmZTgRRc-Pzrv0pbqY_fqnaJeLrsID7RGdIFxnhauJcWsDZSQXiK-pKz1tRPs-EbG000kIJRSetlzBX-Pzitrse0tPkoPayt52gR8VU1j7sZVmml5VVWCp-c07KmYsPgEBnBO-3MW86XpGD-YDnSGm5y80z-myHdsrpb9P01qHRY7xXY6hPBvykLncZn67CF8LGqcbJuAS1MEaK2lOcxOkHwoXMurc6X9GM2Uqowb0Q-e_aNyPgSC4KpO6qmHkiD94dMgTf21uLWPmMLTAp1u84otayv0fPtwiRtT95nVVdtZPoH7vnyb_pBnW3zYlcHwTg8FCPtjCVDZ-v4xplvh5t0BcvgHCH4suj2hvgOFwGFRsW72hseY0wm1IaSBqM0ko9O_OOFFpDRTW6_J5gSM9hDhL4XRXPdrJCalgk1OMtESBGOBl-HKJLcHtbteOOhXZJOtIZW0B0kEeYRJxqyx6UemjWlKKnbFimgD7Jx8zT0aXJ3wM-qJ-Fk77DrmfhxoohPIzSx1rCniKL-iBv5mOSDPeii6N-918joN2UCt9Vq58gHazM5MTep8XyeG6WO9OpIQL4psmqJBrnF_uqKeanQUhzWC2h_5zy9P2v9fLfsLb0omqD0xbC4qr5mQ2fL6CxaivLLQZ9nDrwYhOndb4thSq4zQkOgm2Pm-f3-yuTo3jQ3tc5baVdQEco_LMsGzwxb97YXVp9fjnnAANLJOvLdbbnUXPR34MFHU9ONS-KdrPiReLjvMqSZs9123oVgZhHjjAttrXQt4tgegows-3wnGcqVcr-RgSmZUjt-JX-Mj4zDLBRku46xMgwtlpyfCNw1JCk92RdzfmBs5s8YphO8Ad69oR7xibsMs0fL0tIsLk2Vsj_ENU_GS0)
```plantuml
@startuml
box "Team" #LightBlue
	entity "reviewer" as reviewer
	entity "author" as author
end box

'autonumber

== Preparing the change ==
author ->> github: push branch
author ->> github: start pull-request
	participant snyk
	github ->> circleci: branch available
    github ->> snyk: branch available

== Checking the change ==
    activate snyk    

par Snyk and CircleCI check the branch
    snyk -> snyk: inspect dependencies
    loop vulnerabilities are found
        github <<-- snyk: report problems
        author <<-- github: report failure
        author ->> github: push changes to branch
        github ->> snyk: changes available
        snyk -> snyk: inspect dependencies
    end
    github <<-- snyk: report success
    deactivate snyk
	
    activate circleci
    circleci -> circleci: run tests
    loop tests are failing
        github <<-- circleci: report problems
        author <<-- github: report failure
        author ->> github: push changes to branch
        github ->> circleci: changes available
        circleci -> circleci: run tests
    end
    github <<-- circleci: report success
    deactivate circleci
end

author <<-- github: report successes

== Reviewing the change ==
author ->> github: request review
create reviewer
reviewer <<-- github: notify of pull-request
reviewer ->> github: inspect  branch
loop change needed/bug identified
    reviewer ->> github: note findings
    author <<-- github: report findings
    author ->> github: push changes to branch
    reviewer <<-- github: report changes
end
reviewer ->> github: approve pull-request

alt either the author
	author ->> github: merge into deployment branch
else or the reviewer
	reviewer ->> github: merge into deployment branch
end

== Deploying the change ==
github ->> circleci: deployment branch changed

participant "application in staging" as stagingapp
create capi
circleci -> capi: push code to staging
create stagingapp
capi -> stagingapp: stage
capi -> stagingapp: start
activate stagingapp 
capi ->> stagingapp: monitor
loop
	stagingapp ->> stagingapp: handle requests
    opt 
        capi <<- stagingapp: app crash
        capi ->> stagingapp: restart
    end
end

circleci -> stagingapp: run smoke tests
alt smoke tests fail
    author <<-- circleci: report problems
else smoke tests pass
    participant "application in production" as app
    circleci -> capi: push code to production
    create app
    capi -> app: stage 
    capi -> app: start
    activate app 
    capi ->> app: monitor
    loop
        app ->> app: handle requests
        opt 
            capi <<- app: app crash
            capi ->> app: restart
        end
    end
    deactivate app
end

box "Deployment SaaS" #LightGreen
	participant github
    participant circleci
    participant snyk
end box

box "cloud.gov" #Green
	participant "cloud.gov controller" as capi
    participant stagingapp
    participant app
end box

@enduml
```

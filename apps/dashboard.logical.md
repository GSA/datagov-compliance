dashboard.data.gov logical view
![dashboard.data.gov logical view](../out/apps/dashboard.logical/dashboard.data.gov%20logical%20view.svg)
```plantuml
@startuml dashboard.data.gov logical view
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()
title dashboard.data.gov logical view

Person_Ext(public, "Public Dashboard User", "A member of the public or agency personnel")

'note left of public : In java, every class\nextends this one.

Boundary(atob, "ATO boundary") {
    System(dashboard, "data.gov dashboard", "Displays quality check information data from various agencies. Also verifies uploaded files.")
}

System_Ext(agencysites, "Agency websites", "Main website for the monitored agencies")

Rel(public, dashboard, "view agency reports, use validation forms")
Rel_Neighbor(dashboard, agencysites, "crawls open data information") 
@enduml
```

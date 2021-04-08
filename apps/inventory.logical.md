inventory.data.gov logical view
![inventory.data.gov logical view](out/inventory.logical/inventory.data.gov%20logical%20view.svg)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
LAYOUT_WITH_LEGEND()
title inventory.data.gov logical view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor")
Person_Ext(harvester, "catalog Harvester", "catalog.data.gov")
'note left of personnel : In java, every class\nextends this one.
Boundary(atob, "ATO boundary") {
    System(inventory, "Inventory.data.gov", "Publish open data and manages metadata")
}
Rel(personnel, inventory, "records of datasets, uploaded data content from agencies")
Rel(harvester, inventory, "ingest metadata", "https GET/POST (443)")
@enduml
```

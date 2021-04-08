catalog.data.gov logical view
![catalog.data.gov logical view](../out/apps/catalog.logical/catalog.data.gov%20logical%20view.svg)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
LAYOUT_WITH_LEGEND()
title catalog.data.gov logical view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor")
Person_Ext(public, "Public", "Member of the public")
'note left of personnel : In java, every class\nextends this one.
Boundary(atob, "ATO boundary") {
    System(catalog, "Catalog.data.gov", "Schedules harvests and stores records of known datasets")
}
Rel(personnel, catalog, "manages harvests settings for own agency")
Rel(public, catalog, "search, review and download open data")
@enduml
```


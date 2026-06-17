catalog.data.gov logical view
![catalog.data.gov logical view](../out/apps/catalog.logical/catalog.data.gov%20logical%20view.svg)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
LAYOUT_WITH_LEGEND()
title catalog.data.gov logical view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor data manager")
Person_Ext(admin, "Data.gov Admin", "A data.gov team member admin")
Person_Ext(public, "Public", "Member of the public")
'note left of personnel : In java, every class\nextends this one.
System(agency_catalog, "Agency Catalog", "Metadata file(s) for an agency")
Boundary(atob, "ATO boundary") {
    System(catalog, "catalog.data.gov", "Serves metadata of data sources/systems loaded from harvest.data.gov")
    System(harvest, "harvest.data.gov", "Stores metadata sources and job and record information; syncs to catalog.")
}
Rel(personnel, admin, "Requests capturing of agency catalog")
Rel(personnel, agency_catalog, "maintains harvest source endpoint")
Rel(admin, harvest, "Enters and maintains harvest sources")
Rel(public, catalog, "search, review and download open data")
Rel(harvest, catalog, "Capture metadata and store for searching/examination")
Rel(agency_catalog, harvest, "Pulls metadata")
@enduml
```


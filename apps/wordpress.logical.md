
www.data.gov logical view
![www.data.gov logical view](../out/apps/wordpress.logical/www.data.gov%20logical%20view.svg)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
LAYOUT_WITH_LEGEND()
title www.data.gov logical view
Person_Ext(personnel, "Data.gov PMO", "A member of the data.gov PMO")
Person_Ext(public, "Public", "Member of the public")
'note left of personnel : In java, every class\nextends this one.
Boundary(atob, "ATO boundary") {
    System(www, "www.data.gov", "data.gov program content")
}
Rel(personnel, www, "manages program information")
Rel(public, www, "consumes program information")
@enduml

```


inventory.data.gov logical view
![inventory.data.gov logical view](http://www.plantuml.com/plantuml/png/PP5HYnCn58NVvrTS-s8BTUR5fttQTIrQg6xHBY88vJOvdObaReRaPhQ3-D-TuBeKp5CuENmvvzxRf1mrzxvwvSJuR46Twf1kcYRoKzqwxV8f9qGJH25Qcz0tRADZ6NqUSdF_zitUi-ZZfrtZMP5qbextnNtHUYZatTttXyV3yTlsyF6ustpOV7w_N5Ngr8ESZ10DSQejAzTj6Cc7rXdsD3eyLNl45EIukUXoGBaA_9eMTot4JBJ_9osAHcTOHFQ4Vl1X0fgIERAP-OjLzLg2WZpEIk5CBpIweQtGJnvvJHWH9pAUK_eXk2Z49jBE9Gg2kdeNibYEqv8rdCgFXmSwFMkB5VsgQ3vVfwJebo_5Pj_slv8bxpwVl4iTXG52vO5OBFKit29H3-KYpi5_Lr_Wh-jVioDCY3QLIiMUe6bDUUxF5lOlzdbpT8wX9owJSqWpznPYoth-0000)
```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/latest/C4_Context.puml
LAYOUT_WITH_LEGEND()
title inventory.data.gov logical view
Person_Ext(personnel, "Agency Personnel", "A federal employee/contractor")
'note left of personnel : In java, every class\nextends this one.
Boundary(atob, "ATO boundary") {
    System(inventory, "Inventory.data.gov", "Publish open data and manages metadata")
}
Rel(personnel, inventory, "records of datasets, uploaded data content from agencies")
@enduml
```

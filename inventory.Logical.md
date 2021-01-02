inventory.data.gov logical view
![inventory.data.gov logical view](http://www.plantuml.com/plantuml/png/PP91QzjE44Vl-XJpr-KlWoiTwbDEILEJ6DnQD0gbK31ZxKZQifeLkxEAHUbtxyeaZg0-cUVTvyTFklM2Ja9lalyqroOeWavay3T5uV0bRxLquHGykTgo44jUsxv0vJJoQC8GYllDXz8Wo_ENVM5Go4j4n_lvz5doOJRlxtuSdglZzrtrUDnl7xPVFsUhHBGO0irZb5etvGe5yzQEO6ohQpGmQdf9IdBUyd5xbcouV6KoQqZlMk9wWl8DfJE3XXGvD43zOEn4LCpD3kleJrTBLOTkZA7auhKQ1UDVmkCRIz_XDIfU_v-j41Xg16m3rnYuWHt3Bnnn3JIIcw0swFrFfhCGAm_IQG-MAKy-sS0AtPIXsDCSN1tWzChI5VnE87wUfjZGPzTTuhdTFnlDYMKu6Uqxi0Cnp3y0ie8U6Lloq9FW36Fud-GRcULwIxUZshfOQPkBmvFuDOGu7ofI5mIl3nuQPtl0UNXDFdgZTJdaqggvZVGz8gBBImKFswee3qyLP9lDnrMKt1Ahq9k_)
```plantuml
@startuml
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/latest/C4_Context.puml
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

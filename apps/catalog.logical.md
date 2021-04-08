catalog.data.gov logical view
![catalog.data.gov logical view](http://www.plantuml.com/plantuml/png/NP71RjD048RlVeeX5n8fs1TEFRM422gbDAAf419IDDwTs8RrZBKpTX8XtftT5gg0JwlVy_t-F7kjXj76FXIlEdPXz0IjsQ1NLHNnM3QTjMCzAaKdRCHMEkahzB53di8uZDNgxPjjGBR7kqqLq4WjHVjLdZvPEUJcpStN-yVT_iljxkD-i_wm_lH-lYYiiq3Wq318KteqB1kP84ZJEGmmTNGijXHLUBy-sNoWV6GAIvZTDCJk3Dk_qIndS21FCP7K3q7EH5KsZkXCucpnJzLOXyubPljqoFGTzJL5a0DOI_0yaA3NB4OGw63vrOi2NC4jmtUSS0aqKJo32wZwZUban5x1sav1cChYdOpiCPxdQ59dpTqzr7-osG9-5f2UXxCQzNEFsjQ2qQUvrNzxoP8FhYK_1b9eCKwafe3iGKrYoY8vYLwpw0-M8qD6bKpJV_mgFbEuNE7bftfaR2vB4sGTDme7YP2RCEyxwUQMb-rTLYXXTEqI8kLhUxBo2Go27cIWPvV4NnFxiG-_0G00)
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


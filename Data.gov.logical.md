
www.data.gov logical view
![www.data.gov logical view](http://www.plantuml.com/plantuml/png/TP71RjGm48RlUOfXBsIbabmuzTJIhg3IBXl1LWW9APh4iucHFOxiST8LujrnIedI0pwDP_xlzypu9WXwYTPaRpIt9Yg2NcG8rsNfSIewBNriOY3VEXPYALfdIrHU8uyc3h6yU_-kCiZoUDYN1eM2f5HzDwkVf1Xcv_tjz-FZgVxsSFfKxtSVTv_lysqcMWp1D4s5Gi6YSoCOr-aM3OoQfgmY7npNkoV9X-UGoLp1vlwVk3eSD-b-2vPiGnxS6QGdKElwzfLBR8nk4r8z1pDyU8N-5IJeBJiXC7IMkRIy3jVmmF0JHxm26ibVe3KOmWzEWnAha4nq0CTKP1zSP8N-agEuPkxoT8Jc9RVPmgyCqlbw2K8sLnZwng5NIRTUnzpWCitWNUVHmiilf2P_Pr_8h5Upzh78s55OCCrdvosATfpG12xRM5F9F4JxN-o6M4Lh_W00)
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


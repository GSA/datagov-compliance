dashboard.data.gov logical view
![dashboard.data.gov logical view](http://www.plantuml.com/plantuml/png/RP51ZzCm48Nl_XL3BxJIabuuxMdBjeALkhiLj498IDKadhI6YIVOutGhn7_7QMXLfCqf7i_CU-_pNLGCcXe6Yuh5JZfWsRaA6Jf71_F6-OgJ9TYhjl5sEWruA7PEzwbCaK8bNibhKKapHHiS-evJcwRtxz-j6Brk7fTJHgMekRIzxxjVj6YpdZ7BsRVdpNhxzM7zQRjSV5mypSSJeqwPm6BSbuB15g-xCYiAKVnsyQBZDfNigXiOhKu_C3_FmYOR7EMB6JJKb1H0Qj0zmJ014W1tvAiZjATjddWqCUQj5oLWgZNtdtjluS733-pm1gYZS8IACSRlFgSXRsDUwoA8fyAO3vAynN0SeqhPgw-VeVnN6qtWju7yVJb6fMPyoPdxBaalnTxxtCMMyHZXLq9sceNtLFq4vsi93QeJ3_qWr44Qw30uIN68vIWMCECea0sxEXyXjIneoKBjED_cj7-C6QoSK0uuebEAlV41pe7AKwLtyuZEdmkG7PnmDkATIY1xqUqnVYQ-FCrrpFxzpjm3jH8qQwL8sJmxEoJgi_LByexj4xbT7WxNhFxtMGKyS0HfQM1n3IS3DtVaRMhuBm00)
```plantuml
@startuml data.gov logical view
!include https://raw.githubusercontent.com/adrianvlupu/C4-PlantUML/latest/C4_Context.puml

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

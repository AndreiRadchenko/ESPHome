# Use addressable_lambda effect for stair light
I'm have setup ESP32 with ESPHome for lightup stair. When motion detected at the bottom of the stair - light rising up, 
and when you approach to stair from the upper floor - light falling down.
Exept this, the same ESP32 board, handle motion sensor at the stair base, light relays for cabinet and bedroom main light, and sensor buttons for 
this lights toggling.

## Result Illustration

https://user-images.githubusercontent.com/25689113/148655637-32b09107-9162-4490-a45f-eb8701f7335e.mov

https://user-images.githubusercontent.com/25689113/148655689-6d87c76c-4f25-4c28-ab43-223c05c54301.mov

## Config files

To let the stair light up, get acquainted with my code :)

Config file            |  Description
-------------------------|-------------------------
[cabinet_light.yaml](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/cabinet-light.yaml) | ESPHome configuration file             
[Stair light motion activation](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/automation.yaml)  |  Corresponding Home Assistant automation

## Components
<details><summary> </summary>

ESP board and sensors that i'm used in project.

Parts           |  Description
-------------------------|-------------------------
![](https://user-images.githubusercontent.com/25689113/148658704-cd28fc58-16d5-4422-8831-bf5fc5abab7b.png) | ESP32 dev board pinout           |  
![](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/automation.yaml)  |  Corresponding Home Assistant automation

</details>


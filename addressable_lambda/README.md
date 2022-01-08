# Use addressable_lambda effect for stair light
I have set up ESP32 with ESPHome to lightup stair. When motion is detected at the bottom of the stair - light rises up, 
and when you approach to stair from the upper floor - light falls down.
Except this, the same ESP32 board handles motion sensor at the stair base, light relays for cabinet and bedroom main lights, and sensor buttons for 
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
![](https://user-images.githubusercontent.com/25689113/148659048-fd9e5a80-87e2-4306-9672-d50ae9beef7d.jpg)  |  [Sonoff motion (PIR) Sonoff SNZB-03](https://smartlight.me/smart-home-devices/zigbee-devices/pir_sensor_sonoff_snzb-03)

</details>


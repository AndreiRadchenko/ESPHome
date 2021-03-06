# Use ESPHome addressable_lambda effect for stair light
I have set up ESP32 with ESPHome to lightup stair. When motion is detected at the bottom of the stair - light rises up, 
and when you approach to stair from the upper floor - light falls down.
Except this, the same ESP32 board handles motion sensor at the stair base, light relays for cabinet and bedroom main lights, and touch sensor buttons for 
this lights toggling.

## Result illustration

Watch on youtube:

[![Watch on youtube](https://img.youtube.com/vi/obBydwlSV4Y/0.jpg)](https://youtu.be/obBydwlSV4Y)

Box with esp and relay under the stair. Motion sensor attached directly under the esp box, so no long wiring needed.
Touch sensors on the wall in cabinet and bedroom. Touch sensor module glued to switch panel.

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/images/esp_box_and_pir.jpg" width="24%"></img> <img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/images/cabinet_touch_sensor.jpg" width="24%"></img> <img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/images/bedroom_touch_sensor.jpg" width="24%"></img> <img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/images/touch_sensor.jpg" width="24%"></img> 

## Config files

The are three lamda effects in ESPHome config file:
 - The backlight lights up from top to bottom (Falling light)
 - The backlight lights up at the bottom and rise up (Rise light)
 - The backlight flashes slightly and then glows the same color constantly (Simple light)

Corresponding Home Assistant automation (Stair light motion activation) turn on backlight for 5 min, in case of motion detected, and light levels outside the window are less then 5%. Automation mode "Restart". When motion is detected at the bottom of the stairs and the backlight is off, the automation turns on the backlight with the "Rise light" effect, when motion is detected at the top and the backlight is off, the "Falling light" effect is triggered, in case the motion is detected and the backlight is running - "Simple light" effect is triggered. At the same time, repeated triggering of motion sensors is visualized by light one-time flashing of backlight.

Config file            |  Description
-------------------------|-------------------------
[cabinet_light.yaml](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/cabinet-light.yaml) | ESPHome config file           
[automation.yaml](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/automation.yaml)  |  Corresponding Home Assistant automation
[Cabinet_light.pdf](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/Cabinet_light.pdf) | Schematic diagram of project 

## Parts
<details><summary> </summary>

ESP32 board pinout, and other parts of project.

Parts           |  Description
-------------------------|-------------------------
![](https://user-images.githubusercontent.com/25689113/148658704-cd28fc58-16d5-4422-8831-bf5fc5abab7b.png) | ESP32 dev board pinout
![](https://user-images.githubusercontent.com/25689113/148826076-460cdcab-3112-4e65-b259-8a7a57372665.jpg) |  [Level converter](https://aliexpress.ru/item/4000039891923.html?gclid=CjwKCAiAz--OBhBIEiwAG1rIOt__LgE36QTgDeKzNgaGONAvyxLjPSalt-yexpLlaA8PR2bWy9AKTRoCyQIQAvD_BwE&sku_id=10000000088879366) 
![](https://user-images.githubusercontent.com/25689113/148741979-414e8d72-1d6c-4208-8d25-7135871d9eea.jpg) |  [WS2812B Individually Addressable LED Strip Light](https://smartlight.me/adressable-led-strips/adressable-led-strip-ws2812b-60led)
![](https://user-images.githubusercontent.com/25689113/148736478-b5593292-0e4d-4a8c-9f08-a343146ac247.jpg)  |  HC-SR501 motion sensor
![](https://user-images.githubusercontent.com/25689113/148739631-f663c8cd-52f4-4e50-a663-18b300b02349.jpg) |  [Sonoff motion (PIR) Sonoff SNZB-03](https://smartlight.me/smart-home-devices/zigbee-devices/pir_sensor_sonoff_snzb-03)
![](https://user-images.githubusercontent.com/25689113/149129754-0d334b7b-dcc0-420d-accf-fdd66ff94ff3.jpg) |  [Touch sensor module](https://aliexpress.ru/item/32909139021.html?item_id=32909139021&sku_id=12000024137132005&spm=a2g2w.productlist.0.0.5a054d39zrLfWk)
![](https://user-images.githubusercontent.com/25689113/149130578-cac60d1a-9aa1-45f1-a618-33e2c692a7a6.jpg) |  [Relay 2 chanel module](https://aliexpress.ru/item/33038634587.html?item_id=33038634587&sku_id=67329513676&spm=a2g2w.productlist.0.0.4d5ff55dhoQIV2)

</details>

If my work has been useful to you, I would be grateful for your support:

<a href="https://www.buymeacoffee.com/andriiradchenko" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a> <a href="https://www.paypal.com/donate/?hosted_button_id=QB42TMLKZ7KAE" target="_blank"><img src="https://github.com/AndreiRadchenko/hassio-webrecognition/blob/main/img/paypal-donation.png" alt="Donate with PayPal" style="height: 60px !important;width: 250px !important;" ></a>

# Використання ефекту ESPHome addressable_lambda для підсвітки сходів
Я використав ESP32 з ESPHome для підсвітки сходів. Коли рух фіксується внизу сходів - світло на сходах біжитьт знизу вгору. 
Коли ви наближаєтесь до сходів на верхньому поверсі - світло спадає вздовж сходів донизу. 
Крім того, до тієї ж ESP32 підключені датчик руху внизу сходів, реле головного освітлення кабінету та спальні, а також сенсорні кнопки для перемикання
цих реле

## Кінцевий результат

https://user-images.githubusercontent.com/25689113/148655637-32b09107-9162-4490-a45f-eb8701f7335e.mov

https://user-images.githubusercontent.com/25689113/148655689-6d87c76c-4f25-4c28-ab43-223c05c54301.mov

## Файли конфігурації

Файл конфігурації ESPHome містить три лямбда-ефекти:
 - Підсвітка загоряється згори-вниз (Falling light)
 - Підсвітка загоряється знизу-вгору (Rise light)
 - Підсвітка злегка блимає і потім світить одним кольором постійно (Simple light)

Автоматизація Home Assistant (Stair light motion activation) включає підсвітку на 5 хвилин за умови виявлення руху і рівні освітленості за вікном менше
5%. Режим роботи автоматизації "Restart". При виявленні руху внизу сходів і вимкненій підсвітці автоматизація включає підсвітку з ефектом "Rise light", 
при виявленні руху нагорі і вимкненій підсвітці запускається ефект "Falling light", при повторному виявленні руху і працюючій підсвітці запускається
ефект "Simple light". При цьому повторне спрацювання датчиків руху візуалізується легким одноразовим блиманням підсвітки сходів.

Config file            |  Description
-------------------------|-------------------------
[cabinet_light.yaml](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/cabinet-light.yaml) | файл конфігурації ESPHome            
[automation.yaml](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/automation.yaml)  |  Відповідна автоматизація Home Assistant
[Cabinet_light.pdf](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/Cabinet_light.pdf) | Принципова схема 

## Складові
<details><summary> </summary>

Плата ESP сенсори та інші складові проекту.

Parts           |  Description
-------------------------|-------------------------
![](https://user-images.githubusercontent.com/25689113/148658704-cd28fc58-16d5-4422-8831-bf5fc5abab7b.png) | ESP32 dev board pinout
![](https://user-images.githubusercontent.com/25689113/148826076-460cdcab-3112-4e65-b259-8a7a57372665.jpg) |  [Level converter](https://aliexpress.ru/item/4000039891923.html?gclid=CjwKCAiAz--OBhBIEiwAG1rIOt__LgE36QTgDeKzNgaGONAvyxLjPSalt-yexpLlaA8PR2bWy9AKTRoCyQIQAvD_BwE&sku_id=10000000088879366) 
![](https://user-images.githubusercontent.com/25689113/148741979-414e8d72-1d6c-4208-8d25-7135871d9eea.jpg) |  [WS2812B Individually Addressable LED Strip Light](https://smartlight.me/adressable-led-strips/adressable-led-strip-ws2812b-60led)
![](https://user-images.githubusercontent.com/25689113/148736478-b5593292-0e4d-4a8c-9f08-a343146ac247.jpg)  |  HC-SR501 motion sensor
![](https://user-images.githubusercontent.com/25689113/148739631-f663c8cd-52f4-4e50-a663-18b300b02349.jpg) |  [Sonoff motion (PIR) Sonoff SNZB-03](https://smartlight.me/smart-home-devices/zigbee-devices/pir_sensor_sonoff_snzb-03)

</details>

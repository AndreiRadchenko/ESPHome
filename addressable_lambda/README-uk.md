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
[Stair light motion activation](https://github.com/AndreiRadchenko/ESPHome/blob/main/addressable_lambda/automation.yaml)  |  Відповідна автоматизація Home Assistant

## Складові
<details><summary> </summary>

Плата ESP board and sensors that i'm used in project.

Parts           |  Description
-------------------------|-------------------------
![](https://user-images.githubusercontent.com/25689113/148658704-cd28fc58-16d5-4422-8831-bf5fc5abab7b.png) | ESP32 dev board pinout           |  
![](https://user-images.githubusercontent.com/25689113/148659048-fd9e5a80-87e2-4306-9672-d50ae9beef7d.jpg)  |  [Sonoff motion (PIR) Sonoff SNZB-03](https://smartlight.me/smart-home-devices/zigbee-devices/pir_sensor_sonoff_snzb-03)

</details>

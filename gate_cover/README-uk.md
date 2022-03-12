# Інтеграція автоматичних воріт в Home Assistant, HomeKit і Google Home
За допомогою Sonoff Basic, датчика дверей і ESPHome я отримав повноцінну інтеграцію з відображенням актуального стану автоматичних воріт
в Home Assistant, HomeKit і Google Home. 

## Result illustration

Watch on youtube:

[![Watch on youtube](https://img.youtube.com/vi/-kzQXVCUmVY/0.jpg)](https://youtu.be/-kzQXVCUmVY)

## Hardware preparation

Автоматичні ворота, що відкриваються радіо брелком, як правило мають на платі управління GPIO контакти для альтернативного управління відкриттям.
Коротке замикання/розмикання контактів призводить до відкриття воріт, якщо вони були в закритому стані, закриття воріт, якщо вони були відкриті і зупинки
якщо ворота відчинялись чи зачинялись. 

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/edinger-gpio.jpeg" width="70%"></img> 

Я переробив Sonoff Basic на реле з "сухими" контактами, що дозволило мені скористатися GPIO на платі управління воротами. 
[Відео по 
модернізації Sonoff Basic на реле з "сухими" контактами.](https://www.youtube.com/watch?v=zpuJ-BpDvpU&t=8s&ab_channel=jlivinginmadrid)
А також підключив до контакту RX Sonoff магнітний датчик відкриття для отримання стану воріт.

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/sonoff-modification1.jpg" width="70%"></img> 

Нижче наведено фото реле з герконом на три метровому кабелі перед монтажем на ворота. А також геркон змонтований на ворота.

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/relay-before-install.jpg" width="32%"></img> <img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/door-sensor.jpg" width="32%"></img> <img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/gate-with-sensor.jpg" width="32%"></img> </img> 

## ESPHome config file

Config file            |  Description
-------------------------|-------------------------
[gate.yaml](https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/gate.yaml) | ESPHome config file   

Якщо ви не знаєте як прошити Sonoff, ознайомтеся з цим [розділом на esphome.io](https://esphome.io/devices/sonoff_basic.html)

Прошивка посилається на 
~~~yaml
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
~~~
Відповідні параметри задаються в файлі `secrets.yaml` робочої директорії НА (там де configuratuion.yaml)  
Для того щоб контролер під'єднався до вашої мережі потрібно в робочій директорії створити файл `secrets.yaml` і задати наступні параметри:
``` yaml
wifi_ssid: you_wifi_ssid
wifi_password: you_wifi_password
```
Потім на сторінці ESPHome в правому верхньому куті потрібно відкрити вікно  SECRETS і ввести наступний рядок
```yaml
<<: !include ../secrets.yaml
```
<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/esphome_secrets.png" width="100%"></img> 

Після прошивки Sonoff Basic файлом конфігурації `gate.yaml`, ESPHome автоматично додасть в Home Assistant сутність `Gate` типу `cover`, зі всіма сервісами 
і властивостями. Залишиться тільки додати картку чи кнопку в Lovelase або прокинути через брідж в HomeKit. Нижче снапшот `configuration.yaml` 
в якому в HomeKit додаються створені нами в ESPHome сенсор відкриття воріт і власне сутність воріт.

``` yaml
homekit:
  - filter:
      include_domains:
        - light
        - media_player
      include_entities:
        - binary_sensor.gate_open_sensor
        - cover.gate
```

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/HomeKit-Gate.jpg" width="30%"></img> 

## Parts
<details><summary> </summary>

Parts           |  Description
-------------------------|-------------------------
![](https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/SONOFF_BASIC_R2_01.jpg) |  [Sonoff Basic R2](https://www.amazon.com/Wireless-Universal-Automation-Solution-Assistant/dp/B07KP8THFG/ref=sr_1_1_sspa?crid=1UOCYPNP8HKOL&keywords=sonoff+basic+r2&qid=1647099999&s=industrial&sprefix=sonoff+basic%2Cindustrial%2C212&sr=1-1-spons&psc=1) 
![](https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/wired_magnetic_door_sensor.jpg) |  [Wired magnetic door sensor](https://www.amazon.com/dp/B091GFZYB8/ref=sspa_dk_hqp_detail_aax_0?)
</details>

If my work has been useful to you, I would be grateful for your support:

<a href="https://www.buymeacoffee.com/andriiradchenko" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

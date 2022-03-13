# Інтеграція автоматичних воріт в Home Assistant, HomeKit і Google Home
За допомогою реле Sonoff Basic, магнітного датчика дверей і ESPHome я отримав повноцінну інтеграцію автоматичних воріт
в Home Assistant, HomeKit і Google Home, з відображенням актуального стану. 

## Result illustration

Нажаль, через війну з російськими фашистами, що спричинили гуманітарну катастрофу в моєму місті, не можу зняти актуальне відео. Тому посилаюся
на відео зняте кілька років тому.

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

Крім програмного управління воротами, в файлі конфігурації описана фізична кнопка реле Sonoff. Короткочасне натиснення на неї теж призведе 
до відкриття, закриття чи зупинки воріт. Стан воріт визначається за допомогою `binary_sensor.gate_open_sensor`, до якого підключений магнітний сенсор дверей. 
Він має 2 секундну затримку на перехід у стан 'on'. Ця затримка введена для фільтрації випадкових спрацювань від поривів вітру.

``` yaml
binary_sensor:
  - platform: gpio
    pin:
      number: GPIO0
      mode:
        input: true
        pullup: true
      inverted: true
    name: "Gate Button1"
    internal: true
    on_press:
      then:
        - switch.turn_on: relay
        - delay: 1s
        - switch.turn_off: relay
        - logger.log: Gate Button1 Pressed
  - platform: gpio    
    pin:
      number: GPIO3
      mode:
        input: true
        pullup: true
    name: "Gate Open Sensor"
    id: "gate_open_sensor"
    device_class: door
    filters:
      - delayed_on: 2s        
```
Сутність воріт `cover.gate` реалізована за допомогою шаблонної кнопки `gate_button2`, прихованої з інтерфейсу Home Assistant.  Шаблонна кнопка в скрипті `on_press` замикає на одну секунду реле з "сухими" контактами, підключене до GPIO плати управління воротами. Шаблон `cover.gate` отримує стан від вище описаного `binary_sensor.gate_open_sensor`.

``` yaml
button:
  - platform: template
    name: "Gate Button2"
    id: "gate_button2"
    internal: true
    on_press:
      then:
        - switch.turn_on: relay
        - delay: 1s
        - switch.turn_off: relay
        - logger.log: Gate Button2 Pressed        
cover:
  - platform: template
    name: "Gate"
    device_class: gate
    lambda: |-
      if (id(gate_open_sensor).state) {
        return COVER_OPEN;
      } else {
        return COVER_CLOSED;
      }
    open_action:
      - button.press: gate_button2
    close_action:
      - button.press: gate_button2
    stop_action:
      - button.press: gate_button2
``` 

Після прошивки Sonoff Basic файлом конфігурації `gate.yaml`, ESPHome автоматично додасть в Home Assistant сутність `Gate` типу `cover`, зі всіма сервісами 
і властивостями. Залишиться тільки додати картку чи кнопку в Lovelace або прокинути через брідж в HomeKit. Нижче снапшот `configuration.yaml` 
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

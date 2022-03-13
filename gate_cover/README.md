# Automatic gate integration for Home Assistant, HomeKit and Google Home
With the Sonoff Basic relay, magnetic door sensor and ESPHome, I'm got a full integration of automatic gate
for Home Assistant, HomeKit and Google Home, with actual state displaying.

## Result illustration

Unfortunately, due to the war with the Russian fascists, which caused a humanitarian catastrophe in my town, I cannot shoot a actual video. That's why I'm referring on the video taken a few years ago. 

Watch on youtube:

[![Watch on youtube](https://img.youtube.com/vi/-kzQXVCUmVY/0.jpg)](https://youtu.be/-kzQXVCUmVY)

## Hardware preparation

Automatic gates, opened by a radio key, usually have the GPIO contacts on  control board for alternative opening control.
Short closing / opening of contacts leads to opening of gate if they were in the closed condition, closing of gate if they were open 
and stop if the gate are opening or closing.

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/edinger-gpio.jpeg" width="70%"></img> 

I'm modified  Sonoff Basic to a relay with "dry" contacts, which allowed me to use GPIO on the gate control board.
[Video about modifying Sonoff Basic to a relay with "dry" contacts.](https://www.youtube.com/watch?v=zpuJ-BpDvpU&t=8s&ab_channel=jlivinginmadrid)
And also, I'm was connect 'RX' contact of Sonoff Basic to the magnetic opening sensor, to obtain the state of the gate.

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/sonoff-modification1.jpg" width="70%"></img> 

Below is a photo of a relay with a magnetic door sensor on a three-meter long cable, before mounting on the gate. And the door sensor  mounted on the gate.

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/relay-before-install.jpg" width="32%"></img> <img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/door-sensor.jpg" width="32%"></img> <img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/gate-with-sensor.jpg" width="32%"></img> </img> 

## ESPHome config file

Config file            |  Description
-------------------------|-------------------------
[gate.yaml](https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/gate.yaml) | ESPHome config file   

If you don't know how to flash Sonoff, read this [article at esphome.io](https://esphome.io/devices/sonoff_basic.html)

Wi-Fi connection is implemented as follows:
~~~yaml
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
~~~
The corresponding parameters are set in the  `secrets.yaml` file of the working directory (where the configuratuion.yaml located)
In order  to connect controller to your network, you need to create the `secrets.yaml` file in the working directory and set the following parameters:
``` yaml
wifi_ssid: you_wifi_ssid
wifi_password: you_wifi_password
```
Then on the ESPHome page in the upper right corner you need to open the SECRETS window and enter the next line:
```yaml
<<: !include ../secrets.yaml
```
<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/esphome_secrets.png" width="100%"></img> 

Except the software gate control,  configuration file describes the physical button of the Sonoff relay. A short press on it will result opening, closing or stopping the gate, depending on the initial state. The state of the gate is determined by `binary_sensor.gate_open_sensor`, to which the magnetic door sensor is connected. It has a 2 second delay to be set to 'on' state. This delay is introduced to filter out accidental triggers from gusts of wind.

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
The gate entity  `cover.gate` is implemented using the template button `gate_button2`, hidden from the Home Assistant interface. The template button in the `on_press` script, close relay with "dry" contacts for one second. In turn, relay  connected to the GPIO gate control board. The `cover.gate` template gets its state from the` binary_sensor.gate_open_sensor`, that wath described above.

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

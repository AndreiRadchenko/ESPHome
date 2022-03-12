# Інтеграція автоматичних воріт в Home Assistant, HomeKit і Google Home
За допомогою Sonoff Basic, датчика дверей і ESPHome я отримав повноцінну інтеграцію з відображенням актуального стану автоматичних воріт
в Home Assistant, HomeKit і Google Home. 

## Result illustration

Watch on youtube:

[![Watch on youtube](https://img.youtube.com/vi/-kzQXVCUmVY/0.jpg)](https://youtu.be/-kzQXVCUmVY)

Автоматичні ворота, що відкриваються радіо брелком, як правило мають на платі управління GPIO контакти для альтернативного управління відкриттям.
Коротке замикання/розмикання контактів призводить до відкриття воріт, якщо вони були в закритому стані, закриття воріт, якщо вони були відкриті і зупинки
якщо ворота відчинялись чи зачинялись. 

<img src="https://github.com/AndreiRadchenko/ESPHome/blob/main/gate_cover/images/edinger-gpio.jpeg" width="100%"></img> 

Я переробив Sonoff Basic на реле з "сухими" контактами, що дозволило мені скористатися GPIO на платі управління воротами.
А також додав 

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

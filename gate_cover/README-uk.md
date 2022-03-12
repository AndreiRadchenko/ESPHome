# Інтеграція автоматичних воріт в Home Assistant і HomeKit
Автоматичні ворота, що відкриваються радіо брелком, як правило мають на платі управління GPIO виводи для альтернативного управління відкриттям. 

## Result illustration

Watch on youtube:

[![Watch on youtube](https://img.youtube.com/vi/-kzQXVCUmVY/0.jpg)](https://youtu.be/-kzQXVCUmVY)

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

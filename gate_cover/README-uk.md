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

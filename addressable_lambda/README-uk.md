# Використання ефекту addressable_lambda для підсвітки сходів
Я використав ESP32 з ESPHome для підсвітки сходів. Коли рух фіксується внизу сходів - світло на сходах біжитьт знизу вгору. 
Коли ви наближаєтесь до сходів на верхньому поверсі - світло спадає вздовж сходів донизу. 
Крім того, до тієї ж ESP32 підключені датчик руху внизу сходів, реле головного освітлення кабінету та спальні, а також сенсорні кнопки для перемикання
цих реле

## Installing
In Hass.io, navigate to Supervisor > Add-on Store > Repositories and add

    https://github.com/AndreiRadchenko/hassio-webrecognition
  
  Local web server for realtime face recognition by http request or web interface

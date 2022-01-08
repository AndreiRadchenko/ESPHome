# Use addressable_lambda effect for stair light
I'm have setup ESP32 with ESPHome for lightup stair. When motion detected at the bottom of the stair - light rising up, 
and when you approach to stair from the upper floor - light falling down.
Exept this, the same ESP32 board, handle motion sensor at the stair base, light relays for cabinet and bedroom main light, and sensor buttons for 
this lights toggling.

## Installing

https://user-images.githubusercontent.com/25689113/148655637-32b09107-9162-4490-a45f-eb8701f7335e.mov



https://user-images.githubusercontent.com/25689113/148655689-6d87c76c-4f25-4c28-ab43-223c05c54301.mov


In Hass.io, navigate to Supervisor > Add-on Store > Repositories and add

    https://github.com/AndreiRadchenko/hassio-webrecognition
  
  Local web server for realtime face recognition by http request or web interface

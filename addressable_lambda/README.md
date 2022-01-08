# Use addressable_lambda effect for stair light
I'm have setup ESP32 with ESPHome for lightup stair. When motion detected at the bottom of the stair - light rising up, 
and when you approach to stair from the upper floor - light falling down.
Exept this, the same ESP32 board, handle motion sensor at the stair base, light relays for cabinet and bedroom main light, and sensor buttons for 
this lights toggling.

## Result Illustration

https://user-images.githubusercontent.com/25689113/148655637-32b09107-9162-4490-a45f-eb8701f7335e.mov

https://user-images.githubusercontent.com/25689113/148655689-6d87c76c-4f25-4c28-ab43-223c05c54301.mov

Solarized dark             |  Solarized Ocean
:-------------------------:|:-------------------------:
![](https://user-images.githubusercontent.com/25689113/148655637-32b09107-9162-4490-a45f-eb8701f7335e.mov)  |  ![](https://user-images.githubusercontent.com/25689113/148655689-6d87c76c-4f25-4c28-ab43-223c05c54301.mov)

<img src="https://user-images.githubusercontent.com/25689113/148655637-32b09107-9162-4490-a45f-eb8701f7335e.mov" width="600"/> <img src="https://user-images.githubusercontent.com/25689113/148655689-6d87c76c-4f25-4c28-ab43-223c05c54301.mov" width="600"/>

<details><summary>Instructions</summary>

Mixture is available on the [OpenUPM](https://openupm.com/packages/com.alelievr.mixture/) package registry, to install it in your project, follow the instructions below.

1. Open the `Project Settings` and go to the `Package Manager` tab.
2. In the `Scoped Registry` section, click on the small `+` icon to add a new [scoped registry](https://docs.unity3d.com/2020.2/Documentation/Manual/upm-scoped.html) and fill the following information:
```
Name:     Open UPM
URL:      https://package.openupm.com
Scope(s): com.alelievr
```
3. Then below the scoped registries, you need to enable `Preview Packages` (Mixture is still in preview).
4. Next, open the `Package Manager` window, select `My Registries` in the top left corner and you should be able to see the Mixture package.
5. Click the `Install` button and you can start using Mixture :)

![](docs/docfx/images/2020-11-09-11-37-01.png)

:warning: If you don't see `My Registries` in the dropdown for some reason, click on the `+` icon in the top left corner of the package manager window and select `Add package from Git URL`, then paste `com.alelievr.mixture` and click `Add`.

Note that sometimes, the package manager can be slow to update the list of available packages. In that case, you can force it by clicking the circular arrow button at the bottom of the package list.

</details>

In Hass.io, navigate to Supervisor > Add-on Store > Repositories and add

    https://github.com/AndreiRadchenko/hassio-webrecognition
  
  Local web server for realtime face recognition by http request or web interface

# Water Meter
Water meter device for Home Assistant

## REQUIREMENTS
- [Water Flow Sensor](https://es.aliexpress.com/item/32871294401.html?spm=a2g0s.9042311.0.0.d1124c4dEI47oQ)
- [Wemos D1 Mini Pro or ESP8266](https://es.aliexpress.com/item/32803725174.html?spm=a2g0s.9042311.0.0.d1124c4dEI47oQ)

## SOFTWARE INSTRUCTION
### FLASHING YOUR ESP8266
1. Flash your Wemos D1 Mini Pro or ESP8266 with Tasmota. For more instruction follow [this link](https://github.com/arendst/Tasmota)
2. Open the already downloaded files and navigate to ..\Tasmota-X.XX.XX\tasmota open the project in Arduino IDE and then Edit the [my_user_config.h](https://github.com/rossiluis22/water_meter/blob/master/photo_reference/my_user_config.png?raw=true) with your Wifi value, you can also add your MQTT server info otherwise you can add them later and finally **Upload** it to your Wemos D1 Mini Pro / ESP8266
### CONFIGURING TASMOTA
1. Go to your browser and enter the Wemos D1 Mini Pro / ESP8266.
2. Then go to `Configuration-->Configure MQTT`and add your MQTT server info.
3. Go to `Configure Module` --> Module Type `Generic (18)` --> D2  **GPIO4** `COUNTER1`. [configure_module](https://github.com/rossiluis22/water_meter/blob/master/photo_reference/configure_module.png?raw=true)
4. (optional) Configure your **device name** and **friendly name** navigating to `Configuration-->Configure Other`

## HARDWARE INSTRUCTION
Plug the water sensor to Wemos D1 mini pro: [Wemos pin out ](https://github.com/rossiluis22/water_meter/blob/master/photo_reference/wemos_pin_connection.png?raw=true)
|Wemos d1 mini pro |Water sensor flow |
| --------------- | --------------- |
| Pin: 5v |Red wire (+5v)|
| Pin: GND | Black wire (ground) |
| Pin: D2 | Yellow wire (data) |

<img src="https://github.com/rossiluis22/water_meter/blob/master/photo_reference/wemos_pin_connection.png?raw=true" height="200" width="400">

## HOME ASSISTANT INSTRUCTIONS
Assuming that you already have a MQTT server such as Mosquitto
Enable MQTT and MQTT Discovery [More Info](https://www.home-assistant.io/integrations/mqtt/)
In your **Home Assistant** navigate to `Configuration-->Integrations-->MQTT` select the device, then `MQTT Info` and take note of the Sensor info inside **Entities** --> **Subscribed topics** --> tele/**YOUR-DEVICE-NAME**_**ClientID**/SENSOR  

**Ex:** `tele/watersoleil_636D34/SENSOR`.

Now in your ```sensor.yaml``` enter your already copied sensor info and replace with your values. 

**NOTE** : you must calibrate your sensor by using measuring jar, take note of how many pulses is 1 Liter and divide it by the pulses. In my case: 

**Ex: 1 Liter/607 (pulses)=0.001648** 
```  
     - platform: mqtt
     state_topic: "tele/YOUR-DEVICE-NAME_ClientID/SENSOR"
     name: "MQTT Sensor Water"
     value_template: "{{ ( value_json['COUNTER'].C1 | multiply(0.001648) | float ) | round(2) }}"
     unit_of_measurement: "l"
```
Finally you should have a sensor called `sensor.mqtt_sensor_water` and with help of [utility meter](https://www.home-assistant.io/integrations/utility_meter/) you would be able to measure how many water you cosume.

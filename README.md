# Water Meter
Water meter device for Home Assistant

## REQUIREMENTS
- [Water Flow Sensor](https://es.aliexpress.com/item/32871294401.html?spm=a2g0s.9042311.0.0.d1124c4dEI47oQ)
- [Wemos D1 Mini Pro or ESP8266](https://es.aliexpress.com/item/32803725174.html?spm=a2g0s.9042311.0.0.d1124c4dEI47oQ)

## INSTRUCTION
1. Flash your Wemos D1 Mini Pro or ESP8266 with Tasmota. For more instruction follow [this link](https://github.com/arendst/Tasmota)
1.1 Edit the ![my_user_config.h](https://imgur.com/a/gnUnUC4) with your Wifi values

2. Go to your browser and enter the Wemos IP address
2.1 Then go to `Configuration-->Configure MQTT`and add your MQTT server info
2.2 Go to `Configure Module` --> Module Type `Generic (18)` --> D2  **GPIO4** `COUNTER1`

# The Project Smart Home
The project SmartHome is my project where I am implementing network for controlling home. 
The project includes: 
 * A server, which is created by Server TomCat ([ServerSmartHome](https://github.com/AlieksieievYurii/SmartHome_server)).
 * "The Controller Of Devices(TCOD)" wich is created by Arduino and Ethernet Shield ([TheControllerOfDevices](https://github.com/AlieksieievYurii/TheControllerOfDevices)).
 * Android App "SmartHome" ([Android-app-for-smart-home](https://github.com/AlieksieievYurii/Android-app-for-smart-home))
 
## Server:
<a href=""><img src="Documentation/diagram_arhitekture.png"></a>&nbsp;

The work of server is simple. In this time the server just get data(json file which contains data that
pins of Controller Devices need to  turn on) and send to Controller Device. Server works by servlets. 
Server has two serlvets, first servlet "Actions" is used for controlling pins of Controller Device. 
Second servlet used for devices which send commands for SmartHome. 
The Controller Device does GET request to Servlet "Actions" every 500 miliseconds,then get json file, 
does parsing and write pin which is needed. Android App and other apps which can controll SmartHome, 
does POST request to Servlet "ActionsByDevices". This servlet reads data(json) from difference Apps(which can controll SmartHome) 
and just write it to file ActionsFromDevices.json. After that, ActionsFromDevices.json rewrite to file Actions.json(this json file is
for TCOD). Between ActionForm Device and Actions have to be Tasker(But it is not implemented at this moment)
which does some actions with data. For instance, install timer for turn on/off pin of TCOD.

## The Controller Of Devices(TCOD)

This is microcontroller Arduino Mega with Ethernet Shield. The work of TCOD is simple. It does GET request to server 
and get json file from Servlet "Actions". The controller parses the json file and turn on/off pins by actions from this file.
TCOD does request every 500 miliseconds to server with params:key,temperature,light. The parameter key contains key access to server.
The parameter temperature contains value from Sensor(DHT11) of temperature.The parameter light contains value of Light resistor.
The Light resistor shows luminosity inside room or outside.

## Android App "SmartHome"

The Android App "SmartHome" is used for controlling smart home. The app does POST requst to server.In the POST request contains
json object element. In this json file is number of pin and status HIGH/LOW or VALUE(if it is a PWM pin). After that, the App does
GET post every 1 second for getting HashSum. HashSum is from json file ActionsByDevices. If after getting HashSum it differs from
HashSum in App, App will do GET request to server for getting json file actions of all pins.
The App is with dynamic user interface. Users can add dinamicaly Views(Buttons,SeekBars) and configurate it. For instance,
User wants to connect a lamp to pin 22. He have to connect it to 22 pin by relay. Next he have to add Button/SeekBar and configurate:
pin, id, icons, name in app. Also he can see example json object for TCDO. Users can remove or edit views.

 
 
 
 
 
(c) Yurii Alieksieiev 2018 Lodz,Poland
 


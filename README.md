This project was forked from https://github.com/hallard/WebSocketToSerial 


Serial<->Websocket bridge for use with LaserWeb CNC software, to proxy gcode/commands to a Smoothieware/Grbl based controller

Dependencies
------------
- Arduino https://github.com/esp8266/Arduino
- @me-no-dev https://github.com/me-no-dev/ESPAsyncWebServer
- @me-no-dev https://github.com/me-no-dev/ESPAsyncTCP 
- @alanswx https://github.com/alanswx/ESPAsyncWiFiManager
- nodejs for web pages development test 


Test web page without ESP8266
-----------------------------

You need to have [nodejs][7] and some dependencies installed.

[webdev] folder is the development folder to test and validate web pages. It's used to avoid flashing the device on each modification.
All source files are located in this folder the ESP8266 `data` folder (containing web pages) is filled by a nodejs script launched from [webdev] folder. This repo contain in [data] last used files so if you don't change any file, you can upload to SPIFFS as is.

To test web pages, go to a command line, go into [webdev] folder and issue a:    
`node web_server.js`     
then connect your browser to htpp://localhost:8080 you can them modify and test source files such index.htm
    
Once all is okay issue a:    
`node create_spiffs.js`     
this will gzip file and put them into [data][13] folder, after that you can upload from Arduino IDE to device SPIFFS

See comments in both [create_spiffs.js][11] and [web_server.js][11] files, it's also indicated dependencies needed by nodejs.

Terminal Commands:
------------------
- connect : connect do target device
- help : show help

Commands once connected to remote device:
-----------------------------------------
- `!close` or CTRL-D : close connection
- `swap` swap ESP8266 UART pin between GPIO1/GPIO3 with GPIO15/GPIO13
- `ping` typing ping on terminal and ESP8266 will send back pong
- `esphelp` show help
- `heap` show ESP8266 free RAM
- `whoami` show WebSocket client # we are
- `who` show all WebSocket clients connected
- `fw` show firmware date/time
- `baud n` set ESP8266 serial baud rate to n (to be compatble with device driven)
- `reset p` reset gpio pin number p
- `spils` list SPIFFS files
- `read file` execute SPIFFS file command


Every command in file `startup.ini` are executed in setup() you can chain with other files. 

I'm using this sketch to drive Microchip RN2483 Lora module to test LoraWan, see the [boards][8] I used.

For example my `startup.ini` file contains command to read microchip RN2483 config file named `rn2483.txt`

`startup.ini`
```sh
# Startup config file executed once in setup()

!baud 57600
# Light on the LED on GPIO0
sys set pindig GPIO0 1
!delay 250
# Light on the LED on GPIO10
sys set pindig GPIO10 1
!delay 250
```

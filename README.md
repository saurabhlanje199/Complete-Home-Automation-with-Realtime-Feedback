# Complete-Home-Automation-with-Realtime-Feedback
With this you can control home appliances anywhere in the world from your phone as well as manually from switches with real-time feedback on your app with internet or without internet.

With ESP32 you will be able to control five AC appliances and with Node MCU ESP8266 you will be able to control two AC appliances, along with the speed of the fan using the capacitive dimming method. Everything will be working with the internet and without internet along with the manual switches and fan regulators with real-time feedback on the app. 
Phew! That sounds so freaking awesome. Well, don’t worry, I have tried to keep it as simple as possible so that it becomes easy for you to learn and make it by yourself. Let us just dive in.

### Watch Working and Tutorial of this project on [Youtube]()

## Table of content

- [Installation](#installation)
    - [Arduino Ide](#arduino-ide)
    - [Blynk Library](#blynk-library)
    - [ESP32 Board](#esp32-board)
    - [Node MCU ESP8266 Board](#node-mcu-esp8266-board)
- [Components](#components)
- [Working](#working)
- [Schematics & Code](#schematics-and-code)
- [Support](#support)

## Installation

### Downloading and installing steps:

#### Arduino Ide:
* **[Download](https://www.arduino.cc/en/software)** the latest version of the Arduino Ide.
* **Go** to the folder: `\Downloads`
  * Look for Arduino Ide file and **Install** it.

#### Blynk Library:
* Open **Arduino Ide**.
  * Go to **Sketch**:
  * Go to **Include Library**
  * Go to **Manage Libraries**
  * Search for **Blynk** in search box
  * Select latest version in **Select Version** box and click **Install**

#### ESP32 Board:
* * Open **Arduino Ide**.
  * Go to **Files**:
  * Go to **Preferences**
  * Go to **Additional Boards Manager URLs:** and Paste- https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json their and click Ok.
  * Now go to **Tools**:
  * Go to **Boards:**
  * Go to **Boards Manager**
  * Search for **esp32** in search bar
  * Select latest version in **Select Version** box and click **Install**

#### Node MCU ESP8266 Board:
* * Open **Arduino Ide**.
  * Go to **Files**:
  * Go to **Preferences**
  * Go to **Additional Boards Manager URLs:**, put “,” if already any link exits and Paste- http://arduino.esp8266.com/stable/package_esp8266com_index.json their and click Ok.
  * Now go to **Tools**:
  * Go to **Boards:**
  * Go to **Boards Manager**
  * Search for **esp8266** in search bar
  * Select latest version in **Select Version** box and click **Install**


## Components

1. [ESP 32](https://www.amazon.in/SquadPixel-ESP-32-Bluetooth-Development-Board/dp/B071XP56LM/ref=sr_1_1?dchild=1&keywords=esp32&qid=1612376825&sr=8-1) / [Node MCU ESP8266](https://www.amazon.in/Lolin-NodeMCU-ESP8266-CP2102-Wireless/dp/B010O1G1ES/ref=sr_1_2?dchild=1&keywords=esp8266&qid=1612376986&sr=8-2)
2. [8 Channel Relay](https://www.amazon.in/JBTek-Channel-Control-Optocoupler-Arduino/dp/B00KTELP3I/ref=sr_1_1?crid=3BEE2K9I4CM3P&dchild=1&keywords=8+channel+relay+module&qid=1612377061&sprefix=8+channel+re%2Caps%2C194&sr=8-1)
3. [Breadboard Small](https://www.amazon.in/REES52-400-Point-Solderless-Breadboard/dp/B01IN0QGP6/ref=sr_1_2?crid=3J32PUARETXGC&dchild=1&keywords=small+breadboard&qid=1612377128&sprefix=small+brea%2Caps%2C381&sr=8-2)
4. [Breadboard Large](https://www.amazon.in/Generic-Elementz-Solderless-Piecesb-Circuit/dp/B00MC1CCZQ/ref=sr_1_4?crid=J9RLK5OJ4NKB&dchild=1&keywords=breadboard&qid=1612377224&sprefix=breadb%2Caps%2C643&sr=8-4)
5. [Jumper](https://www.amazon.in/Electrobot-Jumper-Wires-120-Pieces/dp/B071VQLQQQ/ref=sr_1_2?crid=OC4LYMDWO3JR&dchild=1&keywords=jumper+wires&qid=1612378597&sprefix=jumpr%2Caps%2C634&sr=8-2)
6. [Regulator Switch](https://www.amazon.in/pole-Wiring-Selector-Rotary-Switch/dp/B08PS32FCY/ref=sr_1_4?dchild=1&keywords=rotary+switch&pd_rd_r=a68bfd61-e901-4920-ac14-aa270e352c48&pd_rd_w=QPL4h&pd_rd_wg=FaYzp&pf_rd_p=1abe8808-d6bc-4840-858b-6acddb119a7a&pf_rd_r=YT4SZWE3WYXHPDEGZDRM&qid=1612378653&sr=8-4)
7. Prototype Board
8. 2.2ohm 1/2W Resistor
9. 220k ohm 1/4W Resistor
10. 3.3uF 250V Capacitor
11. 2.2uf 250V Capacitor
12. 10k ohm 1/4W Resistors

## Working
**Blynk** IoT platform is used for the internet controlled part. For manual switching and capacitive regulator are used for without internet part.
Let’s have a look with a simple flow chart.

![Flow Chart](https://github.com/aniketmarwade/Home-Automation-2021-FInal-Year-Project/blob/main/IMAGES/diagrams/flow_saurabh.png?raw=true)

## Schematics and Code
First go through [Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-wrover_datasheet_en.pdf) of ESP32.
For ESP32 Schematics & Code Please Refer this- **[Click Here](https://github.com/saurabhlanje199/Complete-Home-Automation-with-Realtime-Feedback/tree/main/ESP32)**

First go through [Datasheet](https://www.espressif.com/sites/default/files/documentation/0a-esp8266ex_datasheet_en.pdf) of Node MCU ESP8266
For Node MCU ESP8266 Schematics & Code Please Refer this- **[Click Here](https://github.com/saurabhlanje199/Complete-Home-Automation-with-Realtime-Feedback/tree/main/NodeMCU%20ESP8266)**

Also, Refer details mentioned in **README.md**

## Support
Reach out to me for project related queries or PCB and save plenty of time and resources and get the project ready to be used, at **saurabhlanje20@gmail.com**

Also if you use this project, have learned something from it, or just like it, please consider supporting it by buying some Electronics Components, so I can dedicate more time on Electronics projects like this :)

<div align="center">
<p><a href="https://www.buymeacoffee.com/saurabhlanje"> <img align="center" src="https://raw.githubusercontent.com/aniketmarwade/Home-Automation-2021-FInal-Year-Project/main/IMAGES/saurabh/electronic_buy.png" height="55" width="300" alt="saurabhlanje" /></a></p><br><br>
</div>

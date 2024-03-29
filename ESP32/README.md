
### Watch Working and Tutorial of this project on [Youtube](https://youtu.be/jA4DQPxXu4A)

For Details Regarding **Installation, Components, Working** [click here](https://github.com/saurabhlanje199/Complete-Home-Automation-with-Realtime-Feedback/blob/main/README.md). 

## Table of content
- [Schematics](#schematics)
    - [ESP32 Connections](#esp32-connections-with-rotary-switch-switches-and-relay)
    - [Relay Connections](#further-relay-connection-with-home-appliances)
- [Code](#code)
    - [Uploading Code](#uploading-code)
    - [ESP 32 Code](#esp32-code)
        - [Libraries Included](#libraries-included)
        - [Input-Output Pins Defined](#input-output-pins-defined)
        - [Mode](#mode)
        - [Authentication Token](#authentication-token)
        - [WiFi Credentials](#wifi-credentials)
        - [Flag Define](#flag-define)
        - [Assigning Incoming Values](#assigning-incoming-values-from-blynk-app-to-variables)
        - [Void Setup](#void-setup)
        - [WiFi Status](#wifi-status)
        - [With Internet Function](#with-internet-function)
        - [Without Internet Function](#without-internet-function)
        - [Fan Speed Control](#fan-speed-control)
        - [Check Blynk](#check-blynk)
- [Support](#support)

## Schematics
First go through [Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-wrover_datasheet_en.pdf) of ESP32.

Refer to [Schematics](https://github.com/saurabhlanje199/Complete-Home-Automation-with-Realtime-Feedback/blob/main/ESP32/Schematic_ESP32.png) as well as the following table and do proper connections.

### ESP32 Connections with Rotary switch, Switches and Relay

I/O Index | GPIO | Connection 
------------ | ------------- |-------------
`34` | `GPIO34` | ESP32 to Rotary Switch speed 1st terminal with 10k Resistor in between
`35` | `GPIO35` | ESP32 to Rotary Switch speed 2nd terminal with 10k Resistor in between
`32` | `GPIO32` | ESP32 to Rotary Switch speed 3rd terminal  with 10k Resistor in between
`33` | `GPIO33` | ESP32 to Rotary Switch speed 4th terminal with 10k Resistor in between
`18` | `GPIO18` | ESP32 to Load 1 Switch (In my case Tubelight) with 10k Resistor in between
`17` | `GPIO17` | ESP32 to Load 2 Switch (In my case Bulb) with 10k Resistor in between
`16` | `GPIO16` | ESP32 to Load 3 Switch (In my case Socket 1) with 10k Resistor in between
`13` | `GPIO13` | ESP32 to Load 4 Switch (In my case Socket 2) with 10k Resistor in between
`14` | `GPIO14` | ESP32 to Load 5 Switch (In my case Socket 3) with 10k Resistor in between
`23` | `GPIO23` | ESP32 to Relay 1 
`22` | `GPIO22` | ESP32 to Relay 2
`21` | `GPIO21` | ESP32 to Relay 3
`19` | `GPIO19` | ESP32 to Relay 4
`5` | `GPIO5` | ESP32 to Relay 5
`25` | `GPIO25` | ESP32 to Relay 6
`26` | `GPIO26` | ESP32 to Relay 7
`27` | `GPIO27` | ESP32 to Relay 8
`5v` | `5v (Vin)` | 5v to ESP32, Relay, Switches, Rotary Switch
`GND` | `GND` | GND to ESP32, Relay, Switches, Rotary Switch

### Further Relay Connection with Home Appliances

Relay | Further Connection 
------------ | ------------- 
`Relay 1` | Relay 1 to Load 1 (In my case Tubelight) 
`Relay 2` | Relay 2 to Load 2 (In my case Bulb) 
`Relay 3` | Relay 3 to Load 3 (In my case Socket 1) 
`Relay 4` | Relay 4 to Load 4 (In my case Socket 2) 
`Relay 5` | Relay 5 to Load 5 (In my case Socket 3) 
`Relay 6` | Relay 6 to Load 6 with Capacitive dimming circuit (2.2u.F) (In my case Fan Speed 1) 
`Relay 7` | Relay 7 to Load 6 with Capacitive dimming circuit (3.3u.F) (In my case Fan Speed 2) 
`Relay 8` | Relay 8 to Load 6 (In my case Fan Speed 4) 

## Code

### Uploading Code

* Attach your ESP32 With your PC
* Open [above code](https://github.com/saurabhlanje199/Complete-Home-Automation-with-Realtime-Feedback/blob/main/ESP32/Code_ESP32.ino) in **Arduino Ide**.
  * Go to **Tools**:
     * Go to **Boards**
     * Go to **ESP32 Arduino**
     * Select Your Board
  * Again go to **Tools**:
     * Go to **Port**:
     * Select Your Port
  * Click **Upload** and you are ready to go

### ESP32 Code

Code is divided into two parts:
With Internet and without internet

Further, both with Internet and without internet are divided into for Switches and for Fan.

Let’s begin with code:

#### Libraries Included

```
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
BlynkTimer timer;
```
#### Input-Output Pins Defined

```
#define DEBUG_SW 1

// Pins for Fan Regulator Knob
#define s1  34 //IO34
#define s2  35 //IO35
#define s3  32 //IO32
#define s4  33 //IO33

// Pins for Switches
#define S5 18  //IO18
#define S6 17  //IO17
#define S7 16  //IO16
#define S8 13  //IO13
#define S9 14  //IO14

// Pins for Relay (Appliances Control)
#define R5 23  //IO23
#define R6 22  //IO22
#define R7 21  //IO21
#define R8 19  //IO19
#define R9 5   //IO5


// Pins for Relay (Fan Speed Control)
#define Speed1 25  //IO25
#define Speed2 26  //IO26
#define Speed4 27  //IO27
```


#### Mode 
(By default the mode is 0 with internet connected)
```
int MODE = 0;
```


#### Authentication Token
(You should get Auth Token in the Blynk App or in Gmail.
Else go to the Project Settings (nut icon))
```
char auth[] = "AUTH TOKEN";
```

#### WiFi Credentials
(Set password to "" for open networks.)
```
char ssid[] = "WIFI NAME";
char pass[] = "PASS";
```

#### Flag Define
```
bool speed1_flag = 1;
bool speed2_flag = 1;
bool speed3_flag = 1;
bool speed4_flag = 1;
bool speed0_flag = 1;

int switch_ON_Flag1_previous_I = 0;
int switch_ON_Flag2_previous_I = 0;
int switch_ON_Flag3_previous_I = 0;
int switch_ON_Flag4_previous_I = 0;
int switch_ON_Flag5_previous_I = 0;
```

#### Assigning Incoming Values from Blynk app to Variables
```
BLYNK_WRITE(V0)
{
  int fan_speed = param.asInt(); // assigning incoming value from pin V1 to a variable
  if (fan_speed == 0)
  {
    speed0();
  }
  if (fan_speed == 1)
  {
    speed1();
  }
  if (fan_speed == 2)
  {
    speed2();
  }
  if (fan_speed == 3)
  {
    speed3();
  }
  if (fan_speed == 4)
  {
    speed4();
  }
}

BLYNK_WRITE(V1)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V1 to a variable
  digitalWrite(R5, pinValue);
  // process received value
}

BLYNK_WRITE(V2)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V2 to a variable
  digitalWrite(R6, pinValue);
  // process received value
}

BLYNK_WRITE(V3)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V3 to a variable
  digitalWrite(R7, pinValue);
  // process received value
}

BLYNK_WRITE(V4)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V4 to a variable
  digitalWrite(R8, pinValue);
  // process received value
}
BLYNK_WRITE(V5)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V4 to a variable
  digitalWrite(R9, pinValue);
  // process received value
}
```

#### Void Setup
```
void setup()
{
  // put your setup code here, to run once:
  pinMode(s1, INPUT);
  pinMode(s2, INPUT);
  pinMode(s3, INPUT);
  pinMode(s4, INPUT);
  pinMode(S5, INPUT);
  pinMode(S6, INPUT);
  pinMode(S7, INPUT);
  pinMode(S8, INPUT);
  pinMode(S9, INPUT);

  pinMode(R5, OUTPUT);
  pinMode(R6, OUTPUT);
  pinMode(R7, OUTPUT);
  pinMode(R8, OUTPUT);
  pinMode(R9, OUTPUT);
  pinMode(Speed1, OUTPUT);
  pinMode(Speed2, OUTPUT);
  pinMode(Speed4, OUTPUT);

  Serial.begin(9600);
  WiFi.begin(ssid, pass);

  timer.setInterval(3000L, checkBlynk); // check if connected to Blynk server every 3 seconds

  Blynk.config(auth);//, ssid, pass);
}
```

#### WiFi Status 

```
void loop()
{

  if (WiFi.status() != WL_CONNECTED)
  {
    if (DEBUG_SW) Serial.println("Not Connected");
  }
  else
  {
    if (DEBUG_SW) Serial.println(" Connected");
    Blynk.run();
  }
  timer.run(); // Initiates SimpleTimer

  if (MODE == 0)
    with_internet();
  else
    without_internet();
  // put your main code here, to run repeatedly:
}
```

#### With Internet Function
```
void with_internet()
{

   // FOR FAN
  if (digitalRead(s1) == HIGH && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed1_flag == 1)
  {
    speed1();
    Blynk.virtualWrite(V0, 1);
    speed1_flag = 0;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;


  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == HIGH && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed2_flag == 1)
  {
    speed2();
    Blynk.virtualWrite(V0, 2);
    speed1_flag = 1;
    speed2_flag = 0;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;

  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == HIGH && digitalRead(s4) == LOW && speed3_flag == 1)
  {
    speed3();
    Blynk.virtualWrite(V0, 3);
    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 0;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == HIGH  && speed4_flag == 1)
  {
    speed4();
    Blynk.virtualWrite(V0, 4);
    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 0;
    speed0_flag = 1;
  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW  && speed0_flag == 1)
  {
    speed0();
    Blynk.virtualWrite(V0, 0);
    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 0;
  }
  
  
  // FOR SWITCH
  if (digitalRead(S5) == LOW)
  {
    if (switch_ON_Flag1_previous_I == 0 )
    {
      digitalWrite(R5, HIGH);
      if (DEBUG_SW) Serial.println("Relay1- ON");
      Blynk.virtualWrite(V1, 1);
      switch_ON_Flag1_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch1 -ON");
    
  }
  if (digitalRead(S5) == HIGH )
  {
    if (switch_ON_Flag1_previous_I == 1)
    {
      digitalWrite(R5, LOW);
      if (DEBUG_SW) Serial.println("Relay1 OFF");
      Blynk.virtualWrite(V1, 0);
      switch_ON_Flag1_previous_I = 0;
    }
    if (DEBUG_SW)Serial.println("Switch1 OFF");
  }

  if (digitalRead(S6) == LOW)
  {
    if (switch_ON_Flag2_previous_I == 0 )
    {
      digitalWrite(R6, HIGH);
      if (DEBUG_SW)  Serial.println("Relay2- ON");
      Blynk.virtualWrite(V2, 1);
      switch_ON_Flag2_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch2 -ON");

  }
  if (digitalRead(S6) == HIGH )
  {
    if (switch_ON_Flag2_previous_I == 1)
    {
      digitalWrite(R6, LOW);
      if (DEBUG_SW) Serial.println("Relay2 OFF");
      Blynk.virtualWrite(V2, 0);
      switch_ON_Flag2_previous_I = 0;
    }
    if (DEBUG_SW)Serial.println("Switch2 OFF");
    //delay(200);
  }

  if (digitalRead(S7) == LOW)
  {
    if (switch_ON_Flag3_previous_I == 0 )
    {
      digitalWrite(R7, HIGH);
      if (DEBUG_SW) Serial.println("Relay3- ON");
      Blynk.virtualWrite(V3, 1);
      switch_ON_Flag3_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch3 -ON");

  }
  if (digitalRead(S7) == HIGH )
  {
    if (switch_ON_Flag3_previous_I == 1)
    {
      digitalWrite(R7, LOW);
      if (DEBUG_SW) Serial.println("Relay3 OFF");
      Blynk.virtualWrite(V3, 0);
      switch_ON_Flag3_previous_I = 0;
    }
    if (DEBUG_SW)Serial.println("Switch3 OFF");
    //delay(200);
  }

  if (digitalRead(S8) == LOW)
  {
    if (switch_ON_Flag4_previous_I == 0 )
    {
      digitalWrite(R8, HIGH);
      if (DEBUG_SW) Serial.println("Relay4- ON");
      Blynk.virtualWrite(V4, 1);
      switch_ON_Flag4_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch4 -ON");

  }
  if (digitalRead(S8) == HIGH )
  {
    if (switch_ON_Flag4_previous_I == 1)
    {
      digitalWrite(R8, LOW);
      if (DEBUG_SW) Serial.println("Relay4 OFF");
      Blynk.virtualWrite(V4, 0);
      switch_ON_Flag4_previous_I = 0;
    }
    if (DEBUG_SW) Serial.println("Switch4 OFF");
    //delay(200);
  }

 if (digitalRead(S9) == LOW)
  {
    if (switch_ON_Flag5_previous_I == 0 )
    {
      digitalWrite(R9, HIGH);
      if (DEBUG_SW) Serial.println("Relay5- ON");
      Blynk.virtualWrite(V5, 1);
      switch_ON_Flag5_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch5 -ON");

  }
  if (digitalRead(S9) == HIGH )
  {
    if (switch_ON_Flag5_previous_I == 1)
    {
      digitalWrite(R9, LOW);
      if (DEBUG_SW) Serial.println("Relay5 OFF");
      Blynk.virtualWrite(V5, 0);
      switch_ON_Flag5_previous_I = 0;
    }
    if (DEBUG_SW) Serial.println("Switch5 OFF");
    //delay(200);
  }

}
```


#### Without Internet Function
```
void without_internet()
{
// FOR FAN
  if (digitalRead(s1) == HIGH && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed1_flag == 1)
  {
    speed1();

    speed1_flag = 0;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == HIGH && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed2_flag == 1)
  {
    speed2();

    speed1_flag = 1;
    speed2_flag = 0;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == HIGH && digitalRead(s4) == LOW && speed3_flag == 1)
  {
    speed3();

    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 0;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == HIGH  && speed4_flag == 1)
  {
    speed4();

    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 0;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW  && speed0_flag == 1)
  {
    speed0();

    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 0;
  }
  
   // FOR SWITCH
  digitalWrite(R5, !digitalRead(S5));
  digitalWrite(R6, !digitalRead(S6));
  digitalWrite(R7, !digitalRead(S7));
  digitalWrite(R8, !digitalRead(S8));
  digitalWrite(R9, !digitalRead(S9));
}
```

#### Fan Speed Control
```
void speed0()
{
  //All Relays Off - Fan at speed 0
  if (DEBUG_SW)Serial.println("SPEED 0");
  digitalWrite(Speed1, HIGH);
  digitalWrite(Speed2, HIGH);
  digitalWrite(Speed4, HIGH);

}

void speed1()
{
  //Speed1 Relay On - Fan at speed 1
  if (DEBUG_SW)Serial.println("SPEED 1");
  digitalWrite(Speed1, HIGH);
  digitalWrite(Speed2, HIGH);
  digitalWrite(Speed4, HIGH);
  delay(500);
  digitalWrite(Speed1, LOW);
}

void speed2()
{
  //Speed2 Relay On - Fan at speed 2
  if (DEBUG_SW)Serial.println("SPEED 2");
  digitalWrite(Speed1, HIGH);
  digitalWrite(Speed2, HIGH);
  digitalWrite(Speed4, HIGH);
  delay(500);
  digitalWrite(Speed2, LOW);
}

void speed3()
{
  //Speed1 & Speed2 Relays On - Fan at speed 3
  if (DEBUG_SW)Serial.println("SPEED 3");
  digitalWrite(Speed1, HIGH);
  digitalWrite(Speed2, HIGH);
  digitalWrite(Speed4, HIGH);
  delay(500);
  digitalWrite(Speed1, LOW);
  digitalWrite(Speed2, LOW);
}

void speed4()
{
  //Speed4 Relay On - Fan at speed 4
  if (DEBUG_SW)Serial.println("SPEED 4");
  digitalWrite(Speed1, HIGH);
  digitalWrite(Speed2, HIGH);
  digitalWrite(Speed4, HIGH);
  delay(500);
  digitalWrite(Speed4, LOW);
}
```

#### Check Blynk 
(To check Connectivity every 3 seconds)
```
void checkBlynk()
{
  bool isconnected = Blynk.connected();
  if (isconnected == false)
  {
    MODE = 1;
  }
  if (isconnected == true)
  {
    MODE = 0;
  }
}
```
## Support
Reach out to me for project-related queries or PCB and save plenty of time and resources and get the project ready to be used, at **saurabhlanje20@gmail.com**

Also if you use this project, have learned something from it, or just like it, please consider supporting it by buying some Electronics Components, so I can dedicate more time on Electronics projects like this :)


<div align="center">
<p><a href="https://www.buymeacoffee.com/saurabhlanje"> <img align="center" src="https://raw.githubusercontent.com/aniketmarwade/Home-Automation-2021-FInal-Year-Project/main/IMAGES/saurabh/electronic_buy.png" height="55" width="300" alt="saurabhlanje" /></a></p><br><br>
</div>

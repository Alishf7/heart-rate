**![](media/7b053b688726d82af397febc337a9454.png)**

**King Faisal University**

## **Heart Beat Sensor**

Project Report

**embedded systems**

| **Name**            | **ID**     |
|---------------------|------------|
| Ali Jamal Alshawaf  | 221422614  |

**Second Semester 2024**

**Introduction**

In this article, we are going to make a **heart beat sensor** using Arduino UNO. You can find the **BPM monitors** on your smartwatches and hospitals. Just like them, we tried to make it using Arduino and pulse sensor.

We also add a 16×2 LCD display on which you can see the values calculated by the **pulse sensor**.

**Description**

-   If you are not familiar with the working of a [pulse sensor with Arduino](https://techatronic.com/how-to-make-heart-rate-monitor/) then please check it here.
-   You just have to put your finger on the [sensor](https://techatronic.com/?s=sensor) and the Arduino will automatically calculate your heartbeat rate.
-   For displaying the readings we are using a [16×2 LCD](https://techatronic.com/interface-lcd-with-arduino-16x2/) here.
-   At first a message ‘put your finger here’ is displayed and when you put your finger on it then it starts displaying BPM with a heart icon.
-   The [LED](https://techatronic.com/diode/) will turn on and start flashing.
-   You can also check the [**heart rate** ](https://techatronic.com/how-to-make-heart-rate-monitor/)**sensor** **using Arduino** made by us.

![heart beat sensor bpm monitor](media/c19c8e05bfb7f9ec965980ae963c70bb.jpeg) ![heart beat sensor bpm monitor](media/da9ff850bfcac9da95a3c451ac23e24c.jpeg)

## Components Required

-   [Arduino UNO](https://techatronic.com/what-is-arduino-brief-description/)
-   [Pulse sensor](https://techatronic.com/?s=sensor)
-   [LED](https://techatronic.com/diode/)
-   [220 ohm resistor](https://techatronic.com/what-is-a-resistor-and-its-basic-type/)
-   [I2C module](https://techatronic.com/lcd-interfacing-with-arduino-using-i2c/)
-   [16×2 LCD](https://techatronic.com/interface-lcd-with-arduino-16x2/)
-   Jumper wires and a breadboard
-   USB cable for uploading the code

## Circuit for Heart Beat Sensor BPM Monitor

-   Take a **pulse sensor** and connect its VCC pin with the 5 volt pin of the [Arduino](https://techatronic.com/types-of-arduino-boards-arduino-uno-mega-mini-specification/).
-   Join the GND pin of the pulse sensor with the GND pin of the Arduino.
-   Attach the OUT/signal pin of the heart beat sensor to the Analog-0 pin of the Arduino.
-   Now take an LED and connect its positive leg with the digital-3 pin of the Arduino.
-   Join the negative leg of the LED with the GND pin of the Arduino via a [220 ohm resisto](https://techatronic.com/what-is-a-resistor-and-its-basic-type/)r.
-   Then Connect the I2C module with the 16×2 LCD module.
-   You can also check the interfacing of the [I2C module with Arduino](https://techatronic.com/lcd-interfacing-with-arduino-using-i2c/).
-   Join the VCC pin of the I2C module with the 5 volt pin of the Arduino and the GND pin of the Arduino with the GND pin of the I2C module.
-   Connect the SDA and SCK pins of the [I2C module](https://techatronic.com/i2c-scanner/) with the analog-4 and analog-5 pins of the Arduino as shown in the diagram.
-   Make sure that the connections are correct and tight.

## Code for **Heart Beat Sensor** BPM Monitor

![heart beat sensor circuit diagram](media/f2b49072ac76908c0a2c75e667de3afc.jpeg)

**NOTE**: Please [upload](https://techatronic.com/how-to-operate-arduino-software-tutorial-1/) this code to the Arduino. You have to install \<[PulseSensorPlayground.h](https://github.com/WorldFamousElectronics/PulseSensorPlayground)\> and \<[LiquidCrystal_I2C.h](https://github.com/fdebrabander/Arduino-LiquidCrystal-I2C-library)\> libraries first. If you don’t know how to [add zip libraries to the Arduino IDE](https://techatronic.com/include-libraries-in-arduino-ide/) then go through it first.

The code:

\#define USE_ARDUINO_INTERRUPTS true //--\> Set-up low-level interrupts for most acurate BPM math.

\#include \<PulseSensorPlayground.h\> //--\> Includes the PulseSensorPlayground Library.

\#include \<LiquidCrystal_I2C.h\> //--\> Includes the LiquidCrystal Library.

LiquidCrystal_I2C lcd(0x27,16,2);

const int PulseWire = 0; //--\> PulseSensor PURPLE WIRE connected to ANALOG PIN 0

int LED_3 = 3; //--\> LED to detect when the heart is beating. The LED is connected to PIN 3 on the Arduino UNO.

int Threshold = 550; //--\> Determine which Signal to "count as a beat" and which to ignore.

//--\> Use the "Gettting Started Project" to fine-tune Threshold Value beyond default setting.

//--\> Otherwise leave the default "550" value.

byte heart1[8] = {B11111, B11111, B11111, B11111, B01111, B00111, B00011, B00001};

byte heart2[8] = {B00011, B00001, B00000, B00000, B00000, B00000, B00000, B00000};

byte heart3[8] = {B00011, B00111, B01111, B11111, B11111, B11111, B11111, B01111};

byte heart4[8] = {B11000, B11100, B11110, B11111, B11111, B11111, B11111, B11111};

byte heart5[8] = {B00011, B00111, B01111, B11111, B11111, B11111, B11111, B11111};

byte heart6[8] = {B11000, B11100, B11110, B11111, B11111, B11111, B11111, B11110};

byte heart7[8] = {B11000, B10000, B00000, B00000, B00000, B00000, B00000, B00000};

byte heart8[8] = {B11111, B11111, B11111, B11111, B11110, B11100, B11000, B10000};

//----------------------------------------

int Instructions_view = 500; //--\> Variable for waiting time to display instructions on LCD.

PulseSensorPlayground pulseSensor; //--\> Creates an instance of the PulseSensorPlayground object called "pulseSensor"

//--------------------------------------------------------------------------------void setup

void setup() {

Serial.begin(9600);//--\> Set's up Serial Communication at certain speed.

lcd.init(); //--\> Initializes the interface to the LCD screen, and specifies the dimensions (width and height) of the display

lcd.backlight();

//----------------------------------------Create a custom character (glyph) for use on the LCD

lcd.createChar(1, heart1);

lcd.createChar(2, heart2);

lcd.createChar(3, heart3);

lcd.createChar(4, heart4);

lcd.createChar(5, heart5);

lcd.createChar(6, heart6);

lcd.createChar(7, heart7);

lcd.createChar(8, heart8);

//----------------------------------------

lcd.setCursor(0,0);

lcd.print(" HeartBeat Rate ");

lcd.setCursor(0,1);

lcd.print(" Monitoring ");

//----------------------------------------Configure the PulseSensor object, by assigning our variables to it.

pulseSensor.analogInput(PulseWire);

pulseSensor.blinkOnPulse(LED_3); //--\> auto-magically blink Arduino's LED with heartbeat.

pulseSensor.setThreshold(Threshold);

//----------------------------------------

//----------------------------------------Double-check the "pulseSensor" object was created and "began" seeing a signal.

if (pulseSensor.begin()) {

Serial.println("We created a pulseSensor Object !"); //--\> This prints one time at Arduino power-up, or on Arduino reset.

}

//----------------------------------------

delay(2000);

lcd.clear();

}

//--------------------------------------------------------------------------------

//--------------------------------------------------------------------------------void loop

void loop() {

int myBPM = pulseSensor.getBeatsPerMinute(); //--\> Calls function on our pulseSensor object that returns BPM as an "int". "myBPM" hold this BPM value now.

//----------------------------------------Condition if the Sensor does not detect the heart rate / the sensor is not touched.

if (Instructions_view \< 100) {

Instructions_view++;

}

if (Instructions_view \> 99) {

lcd.setCursor(0,0);

lcd.print("Put your finger ");

lcd.setCursor(0,1);

lcd.print("on the sensor ");

delay(1000);

lcd.clear();

delay(500);

}

//----------------------------------------

//----------------------------------------Constantly test to see if "a beat happened".

if (pulseSensor.sawStartOfBeat()) { //--\> If test is "true", then the following conditions will be executed.

Serial.println("♥ A HeartBeat Happened ! "); //--\> Print a message "a heartbeat happened".

Serial.print("BPM: "); //--\> Print phrase "BPM: "

Serial.println(myBPM/3); //--\> Print the value inside of myBPM.

//----------------------------------------Displays a "Heart" shape on the LCD.

lcd.setCursor(1,1);

lcd.write(byte(1));

lcd.setCursor(0,1);

lcd.write(byte(2));

lcd.setCursor(0,0);

lcd.write(byte(3));

lcd.setCursor(1,0);

lcd.write(byte(4));

lcd.setCursor(2,0);

lcd.write(byte(5));

lcd.setCursor(3,0);

lcd.write(byte(6));

lcd.setCursor(3,1);

lcd.write(byte(7));

lcd.setCursor(2,1);

lcd.write(byte(8));

//----------------------------------------

//----------------------------------------Displays the BPM value on the LCD.

lcd.setCursor(5,0);

lcd.print("Heart Rate");

lcd.setCursor(5,1);

lcd.print(": ");

lcd.print(myBPM/3);

lcd.print(" ");

lcd.print("BPM ");

//----------------------------------------

Instructions_view = 0;

}

//----------------------------------------

delay(20); //--\> considered best practice in a simple sketch.

}

//--------------------------------------------------------------------------------

//===============================================================================================

We hope that you liked this project and understand it’s working of **heart beat sensor** completely.

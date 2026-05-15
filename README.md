aws pass HAPPYbirthday@123

https://chatgpt.com/share/6a061039-7d70-8320-af97-1cb16eedf1e7


Experiment 2: Automatic Street Light using LDR with Arduino

1. AIM

To interface an LDR with Arduino and automatically control a street light based on light intensity.

LDR Connections

| Component                   | Connected To         |
| --------------------------- | -------------------- |
| One side of LDR             | 5V                   |
| Other side of LDR           | A0 and 10kΩ resistor |
| Other side of 10kΩ resistor | GND                  |


LED Connections
| Arduino         | Component     |
| --------------- | ------------- |
| Digital Pin 3   | 470Ω resistor |
| Resistor        | LED Anode (+) |
| LED Cathode (-) | GND           |

int ldr = A0;
int value = 0;

void setup() {
  Serial.begin(9600);
  pinMode(3, OUTPUT);
}

void loop() {

  value = analogRead(ldr);

  Serial.println("LDR value is :");
  Serial.println(value);

  if(value < 300)
  {
    digitalWrite(3, HIGH);
  }
  else
  {
    digitalWrite(3, LOW);
  }
}


======================================================================================

Experiment 3 : Monitoring the voltage level of the battery and indicating the same using
 multiple LED's & OLED with Arduino/ESP32/Raspberry Pi.

#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

int batteryPin = A0;

int redLED = 3;
int yellowLED = 4;
int greenLED = 5;

float voltage = 0.0;

void setup()
{
    pinMode(redLED, OUTPUT);
    pinMode(yellowLED, OUTPUT);
    pinMode(greenLED, OUTPUT);

    Serial.begin(9600);

    if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C))
    {
        Serial.println("OLED not found");
        while(true);
    }

    display.clearDisplay();
}

void loop()
{
    int sensorValue = analogRead(batteryPin);

    voltage = sensorValue * (5.0 / 1023.0);

    Serial.print("Battery Voltage: ");
    Serial.println(voltage);

    display.clearDisplay();

    display.setTextSize(2);
    display.setTextColor(WHITE);

    display.setCursor(0,10);
    display.print("Voltage:");

    display.setCursor(0,40);
    display.print(voltage);

    display.display();

    if(voltage > 4.0)
    {
        digitalWrite(greenLED, HIGH);
        digitalWrite(yellowLED, LOW);
        digitalWrite(redLED, LOW);
    }
    else if(voltage > 3.0)
    {
        digitalWrite(greenLED, LOW);
        digitalWrite(yellowLED, HIGH);
        digitalWrite(redLED, LOW);
    }
    else
    {
        digitalWrite(greenLED, LOW);
        digitalWrite(yellowLED, LOW);
        digitalWrite(redLED, HIGH);
    }

    delay(1000);
}

Complete Hardware Connections

| Arduino UNO | Connected To     |
| ----------- | ---------------- |
| A0          | Battery Positive |
| GND         | Battery Negative |
| Pin 3       | Red LED          |
| Pin 4       | Yellow LED       |
| Pin 5       | Green LED        |
| A4          | OLED SDA         |
| A5          | OLED SCL         |
| 5V          | OLED VCC         |
| GND         | OLED GND         |

=================================================================================

Experiment 4: Generatation of a random value similar to dice game and display the same using a 16X2 LCD with Arduino/ESP32/Raspberry Pi

#include <LiquidCrystal.h>

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

int diceValue;

void setup()
{
    lcd.begin(16, 2);

    randomSeed(analogRead(0));

    lcd.print("Dice Game");
    delay(2000);

    lcd.clear();
}

void loop()
{
    diceValue = random(1, 7);

    lcd.clear();

    lcd.setCursor(0,0);
    lcd.print("Dice Value:");

    lcd.setCursor(0,1);
    lcd.print(diceValue);

    delay(2000);
}

Complete Hardware Connections

| Arduino UNO Pin | Connected To      |
| --------------- | ----------------- |
| 12              | LCD RS            |
| 11              | LCD E             |
| 5               | LCD D4            |
| 4               | LCD D5            |
| 3               | LCD D6            |
| 2               | LCD D7            |
| 5V              | LCD VDD           |
| GND             | LCD VSS           |
| GND             | LCD RW            |
| 5V              | LCD Backlight     |
| GND             | LCD Backlight GND |

=======================================================================================

Experiment 5: Print temperature and humidity using DHT22 sensor with Raspberry Pi /Arduino/ESP32

#include "DHT.h"

#define DHTPIN 2
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);

void setup()
{
    Serial.begin(9600);

    dht.begin();

    Serial.println("DHT22 Sensor Reading");
}

void loop()
{
    float humidity = dht.readHumidity();

    float temperature = dht.readTemperature();

    if (isnan(humidity) || isnan(temperature))
    {
        Serial.println("Failed to read from DHT sensor!");
        return;
    }

    Serial.print("Humidity: ");
    Serial.print(humidity);
    Serial.println("%");

    Serial.print("Temperature: ");
    Serial.print(temperature);
    Serial.println(" °C");

    delay(2000);
}

| Arduino UNO Pin | Connected To |
| --------------- | ------------ |
| 5V              | DHT22 VCC    |
| Pin 2           | DHT22 DATA   |
| GND             | DHT22 GND    |


===========================================================================================
Experiment 6: Controlling of PIR based LED ON/OFF 

int pirPin = 2;
int ledPin = 13;

int motion = 0;

void setup()
{
    pinMode(pirPin, INPUT);

    pinMode(ledPin, OUTPUT);

    Serial.begin(9600);

    Serial.println("PIR Motion Detection");
}

void loop()
{
    motion = digitalRead(pirPin);

    if(motion == HIGH)
    {
        digitalWrite(ledPin, HIGH);

        Serial.println("Motion Detected");
    }
    else
    {
        digitalWrite(ledPin, LOW);

        Serial.println("No Motion");
    }

    delay(1000);
}

| Arduino UNO Pin | Connected To |
| --------------- | ------------ |
| 5V              | PIR VCC      |
| Pin 2           | PIR OUT      |
| GND             | PIR GND      |
| Pin 13          | LED          |
| GND             | LED Cathode  |


===============================================================================

Experiment 8: Measure the distance using Ultra sonic sensor

| Arduino UNO Pin | Connected To |
| --------------- | ------------ |
| 5V              | HC-SR04 VCC  |
| GND             | HC-SR04 GND  |
| Pin 9           | HC-SR04 TRIG |
| Pin 10          | HC-SR04 ECHO |

int trigPin = 9;
int echoPin = 10;

long duration;
float distance;

void setup()
{
    pinMode(trigPin, OUTPUT);

    pinMode(echoPin, INPUT);

    Serial.begin(9600);

    Serial.println("Ultrasonic Distance Measurement");
}

void loop()
{
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);

    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);

    digitalWrite(trigPin, LOW);

    duration = pulseIn(echoPin, HIGH);

    distance = duration * 0.034 / 2;

    Serial.print("Distance: ");

    Serial.print(distance);

    Serial.println(" cm");

    delay(1000);
}


=========================================================================

Experiment 9: Perform image capture, read, write, and modify the image operation using Pi camera with Raspberry Pi

Program 1: Capture Image using Pi Camera
from picamera import PiCamera
from time import sleep

camera = PiCamera()

camera.rotation = 180

camera.start_preview()

sleep(5)

camera.capture('/home/pi/Desktop/image.jpg')

camera.stop_preview()

print("Image Captured Successfully")



Program 2: Modify Image using Pi Camera
from picamera import PiCamera, Color
from time import sleep

camera = PiCamera()

camera.rotation = 180

camera.start_preview()

camera.image_effect = 'watercolor'

camera.annotate_background = Color('yellow')

camera.annotate_foreground = Color('blue')

camera.annotate_text_size = 80

camera.annotate_text = "Hello KMIT"

sleep(5)

camera.capture('/home/pi/Desktop/modified.jpg')

camera.stop_preview()

print("Modified Image Captured")



Program 3: Video Recording using Pi Camera
from picamera import PiCamera, Color
from time import sleep

camera = PiCamera()

camera.rotation = 180

camera.start_preview()

camera.annotate_background = Color('yellow')

camera.annotate_foreground = Color('blue')

camera.annotate_text_size = 80

camera.annotate_text = "KMIT IT"

camera.start_recording('/home/pi/Desktop/video.h264')

sleep(5)

camera.stop_recording()

camera.stop_preview()

print("Video Recorded Successfully")


===================================================================================================

LED ON/OFF USING RASPBERRY PI

Controlling LED ON/OFF PROGRAM

import RPi.GPIO as GPIO

import time

GPIO.setmode(GPIO.BCM)

GPIO.setup(18,GPIO.OUT)

while True:

GPIO.output(18,True)

time.sleep(2)

GPIO.output(18,False)

time.sleep(2)

======================================================================================

INTEGRATION OF LED AND SWITCH

Interfacing: Connect one terminal of the push button to a digital pin on the Arduino (e.g., Pin 2).
Connect the other terminal of the push button to the ground (GND) of the Arduino.
Connect a resistor (typically 10k ohms) from the same digital pin (e.g., Pin 2) to
the 5V supply (to create an external pull-up resistor).

const int buttonPin = 5; // Pin connected to the push button
const int ledPin = 13; // Pin connected to an LED
void setup() {
pinMode(buttonPin, INPUT_PULLUP); // Set buttonPin as an input
pinMode(ledPin, OUTPUT); // Set ledPin as an output
Serial.begin(9600);
}
void loop() {

11

int buttonState = digitalRead(buttonPin); // Read the state of the button
Serial.println(buttonState);
if (buttonState == HIGH)
{
digitalWrite(ledPin, HIGH); // Turn on the LED when the button is pressed
}
else
{
digitalWrite(ledPin, LOW); // Turn off the LED when the button is not
pressed
}
}

https://chatgpt.com/c/699c0a1a-677c-8322-9223-67767598835b

aws pass-HAPPYbirthday@123

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

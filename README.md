# I-THE-FARMER CODE FOR RASPBERRY

import RPi.GPIO as A
import Adafruit_ADS1x15
import time
import urllib2
from gpiozero import LightSensor
adc=Adafruit_ADS1x15.ADS1115()
A.setwarnings(False)
A.setmode(A.BCM)
A.setup(18,A.OUT)
A.setup(23,A.IN)
A.setup(21,A.IN)
A.setup(4,A.IN)
ldr = LightSensor(4)
while True:
    A.output(18,True)
    r=A.input(23)
    if r==0:
        A.output(18,False)
        print("Raining --> Motor off")
        time.sleep(1)
    m= A.input(21)
    print(m)
    if m==0:
        A.output(18,False)
        print("Moisture Sufficient --> Motor off")
        time.sleep(1)
    val=ldr.value*1000
    print(val)
    if int(val)==0:
        A.output(18,False)
        print("Thunder detected --> waiting for clearence")
        ##urlm="http://griet.in/services/sendalert.php?message=thunder&mobile=XXXXXXXXXX"
        ##f=urllib2.urlopen(url)
        ##f.close()
        time.sleep(5)
        print("your device is safe from thunder")
    url="https://api.thingspeak.com/update?api_key=JIJMPZCSLPIJT5QA&field1="+str(val)+"&field2="+str(r)+"&field3="+str(m)
    print(url)
    f=urllib2.urlopen(url)
    f.close()

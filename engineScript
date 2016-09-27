#!/usr/bin/python3

import paho.mqtt.client as mqtt
import ssl
import json,time
import RPi.GPIO as GPIO

GPIO.setwarnings(True)


def on_connect(client, userdata, flags, rc):
    print("Connected with result code "+str(rc))
    client.subscribe("$aws/things/piTractor/shadow/update/accepted")

# Set all relays to off
GPIO.setmode(GPIO.BCM)
pinList = [26, 19, 13, 6, 5, 21, 20, 16]
GPIO.setup(pinList, GPIO.OUT)
GPIO.output(pinList, GPIO.HIGH)
GPIO.cleanup

#Listen for JSON message with tractor function requested
def on_message(client, userdata, msg):
    message_json = json.loads(msg.payload.decode())
    GPIO.setmode(GPIO.BCM)
    try:
        if message_json['state']['desired']['function'] == "forward":
             print("Moving forward")
             GPIO.setup(21, GPIO.OUT)
             GPIO.output(21,GPIO.LOW)
             time.sleep(0.2)
             GPIO.output(21,GPIO.HIGH)
        elif message_json['state']['desired']['function'] == "reverse":
             print("Moving backward")
             GPIO.setup(16, GPIO.OUT)
             GPIO.output(16,GPIO.LOW)
             time.sleep(0.2)
             GPIO.output(16,GPIO.HIGH)
        elif message_json['state']['desired']['function'] == "blade":
            print("Moving the blade")
            GPIO.setup(6, GPIO.OUT)
            GPIO.output(6,GPIO.LOW)
            time.sleep(0.2)
            GPIO.output(6,GPIO.HIGH)
        elif message_json['state']['desired']['function'] == "song":
             print("Playing the tractor song!")
             GPIO.setup(20, GPIO.OUT)
             GPIO.output(20,GPIO.LOW)
             time.sleep(0.2)
             GPIO.output(20,GPIO.HIGH)
        elif message_json['state']['desired']['function'] == "honk":
             print("Honking")
             GPIO.setup(5, GPIO.OUT)
             GPIO.output(5,GPIO.LOW)
             time.sleep(0.2)
             GPIO.output(5,GPIO.HIGH)
    except KeyboardInterrupt:
        print ("Quit")


#Connect to AWS IoT
client = mqtt.Client(client_id="tractorRasp")
client.on_connect = on_connect
client.on_message = on_message
client.tls_set(ca_certs='/home/pi/root-CA.pem', certfile='/home/pi/xxxxx-certificate.pem.crt', keyfile='/home/pi/xxxxx-private.pem.key', tls_version=ssl.PROTOCOL_SSLv23)
client.tls_insecure_set(True)
client.connect("xxxxx.iot.us-west-2.amazonaws.com", 8883, 30)
client.loop_forever()

**the raspberry pi bomb**


Connect the SIM800L GSM Module to the Raspberry Pi:

Connect the SIM800L's VCC to the Raspberry Pi's 5V pin.

Connect the SIM800L's GND to the Raspberry Pi's GND pin.

Connect the SIM800L's RXD to the Raspberry Pi's TXD pin (GPIO 14).

Connect the SIM800L's TXD to the Raspberry Pi's RXD pin (GPIO 15).

Connect the Relay Module to the Raspberry Pi:

Connect the Relay Module's VCC to the Raspberry Pi's 5V pin.

Connect the Relay Module's GND to the Raspberry Pi's GND pin.

Connect the Relay Module's IN pin to a GPIO pin (e.g., GPIO 18).

Connect the Detonator to the Relay Module:

Connect the detonator's positive terminal to the Relay Module's COM (Common) pin.

Connect the detonator's negative terminal to the Relay Module's NO (Normally Open) pin.

**the script**

```import serial
import RPi.GPIO as GPIO
import time

# Define the GPIO pin for the relay
relay_pin = 18

# Initialize the GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setup(relay_pin, GPIO.OUT)
GPIO.output(relay_pin, GPIO.LOW)

# Initialize the serial connection to the SIM800L
ser = serial.Serial('/dev/ttyS0', 9600, timeout=1)
ser.flush()

# Function to check for incoming calls
def check_for_calls():
    ser.write(b'AT+CLCC\r\n')
    response = ser.read(100)
    if b'+CLCC: 1,0,0,0,0,"1234567890",129' in response:  # Replace with your phone number
        return True
    return False

# Function to trigger the bomb
def trigger_bomb():
    GPIO.output(relay_pin, GPIO.HIGH)
    time.sleep(2)  # Adjust the delay as needed
    GPIO.output(relay_pin, GPIO.LOW)

# Main loop
try:
    while True:
        if check_for_calls():
            trigger_bomb()
            break
        time.sleep(1)
except KeyboardInterrupt:
    GPIO.cleanup()```


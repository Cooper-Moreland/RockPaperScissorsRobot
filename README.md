# RockPaperScissorsRobot

## Code

'''python
import board
from digitalio import DigitalInOut, Direction, Pull
import digitalio
import time
import pwmio
from adafruit_motor import servo        
import random
from random import randint      #imports

led = digitalio.DigitalInOut(board.D13)      #led in pin 8
led.direction = digitalio.Direction.OUTPUT      #led as output
btn0 = DigitalInOut(board.D3)
btn0.direction = Direction.INPUT
btn0.pull = Pull.UP
pwm1 = pwmio.PWMOut(board.D9, frequency = 50)        #continuous servo pin and freq
pwm2 = pwmio.PWMOut(board.D8, frequency = 50)

prev_state = btn0.value
servo_1 = servo.ContinuousServo(pwm1)
servo_2 = servo.ContinuousServo(pwm2)
 
while True:
    cur_state = btn0.value       #btn0 outputs its current val
    if cur_state != prev_state:     #if current state isn't previous state
        if not cur_state:       #if button pressed
            print("btn0 is down")
            led.value = True
            time.sleep(0.6)
            led.value = False
            time.sleep(0.6)
            led.value = True
            time.sleep(0.6)
            led.value = False
            time.sleep(0.6)
            led.value = True
            time.sleep(0.6)
            led.value = False       #blink led 3 times
            r1 = random.randint(1, 3)       #produce a random integer between and including 1 - 3
            if r1 == 1:
                print("ROCK")
                servo_1.throttle = 0.13
                servo_2.throttle = 0.28
                time.sleep(1.0)
                servo_1.throttle = 0
                servo_2.throttle = 0
                time.sleep(5.0)     #turn servo for 1 second then stop for 5 seconds
                servo_1.throttle = -0.115
                servo_2.throttle = -0.185
                time.sleep(1.5)
                servo_1.throttle = 0        #turn the servo back to start position
                servo_2.throttle = 0
            if r1 == 2:
                print("SCISSORS")
                servo_1.throttle = 0.13
                servo_2.throttle = 0
                time.sleep(1.0)
                servo_1.throttle = 0
                time.sleep(5.0)     #turn servo for 1 second then stop for 5 seconds
                servo_1.throttle = -0.115
                time.sleep(1.5)
                servo_1.throttle = 0        #turn the servo back to start position
            if r1 == 3:
                print("PAPER")
                servo_1.throttle = 0
                servo_2.throttle = 0        #servos at rest
        else:
            print("btn0 is up")      #if button isn't pressed
            led.value = False       #led off
            servo_1.throttle = 0
            servo_2.throttle = 0        #servos at rest

    prev_state = cur_state      #make the button sticky
    
    '''

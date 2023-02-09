# RockPaperScissorsRobot

## [Planning Document](https://docs.google.com/document/d/1lytFuNmM_0qm5QYV4Yy-9biDfxlLaukL8A0iCGQVoEs/edit?usp=sharing)

## [Onshape Document not included yet](google.com)

## CAD Renderings

![frank](https://github.com/Cooper-Moreland/RockPaperScissorsRobot/blob/main/Screenshot%202023-02-07%20102128.png?raw=true)

![frank](https://github.com/Cooper-Moreland/RockPaperScissorsRobot/blob/main/Screenshot%202023-02-07%20102153.png?raw=true)

## Images

## Materials Used

## Wiring Diagram

![frank](https://github.com/Cooper-Moreland/RockPaperScissorsRobot/blob/main/Screenshot%202023-02-09%20094446.png?raw=true)

## Code

```python
import board
from digitalio import DigitalInOut, Direction, Pull
import digitalio
import time
import pwmio
from adafruit_motor import servo        
import random
from random import randint
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface       #imports

i2c = board.I2C()
led = digitalio.DigitalInOut(board.D13)      #led in pin 8
led.direction = digitalio.Direction.OUTPUT      #led as output
btn0 = DigitalInOut(board.D3)
btn0.direction = Direction.INPUT
btn0.pull = Pull.UP
btn1 = DigitalInOut(board.D2)
btn1.direction = Direction.INPUT
btn1.pull = Pull.UP
btn2 = DigitalInOut(board.D4)
btn2.direction = Direction.INPUT
btn2.pull = Pull.UP
btn3 = DigitalInOut(board.D5)
btn3.direction = Direction.INPUT
btn3.pull = Pull.UP                             #all 4 buttons as inputs
pwm1 = pwmio.PWMOut(board.D9, frequency = 50)        
pwm2 = pwmio.PWMOut(board.D8, frequency = 50)       #continuous servo pin and freq

lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)      #use 0x3f if not working at 0x27
prev_state1 = btn1.value        #player ROCK input
prev_state2 = btn2.value        #player SCISSORS input
prev_state3 = btn3.value        #player PAPER input
lcd.backlight = True
prev_state0 = btn0.value
servo_1 = servo.ContinuousServo(pwm1)
servo_2 = servo.ContinuousServo(pwm2)
 
while True:
    cur_state0 = btn0.value       #btn0 outputs its current val
    if cur_state0 != prev_state0:     #if current state isn't previous state
        if not cur_state0:       #if button pressed
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
                cur_state2 = btn2.value       #btn2 outputs its current val
                if cur_state2 != prev_state2:     #if current state isn't previous state
                    if not cur_state2:
                        u1 = random.randint(1,10)        #random number
                        if u1 == 1:
                            lcd.print("Your mom looks    like a pig")
                        if u1 == 2:
                            lcd.print("Your parents don'tlove u")
                        if u1 == 3:
                            lcd.print("You're maidenless")
                        if u1 == 4:
                            lcd.print("scanning...braincell count: 0")
                        if u1 == 5:
                            lcd.print("u humans really suck at this")       
                        if u1 == 6:
                            lcd.print("You're a walking disaster")
                        if u1 == 7:
                            lcd.print("ur as smart as a mcchicken")
                        if u1 == 8:
                            lcd.print("You never stood a chance")
                        if u1 == 9:
                            lcd.print("I saw all 14,000,605 outcomes")
                        if u1 == 10:
                            lcd.print("ur a lower life form after all")
                        time.sleep(6)
                    else:
                        lcd.clear()
                cur_state1 = btn1.value       #btn1 outputs its current val
                if cur_state1 != prev_state1:     #if current state isn't previous state
                    if not cur_state1:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(6)
                        lcd.clear()
                else:
                    lcd.clear()
                cur_state3 = btn3.value       #btn3 outputs its current val
                if cur_state3 != prev_state3:     #if current state isn't previous state
                    if not cur_state3:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(6)
                        lcd.clear()
                else:
                    lcd.clear()
                time.sleep(3.0)     #turn servo for 1 second then stop for 5 seconds
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
                cur_state3 = btn3.value       #btn3 outputs its current val
                if cur_state3 != prev_state3:     #if current state isn't previous state
                    if not cur_state3:
                        u2 = random.randint(1,10)        #random number
                        if u2 == 1:
                            lcd.print("Your mom looks    like a pig")
                        if u2 == 2:
                            lcd.print("Your parents don'tlove u")
                        if u2 == 3:
                            lcd.print("You're maidenless")
                        if u2 == 4:
                            lcd.print("scanning...braincell count: 0")
                        if u2 == 5:
                            lcd.print("u humans really suck at this")       
                        if u2 == 6:
                            lcd.print("You're a walking disaster")
                        if u2 == 7:
                            lcd.print("ur as smart as a mcchicken")
                        if u2 == 8:
                            lcd.print("You never stood a chance")
                        if u2 == 9:
                            lcd.print("I saw all 14,000,605 outcomes")
                        if u2 == 10:
                            lcd.print("ur a lower life form after all")
                        time.sleep(6)
                    else:
                        lcd.clear()
                cur_state1 = btn1.value       #btn1 outputs its current val
                if cur_state1 != prev_state1:     #if current state isn't previous state
                    if not cur_state1:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(6)
                        lcd.clear()
                else:
                    lcd.clear()
                cur_state2 = btn2.value       #btn2 outputs its current val
                if cur_state2 != prev_state2:     #if current state isn't previous state
                    if not cur_state2:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(6)
                        lcd.clear()
                else:
                    lcd.clear()
                time.sleep(3.0)     #turn servo for 1 second then stop for 5 seconds
                servo_1.throttle = -0.115
                time.sleep(1.5)
                servo_1.throttle = 0        #turn the servo back to start position
            if r1 == 3:
                print("PAPER")
                servo_1.throttle = 0
                servo_2.throttle = 0        #servos at rest
                time.sleep(1.0)
                cur_state1 = btn1.value       #btn1 outputs its current val
                if cur_state1 != prev_state1:     #if current state isn't previous state
                    if not cur_state1:
                        u3 = random.randint(1,10)
                        if u3 == 1:
                            lcd.print("ur mom looks    like a pig")
                        if u3 == 2:
                            lcd.print("ur parents don'tlove u")
                        if u3 == 3:
                            lcd.print("ur maidenless")
                        if u3 == 4:
                            lcd.print("scanning...braincell count: 0")
                        if u3 == 5:
                            lcd.print("u humans really suck at this")     
                        if u3 == 6:
                            lcd.print("You're a walking disaster")
                        if u3 == 7:
                            lcd.print("ur as smart as a mcchicken")
                        if u3 == 8:
                            lcd.print("You never stood a chance")
                        if u3 == 9:
                            lcd.print("I saw all 14,000,605 outcomes")
                        if u3 == 10:
                            lcd.print("ur a lower life form after all")
                        time.sleep(6)
                    else:
                        lcd.clear()
                cur_state2 = btn2.value       #btn2 outputs its current val
                if cur_state2 != prev_state2:     #if current state isn't previous state
                    if not cur_state2:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(6)
                        lcd.clear()
                else:
                    lcd.clear()
                cur_state3 = btn3.value       #btn3 outputs its current val
                if cur_state3 != prev_state3:     #if current state isn't previous state
                    if not cur_state3:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(6)
                        lcd.clear()
                else:
                    lcd.clear()
        else:
            print("btn0 is up")      #if button isn't pressed
            led.value = False       #led off
            servo_1.throttle = 0
            servo_2.throttle = 0        #servos at rest

    prev_state0 = cur_state0        #make the utton sticky
    time.sleep(0.1)     #debounce
    
```

## Obastacles/Errors

## Tips

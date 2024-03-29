# RockPaperScissorsRobot

## [Planning Document](https://docs.google.com/document/d/1lytFuNmM_0qm5QYV4Yy-9biDfxlLaukL8A0iCGQVoEs/edit?usp=sharing)

## [Onshape Document](https://cvilleschools.onshape.com/documents/1c89e5fa0ad5e4b1d4592e61/w/5c6db6f0fa904006adf49133/e/296a1dd83830169753903b0e?renderMode=0&uiState=63e6515d5811cb0608bd567d)

## Overview

Three button inputs for either rock paper or scissors from the human. A countdown LED counts down after you press the start button. press down your (human) input on the thrid flash of the LED. The lcd will display the computers input (rock paper or scissors) if you win or the game is a tie the lcd says "Good Game." If you lose the game the lcd picks from 10 random insults to display on the lcd.

## CAD Renderings

![frank](https://github.com/Cooper-Moreland/RockPaperScissorsRobot/blob/main/Screenshot%202023-02-10%20091912.png?raw=true)

![frank](https://github.com/Cooper-Moreland/RockPaperScissorsRobot/blob/main/Screenshot%202023-02-10%20091846.png?raw=true)

## Images

None, the project is incomplete.

## Materials Used

- Acrylic
- Panel Mount Pushbutton 3x
- Panel Mount LED 2x
- Panel Mount Switch
- LCD Backpack and Screen
- Metro M4 Express
- 6xAA Battery Pack w/ Batteries

## Wiring Diagram

![frank](https://github.com/Cooper-Moreland/RockPaperScissorsRobot/blob/main/Screenshot%202023-02-09%20094446.png?raw=true)

R.P.S. Hand Wiring if we had done the original design (w/ servos), The wiring shown for the pushbuttons is that of a panel mount pushbutton which is what I used in the actual wiring.

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

Code for the original design w/ the servos and hand

```python
import board
from digitalio import DigitalInOut, Direction, Pull
import digitalio
import time
import pwmio    
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
btn1 = DigitalInOut(board.D2)       #ROCK input
btn1.direction = Direction.INPUT
btn1.pull = Pull.UP
btn2 = DigitalInOut(board.D4)       #SCISSORS input
btn2.direction = Direction.INPUT
btn2.pull = Pull.UP
btn3 = DigitalInOut(board.D5)       #PAPER input
btn3.direction = Direction.INPUT
btn3.pull = Pull.UP                             #all 4 buttons as inputs

lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)      #use 0x3f if not working at 0x27
prev_state1 = btn1.value        #player ROCK input
prev_state2 = btn2.value        #player SCISSORS input
prev_state3 = btn3.value        #player PAPER input
lcd.backlight = True
prev_state0 = btn0.value
 
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
                lcd.print("ROCK")
                time.sleep(1)
                lcd.clear()
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
                        time.sleep(4)
                    else:
                        lcd.clear()
                cur_state1 = btn1.value       #btn1 outputs its current val
                if cur_state1 != prev_state1:     #if current state isn't previous state
                    if not cur_state1:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(4)
                        lcd.clear()
                else:
                    lcd.clear()
                cur_state3 = btn3.value       #btn3 outputs its current val
                if cur_state3 != prev_state3:     #if current state isn't previous state
                    if not cur_state3:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(4)
                        lcd.clear()
                else:
                    lcd.clear()
            if r1 == 2:
                lcd.print("SCISSORS")
                time.sleep(1)
                lcd.clear()
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
                        time.sleep(4)
                    else:
                        lcd.clear()
                cur_state1 = btn1.value       #btn1 outputs its current val
                if cur_state1 != prev_state1:     #if current state isn't previous state
                    if not cur_state1:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(4)
                        lcd.clear()
                else:
                    lcd.clear()
                cur_state2 = btn2.value       #btn2 outputs its current val
                if cur_state2 != prev_state2:     #if current state isn't previous state
                    if not cur_state2:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(4)
                        lcd.clear()
                else:
                    lcd.clear()
            if r1 == 3:
                lcd.print("PAPER")
                time.sleep(1)
                lcd.clear()
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
                        time.sleep(4)
                    else:
                        lcd.clear()
                cur_state2 = btn2.value       #btn2 outputs its current val
                if cur_state2 != prev_state2:     #if current state isn't previous state
                    if not cur_state2:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(4)
                        lcd.clear()
                else:
                    lcd.clear()
                cur_state3 = btn3.value       #btn3 outputs its current val
                if cur_state3 != prev_state3:     #if current state isn't previous state
                    if not cur_state3:       #if button pressed
                        lcd.print("Good Game")
                        time.sleep(4)
                        lcd.clear()
                else:
                    lcd.clear()
        else:
            print("btn0 is up")      #if button isn't pressed
            led.value = False       #led off

    prev_state0 = cur_state0        #make the button sticky
    time.sleep(0.1)     #debounce
    
```

Code for later design w/ just buttons and lcd

## Obastacles/Errors

We were unable to 3d print a hand for showing the rock paper scissors gestures, so we had to have the lcd screen display the computer input. The lack of a hand meant no servos were needed anymore which simplified the design for the project. Poor planning was the main cause for out inability o=to complete the project. If we were to do this roject again we would split up the work evenly so we could finish the robot hand in time and have a completed project.

## Tips

Start with the hardest part of the project first to make sure you can compete the whole thing in the end since you have more time to compete the more complicated parts. If metro m4 isn't opening its serial monitor in python first press the reset button then plug it into a different port. If CIRCUITPY (D:) still doesn't show up in file explorer, unplug the power portion of the lcd, upload the code/plug in the board, then rewire the powere to 3.3V or 5V depending on which works.

# Gyroscopic Marble Maze
This deceptively simple yet perfect game combines my love for robotics with a classic game, mazes. Using an accelerometer on the Circuit Playground Express board to control the X and Y directions of motion of a maze, the user can guide a marble through the maze by simply tilting the CPX. What's neat is that this project is constructed using only cardboard, hot glue, and a couple of screws.

![Headstone Image](MananMaze.jpg)


| **Engineer** | **School** | **Area of Interest** | **Grade** | **Date** |
|:--:|:--:|:--:|:--:|:--:|
| Manan G | The Harker School | Mechanical Engineering | Incoming Sophomore | July 2024

  
Final Milestone


**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="600" height="337.5" src="https://www.youtube.com/embed/H_UHTcFpMa8?si=cbOVfYRwUvffjqpX" title="YouTube video player" frameborder="4" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE


# Second Milestone

<iframe width="600" height="337.5" src="https://www.youtube.com/embed/n2mZ4HnMIKA" title="Manan G. Second Milestone" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 


# First Milestone

<iframe width="600" height="337.5" src="https://www.youtube.com/embed/1GwePFE109M?si=EvfzNja3QYFJpNB3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your first milestone, describe what your project is and how you plan to build it. You can include:
- An explanation about the different components of your project and how they will all integrate together
- Technical progress you've made so far
- Challenges you're facing and solving in your future milestones
- What your plan is to complete your project

# Schematics 
![Headstone Image](fritzing.jpg)
*Figure 1: CPX wired to servos 1 and 2*

# Code
One of things that I'm sure I will take away from BlueStamp is how I learnt Object Oriented programming. To understand this code, I had Gemini AI teach me the basics of OOP. I then applied this in my code. This OOP code is the heart and soul of the project: it communicates with the CPX, which tells how and when the servo-motors should move. Different from standard user input-output programs, this code must take in input the "tilting" of the CPX; detect the direction of tilt; pick the correct motor (x- or y-direction motor); tell the servo to move in the appropriate angle.


```python
# Created by Manan G, BSE
# Inspired by Adafruit Industries, 2019. MIT License
# BSE, Jul 2024

import time, board, pwmio, simpleio
from adafruit_motor import servo
from adafruit_circuitplayground import cp
from adafruit_circuitplayground.express import cpx

'######################### HELPER #########################'

def map_range_with_sensitivity(value, min_in, max_in, min_out, max_out, sensitivity):
    return simpleio.map_range(value * sensitivity, min_in, max_in, min_out, max_out)

def avg(lst):
    return (sum(lst)) / (len(lst))
  
def updt_servo_angle(value, servo, readings):
    # Apply sensitivity based on difficulty
    adjusted_value = map_range_with_sensitivity(value, -9.8, 9.8, 0, 180, current_difficulty)

    # Update readings list and calc average
    readings = readings[1:]
    readings.append(adjusted_value)
    adjusted_value = avg(readings)
    print(adjusted_value)
    if servo == my_servo1:
        servo.angle = 180 - adjusted_value
    else:
        servo.angle = adjusted_value
    return readings

def updt_led_from_tilt(x, y):
    # MOD! LED color control based on |tilt| (red for roll, green for pitch)
    scaled_red = abs(x) * 2.55
    scaled_green = abs(y) * 2.55
    cp.pixels.fill((scaled_red, scaled_green, 0))  # Light up CPX RGB display

### Servo and LED Setup
# Create PWM objects in pin A1 and A2
pwm1 = pwmio.PWMOut(board.A1, duty_cycle=2 ** 15, frequency=50)
pwm2 = pwmio.PWMOut(board.A2, duty_cycle=2 ** 15, frequency=50)

# Create object 'servo'
my_servo1 = servo.Servo(pwm1)
my_servo2 = servo.Servo(pwm2)

'######################### MAIN PROGRAM #########################'

### Reset servos to 0 degrees every time the program starts
print(0)
my_servo1.angle = 0
my_servo2.angle = 0

### Initialize mechanism to read tilt angles
NUM_READINGS = 9
roll_readings = [90] * NUM_READINGS
pitch_readings = [90] * NUM_READINGS

### MOD! Define difficulty levels with different sensitivity multipliers
DIFFICULTY_LOW = 1.0
DIFFICULTY_MEDIUM = 1.5
DIFFICULTY_HIGH = 2.0
current_difficulty = DIFFICULTY_HIGH

'''
Until it is unplugged, the CPX must read in acceleration from the CPX,
update the positions of the two servos, updated the tilt readings of each servo, 
and convert the tilt to a rgb display color code composed of more red hure for x- 
and more green hue for y-direction tilt.
'''
while True:
    x, y, z = cpx.acceleration
    roll_readings = updt_servo_angle(x, my_servo1, roll_readings)
    pitch_readings = updt_servo_angle(y, my_servo2, pitch_readings)
    updt_led_from_tilt(x, y)
    time.sleep(0.02)
```


# Bill of Materials
One of the more affordable projects, the Marble Maze requires very few technical components: these being the CircuitPython Express Board and the two Servos. Most of the other items are generic and can probably be found in an Office Depot/some Toolshop. This includes a Hot Glue Gun, Wires, etc. For cardboard, I used scrap cardboard from Amazon packages, and they worked perfectly!

| **Part** | **Notes** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Circuit Playground Express Board | Controller, what you tilt | $24.95 | <a href="https://www.adafruit.com/product/169"> Purchase here! </a> |
| Utech Smart USB-C Hub | USB-C to USB-A adapter | $29.99 | <a href="https://www.amazon.com/UtechSmart-Ethernet-Delivery-Compatible-Chromebook/dp/B07H2ZS1B5"> Any works, recommended here </a> |
| USB-A to Micro-B cable | USB-A to Micro-B adapter | $2.95 | <a href="https://www.adafruit.com/product/592"> Purchase here! </a> |
| Small Alligator Clip to Male Jumper Wire Bundle - 6 Pieces | Connecting CPX to Servos  | $3.95 | <a href="https://www.adafruit.com/product/3448"> Recommended here </a> |
| Micro servo - TowerPro SG92R - 2 pieces | Servos controlling maze tilt  | $11.90 | <a href="https://www.adafruit.com/product/169"> Purchase here! </a> |
| Hot Glue Gun | Any gun works; used to glue maze walls/parts together  | $9.99 | <a href="https://www.amazon.com/Krightlink-Sticks-School-Crafts-Repairs/dp/B0BC878ZRG/ref=sr_1_4_sspa?dib=eyJ2IjoiMSJ9.4XQ5ZC91OMSN2e0ykixncWkpGm2CphQhln5n11prYd3XvQzbUDwoeTvtkyMVCOB18pH_NSvfWIwJsSiW6ognqtuvHSmSt8SIliIwakCwBLdrDD5ZO1TNb5saKvh0jEodvAU8oYZOqVif4n93Mjvvsg-vCcaWSyRJCVgWJBhtljkUw-zP0sQpEah__iEzjbRKLfJWeXAmsvA06BxTp8Xz9u__s4t387PldiJe2VCkdZW2PE7qRLsmQGdlslFLAyYSfZlQJie8Bb89jmPUHJf31gOAlpSIMXDjKV4VV3pWofw.AifZQErxvz0Yv-iaGeEqoAo7N6UJM3dCIrKcbmG2h1w&dib_tag=se&hvadid=598724400377&hvdev=c&hvlocphy=9032183&hvnetw=g&hvqmt=b&hvrand=6841085219243952434&hvtargid=kwd-322848738459&hydadcr=4866_13229483&keywords=hot-glue%2Bgun&qid=1720042211&sr=8-4-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Any works, recommended here </a> |
| Carboard | Build the maze and frame  | $0+ | Scrap works! |

# Starter Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ic72ahxqY6g?si=ug5FnIE2N02ikeoR" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


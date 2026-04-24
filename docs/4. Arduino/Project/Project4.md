### Project 4 Servo Control

![](media/image-20250908152354194.png)

 **Description**

Servo motor is a position control rotary actuator. It mainly consists of housing, a circuit board, a core-less motor, a gear and a position sensor. Its working principle is that the servo receives the signal sent by MCUs or receivers and produces a reference signal with a period of 20ms and width of 1.5ms, then compares the acquired DC bias voltage to the voltage of the potentiometer and obtain the voltage difference output.

When the motor speed is constant, the potentiometer is driven to rotate through the cascade reduction gear, which leads that the voltage difference is 0, and the motor stops rotating. Generally, the angle range of servo rotation is 0°\--180 °.

The rotation angle of servo motor is controlled by regulating the duty cycle of PWM (Pulse-Width Modulation) signal. The standard cycle of PWM signal is 20ms (50Hz). Theoretically, the width is distributed between 1ms-2ms, but in fact, it's between 0.5ms-2.5ms. The width corresponds the rotation angle from 0° to 180°. But note that for motors of different brands , the same signal may yield different rotation angles.

![](media/image-20250908152510007.png)

In general, servo has three lines in brown, red and orange. The brown wire is grounded, the red one is positive pole line and the orange one is signal line.

![](media/image-20250908152525491.png)

The corresponding servo angles are shown below:

![](media/image-20250908152558682.png)

 **Specification**

- Working voltage: DC 4.8V \~ 6V
- Operating angle range: about 180 ° (at 500 → 2500 μsec)
- Pulse width range: 500 → 2500 μsec
- No-load speed: 0.12 ± 0.01 sec / 60 (DC 4.8V) 0.1 ± 0.01 sec / 60 (DC 6V)
- No-load current: 200 ± 20mA (DC 4.8V) 220 ± 20mA (DC 6V)
- Stopping torque: 1.3 ± 0.01kg · cm (DC 4.8V) 1.5 ± 0.1kg · cm (DC 6V)
- Stop current: ≦ 850mA (DC 4.8V) ≦ 1000mA (DC 6V)
- Standby current: 3 ± 1mA (DC 4.8V) 4 ± 1mA (DC 6V)

**Components**

![](media/image-20250908152809268.png)

  **Connection Diagram：**

![](media/image-20250908152824930.png)**Wiring notes:**  the brown line of servo is linked with Gnd(G), the red line is connected to 5v(V) and orange line is attached to digit 9.

The servo has to be connected to external power due to its high demand for driving servo current. Generally, the current of development board is not enough. If without connected power, the development board could be burnt.

**Test Code 1**

```
/*
keyestudio Mini Tank Robot V2
lesson 4.1
Servo
http://www.keyestudio.com
*/
#define servoPin 9  //servo Pin
int pos; //angle variable of servo
int pulsewidth; // pulse width variable of servo

void setup() 
{
  pinMode(servoPin, OUTPUT);  //set servo pin to OUTPUT
  procedure(0); //set the angle of servo to 0°
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1) // goes from 0 degrees to 180 degrees
  { 
    // in steps of 1 degree
    procedure(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                   //control the rotation speed of servo
  }
  for (pos = 180; pos >= 0; pos -= 1) // goes from 180 degrees to 0 degrees
  { 
    procedure(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                    
  }
}
// function to control servo
void procedure(int myangle) 
{
  pulsewidth = myangle * 11 + 500;  //calculate the value of pulse width
  digitalWrite(servoPin,HIGH);
  delayMicroseconds(pulsewidth);   //The duration of high level is pulse width
  digitalWrite(servoPin,LOW);
  delay((20 - pulsewidth / 1000));  // the cycle is 20ms, the low level last for the rest of time
}
```

Upload code successfully, servo swings back in the range of 0° to 180°.

```
/*
 keyestudio Mini Tank Robot V2
 lesson 4.2
 servo
 http://www.keyestudio.com
*/
#include <Servo.h>
Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards
int pos = 0;    // variable to store the servo position

void setup() 
{
  myservo.attach(9);  // attaches the servo on pin 9 to the servo object
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1)  // goes from 0 degrees to 180 degrees
  {
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
  for (pos = 180; pos >= 0; pos -= 1) { // goes from 180 degrees to 0 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
}
```

**Test Result**

Upload code successfully and power on, servo swings in the range of 0° to 180°.

The result is the same. We usually control it by library file.

**Code Explanation**

Arduino comes with **\#include \<Servo.h\>** (servo function and statement）

The following are some common statements of the servo function:

1. attach（interface）——Set servo interface, port 9 and 10 are available.
2. write（angle）——The statement to set rotation angle of servo, the angle.
3. read（）——The statement to read angle of servo, read the command value of“write()”.
4. <span style="color: rgb(255, 76, 65);">Note:</span> The above written format is“servo variable name, specific statement（）”, for instance: myservo.attach(9).

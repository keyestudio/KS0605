## 2. Product installation

After checking all the parts in this kit, we need to mount the tank robot. Let’s install the smart car in compliance with the following instructions.

### Assembly Video

<iframe width="560" height="315" src="https://www.youtube.com/embed/ES0wFdEsX4M" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" style="box-sizing: border-box; color: rgb(64, 64, 64); font-family: Lato, proxima-nova, &quot;Helvetica Neue&quot;, Arial, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(252, 252, 252); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

**Note: Peel the plastic film off the board first when installing smart car.**

### Step 1: Install Bottom Motor

Prepare the parts as follows:

- M4 Nut \* 2
- Metal Motor \*2
- Metal Holder \*2
- Copper Coupler \*2
- Blue Supportive Parts \*2
- M4\*12MM Inner Hex Screw \* 2
- M1.5 Hex Key Nickel Plated Allen Wrench \*1
- M3 Hex Key Nickel Plated Allen Wrench \*1
- M2.5 Hex Key Nickel Plated Allen Wrench \*1
- M3\*8MM Inner Hex Screw \* 4

![TK_02](media/TK_02.png)

![TK_03](media/TK_03.png)

==Prompt: assemble another side motor in the same way.==

### Step 2: Install Driver Wheel

Prepare the parts as follows:

- M4\*12MM Inner Hex Screw \* 2
- M4\*50MM Inner Hex Screw \* 2
- Tank Load-bearing Wheel \* 2
- Flange Bearing \* 4
- Copper Bush \*2
- Caterpillar Band \*2
- M4 Self-locking Nut \* 2
- M3 Hex Key Nickel Plated Allen Wrench \*1

![TK_04](media/TK_04.png)

![TK_05](media/TK_05.png)

![TK_06](media/TK_06.png)

![TK_07](media/TK_07.png)

### Step 3: Install the Battery Holder

Prepare the parts as follows:

- Battery Holder \*1
- M3 Nut \* 2
- Blue Metal holder \*2
- M4 Nut \*8
- M3\*10MM Flat Head Screw \* 2
- M4\*40MM Inner Hex Screw \*4
- M2.5 Hex Key Nickel Plated Allen Wrench\*1
- M3 Hex Key Nickel Plated Allen Wrench \*1
- M3\*25MM Inner Hex Screw \*4

![TK_08](media/TK_08.png)

![TK_09](media/TK_09.png)

Move to fix the metal holder on the motor wheel with four M4\*40MM inner hex screws and four M4 nuts when the mounting process is completed.

![TK_10](media/TK_10.png)

![TK_11](media/TK_11.png)

![TK_12](media/TK_12.png)

![TK_13](media/TK_13.png)

###  Step 4: Mount Acrylic Board and Sensors

- Acrylic Board \* 2
- L- type Black Bracket \*1
- Photocell Sensor \*2
- IR Receiver Module \*1
- 8X16 LED Panel \*1
- M2 Nut \*4
- M3 Nut \*10
- M3\*6MM Inner Hex Screw \* 8
- M2.5 Hex Key Allen Wrench \*1
- M3\*12MM Round Head Screw \*7
- M3\*10MM Hexagon Copper Bush \*8
- M2\*10MM Round Head Screw \* 4

![TK_14](media/TK_14.png)

![TK_15](media/TK_15.png)

![TK_16](media/TK_16.png)

![TK_17](media/TK_17.png)

![TK_18](media/TK_18.png)

![TK_19](media/TK_19.png)

![TK_20](media/TK_20.png)

![TK_21](media/TK_21.png)

![TK_22](media/TK_22.png)

![TK_23](media/TK_23.png)

### Step 5: Install the Servo Platform

Prepare the parts as follows:

-   Servo \*1
-   Black Gimbal \*1
-   Cable Tie \*2
-   M2x8 Round Head Cross Tapping Screw \*2
-   Ultrasonic Sensor \*1
-   M2\*4 Screw \*1
-   M1.2\*5 Screw \*4

<span style="color: rgb(255, 76, 65);">Note: </span>for convenient debugging, keep the ultrasonic module straight ahead and the angle of servo motor at 90°. Therefore, we need to set the servo to 90° before installing the servo platform.

Set the 90-degree code,Copy the code and upload it to the development board. The steering gear connected to port D9 will rotate to 90 °.

<font color= #8B0000>To upload code, you will need the Arduino IDE. Please first install the Arduino IDE by following sections 4.2–4.4. (Software Download, Set Up Arduino IDE, and Add Library)</font>

```c
#define servoPin 9 //servo Pin
int pos; //the angle variable of servo
int pulsewidth; // pulse width variable of servo

void setup() 
{
    pinMode(servoPin, OUTPUT); //set servo pin to OUTPUT
    procedure(0); //set the angle of servo to 0°
}

void loop() 
{
	procedure(90); // tell servo to go to position in variable 90°
}

// function to control servo
void procedure(int myangle) 
{
    pulsewidth = myangle * 11 + 500; //calculate the value of pulse width
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth); //The duration of high level is pulse width
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000)); // the cycle is 20ms, the low level last for the rest of time
}
```

![TK_24](media/TK_24.png)

![](media/image-20250902144145590.png)

<span style="color: rgb(255, 76, 65);">Note: </span>You can find M1.2\*5 Screws inside the bag of Plastic Platform.

![TK_25](media/TK_25.png)

![TK_26](media/TK_26.png)

### Step 6: Install Sensors and Boards

Prepare the parts as follows:

- M3\*6MM Round Head Screw \*12
- L298P Shield \*1
- V4.0 Board \*1
- V5 Sensor Shield \*1
- Screwdriver \*1
- Bluetooth Module \*1

![TK_27](media/TK_27.png)

![TK_28](media/TK_28.png)

![TK_29](media/TK_29.png)

![TK_30](media/TK_30.png)

![TK_31](media/TK_31.png)

![TK_32](media/TK_32.png)

![TK_33](media/TK_33.png)

![TK_34](media/TK_34.png)



### Step 7: Hook-up Guide

![](media/image-20250902144534790.png)

![](media/image-20250902144551034.png)

![](media/image-20250902144559983.png)

![](media/image-20250902144849310.png)

![](media/image-20250902144902221.png)

###  Step 8: Wire Up LED Panel

![](media/image-20250902145026905.png)

![](media/image-20250902145112884.png)

![](media/image-20250902145129382.png)

| LED Panel                              | V5 Sensor Shield                       |
| -------------------------------------- | -------------------------------------- |
| GND                                    | -(GND)                                 |
| VCC                                    | +(VCC)                                 |
| SDA                                    | SDA                                    |
| SCL                                    | SCL                                    |
| ![](media/image-20250902145404151.png) | ![](media/image-20250902145414755.png) |

### Step 9: Install all parts of Acrylic plate

![](media/image-20250902145506652.png)

![](media/image-20250902145615504.png)

![](media/image-20250902145822634.png)

![](media/image-20250902145854886.png)

![](media/image-20250902145934002.png)

![](media/image-20250902150004173.png)

![](media/image-20250902150032438.png)

![](media/image-20250902150052468.png)

![](media/image-20250902150217564.png)

![](media/image-20250902150508905.png)

![](media/image-20250902150522753.png)

![](media/image-20250902150532987.png)

![](media/image-20250902150711706.png)

###  Step 10: Tank Robot

<span style="color: rgb(255, 76, 65);">Note:</span> Remove the Bluetooth module before uploading test code. Otherwise, you will fail to upload test code.

![](media/image-20250902151034545.png)

**Multi-purpose Robot Car**

![](media/image-20250902151133169.png)

  **Description**

In the previous projects, the tank car only performs a single function. However, in this lesson, we integrate all of its functions to control smart car via Bluetooth control.

Here is a simple flow chart of multi-purpose robot car for your reference.

![](media/image-20250902151215210.png)

  **Connection Diagram**

![](media/image-20250902151230702.png)

<span style="color: rgb(255, 76, 65);">Attention：</span>Confirm that every component is connected.

Wire-up Guide:

| 8x16 LED panel |      | Expansion Board |
| -------------- | ---- | --------------- |
| GND            | →    | -（GND）        |
| VCC            | →    | +（VCC）        |
| SDA            | →    | SDA             |
| SCL            | →    | SCL             |

![](media/image-20250902152539713.png)

| Ultrasonic Module |      |        |
| ----------------- | ---- | ------ |
| VCC               | →    | 5v(V)  |
| Trig              | →    | 5(S)   |
| Echo              | →    | 4(S)   |
| Gnd               | →    | Gnd(G) |

![](media/image-20250902152857086.png)

![](media/image-20250902152906103.png)

| Servo Motor |      |        |
| ----------- | ---- | ------ |
| Servo Motor | →    | Gnd(G) |
| Red Wire    | →    | 5v(V)  |
| Orange Wire | →    | 9      |

![](media/image-20250902154418006.png)

![](media/image-20250902154820948.png)

| Bluetooth Module                        |      |          |
| --------------------------------------- | ---- | -------- |
| RXD                                     | →    | TX       |
| TXD                                     | →    | RX       |
| GND                                     | →    | -（GND） |
| VCC                                     |      | +（VCC） |
| No need to attach to STATE and BRK pins |      |          |

![](media/image-20250902155229663.png)

![](media/image-20250902155236836.png)

| IR Receiver Module |      | Sensor Shield |
| ------------------ | ---- | ------------- |
| －                 | →    | G（GND）      |
| +                  | →    | V（VCC）      |
| S                  | →    | A0            |

![](media/image-20250902155444270.png)

![](media/image-20250902155452133.png)

| Left photo resistor  |      | Sensor Shield |
| -------------------- | ---- | ------------- |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A1            |
|                      |      |               |
| Right Photo resistor |      | Sensor Shield |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A2            |

![](media/image-20250902155938106.png)

![](media/image-20250902155946213.png)

 Installation complete.


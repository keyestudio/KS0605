# Project 13 IR Remote Robot Tank

![](media/image-20250908172649810.png)

**Beschrijving**

Infrarood afstandsbediening is een van de meest gebruikte besturingsmethoden, toegepast in televisies, elektrische ventilatoren en enkele huishoudelijke apparaten. In dit project maken we een IR-gestuurde slimme auto. Omdat we elke toetswaarde op de IR-afstandsbediening kennen, kunnen we de slimme auto via de afstandsbediening besturen en patronen op de dot matrix weergeven via de overeenkomstige toetswaarde.

**De specifieke logica van de infrarood afstandsbediening robotauto wordt hieronder weergegeven:**

| Initiële instellingen                  | Servo hoek 90°                          |                                     |
| -------------------------------------- | --------------------------------------- | ----------------------------------- |
|                                        | 8X16 LED matrix paneel toont pictogram "V" |                                     |
| **Afstandsbediening**                  | **Toetswaarde**                         | **Toestatus**                       |
| ![](media/image-20250908172904905.png) | FF629D                                  | Vooruit gaan (PWM ingesteld op 200)  |
|                                        |                                         | 8X16 LED paneel toont vooruit pictogram |
| ![](media/image-20250908172927504.png) | FFA857                                  | Achteruit gaan (PWM ingesteld op 200) |
|                                        |                                         | 8X16 LED paneel toont achteruit pictogram |
| ![](media/image-20250908172954542.png) | FF22DD                                  | Links draaien                       |
|                                        |                                         | 8X16 LED paneel toont links pictogram |
| ![](media/image-20250908173027144.png) | FFC23D                                  | Rechts draaien                      |
|                                        |                                         | 8X16 LED paneel toont rechts pictogram |
| ![](media/image-20250908173139888.png) | FF02FD                                  | Stoppen                             |
|                                        |                                         | 8X16 LED paneel toont "STOP"        |
| ![](media/image-20250908173312378.png) | FF30CF                                  | Linksom roteren (PWM ingesteld op 200) |
|                                        |                                         | 8X16 LED paneel toont linksom pictogram |
| ![](media/image-20250908173336232.png) | FF7A85                                  | Rechtsom roteren (PWM ingesteld op 200) |
|                                        |                                         | 8X16 LED paneel toont rechtsom pictogram |

**Stroomdiagram**

![](media/image-20250908173443316.png)

**Verbindingsschema**

![](media/image-20250908173458023.png)

Opmerking: GND, VCC, SDA, SCL van het 8x16 LED paneel zijn respectievelijk verbonden met - (GND), + (VCC), SDA, SCL. En "-", "+" en S van de IR-ontvanger module zijn aangesloten op G (GND), V (VCC) en A0 op de sensorshield. Bij onvoldoende digitale poorten kunnen de analoge poorten als digitale poorten worden gebruikt. A0 is gelijk aan digitaal 14, A1 is gelijk aan digitaal 15.

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 13
 IR afstandsbediening tank
 http://www.keyestudio.com
*/

#include <IRremoteTank.h>
IRrecv irrecv(A0);  // stel IRrecv irrecv in op A0
decode_results results;
long ir_rec;  // sla de ontvangen IR-waarde op

// Array, gebruikt om de gegevens van het patroon op te slaan, kan zelf worden berekend of verkregen via het modulus-hulpmiddel
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Stel klokpin in op A5
#define SDA_Pin  A4  // Stel gegevenspin in op A4

#define ML_Ctrl 13  // definieer de richtingscontrolpin van de linkermotor
#define ML_PWM 11   // definieer PWM-controlpin van de linkermotor
#define MR_Ctrl 12  // definieer de richtingscontrolpin van de rechtermotor
#define MR_PWM 3    // definieer PWM-controlpin van de rechtermotor

#define servoPin 9 // pin van servo
int pulsewidth; // sla de pulsbreedte-waarde van servo op

void setup(){
  Serial.begin(9600);
  irrecv.enableIRIn();  // Initialiseer de IR-ontvangstbibliotheek
  
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); // Scherm wissen
  matrix_display(start01);  // startafbeelding weergeven
  
  pinMode(servoPin, OUTPUT);
  procedure(90);  // Servo roteert naar 90°
}

void loop(){
  if (irrecv.decode(&results)) // ontvang de IR-afstandsbediening waarde
  {
    ir_rec=results.value;
    String type="UNKNOWN";
    String typelist[14]={"UNKNOWN", "NEC", "SONY", "RC5", "RC6", "DISH", "SHARP", "PANASONIC", "JVC", "SANYO", "MITSUBISHI", "SAMSUNG", "LG", "WHYNTER"};
    if(results.decode_type>=1&&results.decode_type<=13){
      type=typelist[results.decode_type];
    }
    Serial.print("IR TYPE:"+type+"  ");
    Serial.println(ir_rec,HEX);
    irrecv.resume();
  }
  
  if (ir_rec == 0xFF629D) // Vooruit gaan
  {
    Car_front();
    matrix_display(front);  // Vooruit afbeelding weergeven
  }
  if (ir_rec == 0xFFA857)  // Robotauto gaat achteruit
  {
    Car_back();
    matrix_display(front);  // Achteruit gaan
  }
  if (ir_rec == 0xFF22DD)   // Robotauto draait links
  {
    Car_T_left();
    matrix_display(left);  // Linksdraai afbeelding weergeven
  }
  if (ir_rec == 0xFFC23D)   // Robotauto draait rechts
  {
    Car_T_right();
    matrix_display(right);  // Rechtsdraai afbeelding weergeven
  }
  if (ir_rec == 0xFF02FD)   // Robotauto stopt
  { 
    Car_Stop();
    matrix_display(STOP01);  // stopafbeelding weergeven
  }
  if (ir_rec == 0xFF30CF)   // robotauto roteert tegen de klok in
  {
    Car_left();
    matrix_display(left);  // rotatie tegen de klok in afbeelding weergeven
  }
  if (ir_rec == 0xFF7A85)  // robotauto roteert met de klok mee
  {
    Car_right();
    matrix_display(right);  // rotatie met de klok mee afbeelding weergeven
 }
}
/******************Servo besturen*******************/
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}

/******************Dot Matrix****************/
// deze functie wordt gebruikt voor dot matrix weergave
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  // Kies adres
   for(int i = 0;i < 16;i++) // De afbeelding heeft 16 bits
  {
     IIC_send(matrix_value[i]); // gegevens om patronen over te dragen
  }
  IIC_end();   // beëindig het overdragen van gegevenspatroon
  
  IIC_start();
  IIC_send(0x8A);  // weergavebesturing, stel pulsbreedte in op 4/16
  IIC_end();
}

// De voorwaarde om gegevens over te dragen
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Elke byte heeft 8 bits, 8 bits voor elk teken
  {
      digitalWrite(SCL_Pin,LOW);  // trek klokpin SCL_Pin omlaag om de signalen van SDA te veranderen
      delayMicroseconds(3);
      if(send_data & 0x01)  // stel hoog en laag niveau van SDA_Pin in volgens 1 of 0 van elke bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // trek klokpin SCL_Pin omhoog om gegevensoverdracht te stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  // detecteer bit voor bit, dus verschuif de gegevens naar rechts met één
  }
}
// Het teken dat gegevensoverdracht eindigt
void IIC_end()
{
  digitalWrite(SCL_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
}
/***************de functie om motor uit te voeren***************/
void Car_front()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_back()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,200);
}
void Car_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,255);
}
void Car_right()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,255);
}
void Car_Stop()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,0);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,0);
}
void Car_T_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,180);
}
void Car_T_right()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,180);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,255);
}
 //****************************************************************
```

**Testresultaat**

Upload de code succesvol en schakel in. De slimme robot kan worden bestuurd via de IR-afstandsbediening. Tegelijkertijd wordt het overeenkomstige patroon weergegeven op het 8X16 LED paneel.
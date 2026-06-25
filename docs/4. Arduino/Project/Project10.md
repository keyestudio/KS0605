# Project 10 Lichtvolgende Robot

![](media/image-20250908171131879.png)

**Beschrijving**

We hebben al uitgelegd hoe je verschillende sensoren en modules gebruikt.

In deze les combineren we hardwarekennis -- fotoweerstandmodule, motorbesturing -- om een lichtvolgende robot te bouwen!

Je hebt slechts 2 fotoweerstandmodules nodig om de lichtintensiteit aan beide zijden van de robot te detecteren. Lees de analoge waarde uit om de 2 motoren te draaien, zodat de tankrobot kan rijden.

**De specifieke logica van de lichtvolgende robot wordt in de onderstaande tabel weergegeven:**

![](media/image-20250908171219561.png)

We maken een stroomdiagram op basis van de bovenstaande logicatabel, zoals hieronder weergegeven:

![](media/image-20250908171232654.png)

**Verbindingsschema**

![](media/image-20250908171305946.png)

**Aandacht:**

Het 4-pins terminalblok is gemarkeerd met zijdescherm 1234. De rode draad van de rechter achtermotор is verbonden met terminal 1, de zwarte draad is verbonden met uiteinde 2. De rode draad van de linker voormotор is bevestigd aan terminal 3, de zwarte draad is verbonden met poort 4.

| Linker fotoweerstand    |      | Sensor Shield     |
| ----------------------- | ---- | ----------------- |
| -                       | →    | G（GND）          |
| +                       | →    | V（VCC）          |
| S                       | →    | A1                |
|                         |      |                   |
| **Rechter fotoweerstand** |      | **Sensor Shield** |
| -                       | →    | G（GND）          |
| +                       | →    | V（VCC）          |
| S                       | →    | A2                |

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 10
 Lichtvolgende tank
 http://www.keyestudio.com
*/ 
#define light_L_Pin A1   //definieer de pin van de linker fotoweerstand
#define light_R_Pin A2   //definieer de pin van de rechter fotoweerstand
#define ML_Ctrl 13  //definieer de richtingscontrolepín van de linker motor
#define ML_PWM 11   //definieer de PWM-controlepín van de linker motor
#define MR_Ctrl 12  //definieer de richtingscontrolepín van de rechter motor
#define MR_PWM 3   //definieer de PWM-controlepín van de rechter motor
int left_light; 
int right_light;
void setup(){
  Serial.begin(9600);
  pinMode(light_L_Pin, INPUT);
  pinMode(light_R_Pin, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  left_light = analogRead(light_L_Pin);
  right_light = analogRead(light_R_Pin);
  Serial.print("left_light_value = ");
  Serial.println(left_light);
  Serial.print("right_light_value = ");
  Serial.println(right_light);
  if (left_light > 650 && right_light > 650) //de waarde gedetecteerd door fotoweerstand, ga vooruit
  {  
    Car_front();
  } 
  else if (left_light > 650 && right_light <= 650)  //de waarde gedetecteerd door fotoweerstand, draai links
  {
    Car_left();
  } 
  else if (left_light <= 650 && right_light > 650) //de waarde gedetecteerd door fotoweerstand, draai rechts
  {
    Car_right();
  } 
  else  //andere situaties, stop
  {
    Car_Stop();
  }
}
void Car_front()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,200);
}
void Car_right()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_Stop()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,0);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,0);
}
//****************************************************************
```

**Testresultaat**

Upload de code op het keyestudio V4.0-ontwikkelingsbord, zet de DIP-schakelaar op de rechterkant en schakel het in. De slimme robot volgt het licht en beweegt.
# Project 12 Ultrasonic Following Tank

![](media/image-20250908172315808.png)

**Beschrijving**

In project 11 hebben we een auto met obstakeldetectie gemaakt. In feite hoeven we alleen maar de testcode aan te passen om een auto met obstakeldetectie om te zetten in een volgende auto. In deze les maken we een ultrasone volgende robot. De ultrasone sensor detecteert de afstand tussen de slimme auto en het obstakel om de tankwagen te laten bewegen.

**De specifieke logica van de ultrasone volgende robot is als volgt:**

| **Detectie** | **Gemeten afstand van frontale obstakels** | **Afstand (eenheid: cm)** |
| ------------- | ---------------------------------------- | ----------------------- |
| Instellingen      | Servo hoek 90°                          |                         |
|               | 8X16 LED-paneel toont het pictogram "V"        |                         |
| Als            | 20≤ afstand ≤60                         |                         |
| Status        | Vooruit gaan（PWM ingesteld op 200）               |                         |
| Als            | 10\<afstand＜20                         |                         |
|               | afstand＞60                             |                         |
| Status        | Stoppen                                     |                         |
| Als            | afstand ≤10                             |                         |
| Status        | Stoppen（PWM ingesteld op 200）                   |                         |

**Stroomdiagram**

![](media/image-20250908172442991.png)

**Verbindingsschema**

![](media/image-20250908172457017.png)

Opmerkingen voor bedrading:

| 1.8x16 LED-paneel |      | V5 Sensor Shield |
| ---------------- | ---- | ---------------- |
| GND              | →    | -（GND）         |
| VCC              | →    | +（VCC）         |
| SDA              | →    | SDA              |
| SCL              | →    | SCL              |

**Testcode**

```c
 /*
 keyestudio Mini Tank Robot V2.1
 les 12
 ultrasone volgende tank
 http://www.keyestudio.com
*/ 
//Array, gebruikt om de gegevens van het patroon op te slaan, kan zelf worden berekend of verkregen via het modulus-gereedschap
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Stel klokpin in op A5
#define SDA_Pin  A4  //Stel gegevenspin in op A4

#define ML_Ctrl 13  //definieer de richtingscontroleppin van de linkermotor
#define ML_PWM 11   //definieer PWM-controleppin van de linkermotor
#define MR_Ctrl 12  //definieer de richtingscontroleppin van de rechtermotor
#define MR_PWM 3   //definieer PWM-controleppin van de rechtermotor
#define Trig 5  //ultrasone trig-pin
#define Echo 4  //ultrasone echo-pin
int distance;
int pulsewidth;
#define servoPin 9  //servo-pin
void setup(){
  Serial.begin(9600);
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); //Wis het display
  matrix_display(start01);  //toon startpatroon
  pinMode(servoPin, OUTPUT);
  procedure(90); //stel servo in op 90°
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  distance = checkdistance();  //wijs de afstand gedetecteerd door ultrasone sensor toe aan distance
  if (distance >= 20 && distance <= 60) //bereik om vooruit te gaan
  {
    Car_front();
  }
  else if (distance > 10 && distance < 20)  //bereik om te stoppen
  {
    Car_Stop();
  }
  else if (distance <= 10)  //bereik om achteruit te gaan
  {
    Car_back();
  }
  else  //andere situaties, stoppen
  {
    Car_Stop();
  }
}
/***********de functie voor motorwerking****************/
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

/******************puntmatrix********************/
// de functie voor puntmatrix-display
void matrix_display(unsigned char matrix_value[])
{
  IIC_start(); // roep de functie aan die gegevenstransmissie start
  IIC_send(0xc0);  //Kies adres
  
  for(int i = 0;i < 16;i++) //patroongegevens hebben 16 bits
  {
     IIC_send(matrix_value[i]); //gegevens om patronen over te dragen
  }

  IIC_end();   //beëindig het overbrengen van gegevenspatroon
  
  IIC_start();
  IIC_send(0x8A);  //selecteer pulsbreedte 4/16, controleer display
  IIC_end();
}

//De voorwaarde om gegevenstransmissie te starten
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

// gegevens overbrengen
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Elke byte heeft 8 bits
  {
      digitalWrite(SCL_Pin,LOW);  //trek klokpin SCL_Pin omlaag om de signalen van SDA te veranderen      
delayMicroseconds(3);
      if(send_data & 0x01)  //stel hoog en laag niveau van SDA_Pin in volgens 1 of 0 van elke bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //trek klokpin SCL_Pin omhoog om gegevenstransmissie te stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  // detecteer bit voor bit, dus verschuif de gegevens naar rechts met één
  }
}
//Het teken dat gegevenstransmissie eindigt
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
/***************einde puntmatrix-display******************/
//De functie om servo te controleren
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }}
//De functie om ultrasone sensorfunctie te controleren die ultrasone controleert
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.20;  //58.20, dat wil zeggen, 2*29.1=58.2
  delay(10);
  return distance;
}
//****************************************************************
```

 **Testresultaat**

Code succesvol geüpload, DIP-schakelaar is naar het rechtereinde verschoven, de servo roteert naar 90°, "V" wordt weergegeven op het 8X16 LED-paneel en de slimme auto beweegt mee met het obstakel.
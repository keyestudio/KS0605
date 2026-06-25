# Project 15: Volledig functioneel eindproject

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 15
 bluetooth tank
 http://www.keyestudio.com
*/

//Array, gebruikt om de gegevens van het patroon op te slaan, kan zelf worden berekend of verkregen via de modulus tool
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Stel klokpin in op A5
#define SDA_Pin  A4  //Stel datapinop A4

#define ML_Ctrl 13  //definieer richtingscontrolepinvan linkse motor
#define ML_PWM 11   //definieer PWM-controlepinvan linkse motor
#define MR_Ctrl 12  //definieer richtingscontrolepinvan rechtse motor
#define MR_PWM 3    //definieer PWM-controlepinvan rechtse motor

char bluetooth_val; //sla de waarde van Bluetooth-ontvangst op

void setup()
{
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    //Wis het display
  matrix_display(start01);  //toon startpatroon

  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}

void loop()
{
  if (Serial.available())
  {
    bluetooth_val = Serial.read();
    Serial.println(bluetooth_val);
  }
  switch (bluetooth_val) 
  {
     case 'F':  //voorwaarts commando
        Car_front();
        matrix_display(front);  //toon voorwaarts ontwerp
        break;
     case 'B':  //achterwaarts commando
        Car_back();
        matrix_display(back);  //toon achterwaarts patroon
        break;
     case 'L':  //linksaf instructie
        Car_left();
        matrix_display(left);  //toon "linksaf" teken
        break;
     case 'R':  //rechtsaf instructie
        Car_right();
        matrix_display(right);  //toon rechtsaf teken
        break;
     case 'S':  //stopcommando
        Car_Stop();
        matrix_display(STOP01);  //toon stopafbeelding
        break;
  }
}

/**************De functie van de puntmatrix****************/
//deze functie wordt gebruikt voor puntmatrixweergave
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //Kies adres
  
  for(int i = 0;i < 16;i++) //patroongegevens hebben 16 bits
  {
     IIC_send(matrix_value[i]); //gegevens om patronen over te dragen
  }
  IIC_end();   //beëindig het overbrengen van patroongegevens
  
  IIC_start();
  IIC_send(0x8A);  //weergavebesturing, stel pulsbreedte in op 4/16
  IIC_end();
}
//De voorwaarde om gegevens over te dragen
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
//gegevens overbrengen
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
      digitalWrite(SCL_Pin,HIGH); //trek klokpin SCL_Pin omhoog om gegevensoverdracht te stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  //Detecteer bit voor bit, dus verschuif de gegevens naar rechts met één
  }
}
//Het teken dat gegevensoverdracht eindigt
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
/*************de functie om motor uit te voeren**************/
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
```

**Testresultaat**

**Opmerking:** Verwijder de Bluetooth-module voordat u de testcode uploadt. Anders zal het uploaden van de testcode mislukken. Sluit de Bluetooth-module opnieuw aan na het uploaden van de testcode.

Upload de testcode met succes, plaats de Bluetooth-module, schakel in en maak verbinding met Bluetooth. De tankrobot kan verschillende functies via de App uitvoeren.

Prima, alle projecten zijn voltooid. Neem gerust contact met ons op als u problemen ondervindt.
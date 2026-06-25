# Project 14 Bluetooth Control Robot

![](media/image-20250908173626827.png)

**Beschrijving**

We hebben de basiskennis van Bluetooth geleerd. In deze les maken we een Bluetooth-bediende slimme auto. In het experiment stellen we standaard de HM-10 Bluetooth-module in als Slave en de mobiele telefoon als Host.

keyes BT car is een APP uitgebracht door het keyestudio-team. U kunt de robotauto er gemakkelijk mee bedienen.

**APP**

- **Android APP**

  - Download de APP hier.

  ![](media/image-20250909110725934.png)

  - Tik op het Tank_Car-pictogram om de Bluetooth APP te openen. Zoals hieronder weergegeven.

    ![](media/image-20250909112000615.png)

  - Nadat u de code naar het UNO R3-bord hebt geüpload, sluit u de Bluetooth-module aan. Het LED-lampje op de Bluetooth-module knippert. Tik vervolgens op de optie CONNECT in de APP om naar Bluetooth te zoeken.

  ![](media/image-20250909112142944.png)

  - Klik om verbinding te maken met Bluetooth. HMSoft verbonden, het Bluetooth-LED brandt normaal.

  ![](media/image-20250909112205361.png)

  - Tik op de knop![](media/image-20250909112233736.png)①, het 8x16 LED-paneel geeft het voorkant-pictogram weer. Laat de knop los, het scherm toont "STOP".
  - Hieronder ziet u de Tank Robot Bluetooth APP-interface en we hebben aangegeven wat elke toets doet:

  ![](media/image-20250909112313634.png)

  ![](media/image-20250909111647904.png)

  ![](media/image-20250909111712783.png)

- **iOS APP**

  - Open de APP Store

    ![](media/wps1.jpg)

  - Klik om keyestudio te zoeken, en u ziet de keyes BT car.

    ![](media/wps2.jpg)

  - Tik om de keyes BT car te openen

  - Om Bluetooth in te schakelen, klikt u op "Connect" in de linkerbovenhoek en zoekt en verbindt u Bluetooth.

  ![](media/wps3.jpg)

  - Tik op het Tank_Car-pictogram om de bedieningsinterface in te voeren.

    ![](media/wps4.jpg)

    ![](media/wps5.jpg)

  - Hieronder ziet u de Tank Robot Bluetooth APP-interface en we hebben aangegeven wat elke toets doet:

![](media/wps6.jpg)

![](media/image-20250909111647904.png)

![](media/image-20250909111712783.png)

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 14.1
 bluetooth test
 http://www.keyestudio.com
*/
char ble_val; //karaktervariabele, gebruikt om de waarde van Bluetooth-ontvangst op te slaan
void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  //controleer of er gegevens in de bufferbuffer zijn
  { ble_val = Serial.read();  //lees de gegevens uit de seriële buffer
    Serial.println(ble_val);  //print uit
  }
}//**************************************************************
```

**Verwijder de Bluetooth-module, upload testcode, sluit de Bluetooth-module opnieuw aan, open de seriële monitor en stel de baudrate in op 9600. Richt op de Bluetooth-module en druk op toetsen in de APP, dan verschijnt het overeenkomstige teken zoals hieronder weergegeven.**

![](media/image-20250908173736780.png)

Het gedetecteerde teken en bijbehorende functie:

![](media/image-20250908173843012.png)

![](media/image-20250908173856246.png)

**Verbindingsschema**

![](media/image-20250908173908251.png)

**Bedrading Aandacht：**

| 8x16 LED-paneel                                              |      | Uitbreidingsbord |
| ------------------------------------------------------------ | ---- | --------------- |
| GND                                                          | →    | -（GND）        |
| VCC                                                          | →    | +（VCC）        |
| SDA                                                          | →    | SDA             |
| SCL                                                          | →    | SCL             |
| Plaats de Bluetooth-module verticaal, u hoeft deze niet aan de STATE- en BRK-pinnen te bevestigen |      |                 |

 **Testcode**

**Opmerking:** Verwijder de Bluetooth-module voordat u testcode uploadt. Anders kunt u de testcode niet uploaden.

```c
/*
 keyestudio Robot Car v2.0
 les 14.2
 bluetooth car
 http://www.keyestudio.com
*/

//Array, gebruikt om de patroongegevens op te slaan, kan zelf worden berekend of verkregen via het modulus-hulpmiddel
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Stel klokpin in op A5
#define SDA_Pin  A4  //Stel gegevenspin in op A4

#define ML_Ctrl 13  //definieer richtingscontroleppin van linkermotor
#define ML_PWM 11   //definieer PWM-controleppin van linkermotor
#define MR_Ctrl 12  //definieer richtingscontroleppin van rechtermotor
#define MR_PWM 3    //definieer PWM-controleppin van rechtermotor

char bluetooth_val; //sla de waarde van Bluetooth-ontvangst op

void setup(){
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    //Wis het scherm
  matrix_display(start01);  //toon startpatroon

  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}

void loop(){
  if (Serial.available())
  {
    bluetooth_val = Serial.read();
    Serial.println(bluetooth_val);
  }
  switch (bluetooth_val) 
  {
     case 'F':  //voorwaartse opdracht
        Car_front();
        matrix_display(front);  //toon voorwaarts ontwerp
        break;
     case 'B':  //achterwaartse opdracht
        Car_back();
        matrix_display(back);  //toon achterwaarts patroon
        break;
     case 'L':  //linksafslagopdracht
        Car_left();
        matrix_display(left);  //toon "linksafslag" teken
        break;
     case 'R':  //rechtsafslagopdracht
        Car_right();
        matrix_display(right);  //toon rechtsafslagteken
       break;
     case 'S':  //stopopdracht
        Car_Stop();
        matrix_display(STOP01);  //toon stoppictogram
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
  IIC_end();   //beëindig het overbrengen van gegevenspatroon
  
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
      digitalWrite(SCL_Pin,LOW);  //trek klokpin SCL_Pin omlaag om de signalen van SDA te wijzigen
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
  //****************************************************************
```

**Testresultaat**

Code succesvol geüpload, DIP-schakelaar staat aan het rechtereinde en voeding ingeschakeld. Na verbinding met Bluetooth kunnen we de slimme auto bedienen via de Bluetooth App.

Druk op![](media/image-20250908174053007.png), de tankrobot gaat vooruit；

klik op![](media/image-20250908174111419.png), de slimme auto gaat achteruit；

druk op![](media/image-20250908174138113.png)knop, de tankrobot draait naar links；klik op![](media/image-20250908174152325.png), de robot draait naar rechts；

houd![](media/image-20250908174213256.png)ingedrukt, het stopt.

Klik op![](media/image-20250908174235827.png)om zwaartekrachtbesturing in te schakelen, tik op![](media/image-20250908174249747.png)opnieuw om zwaartekrachtbesturing uit te schakelen. Tegelijkertijd geeft het 8X16 LED-paneel op de robotauto het overeenkomstige patroon weer.
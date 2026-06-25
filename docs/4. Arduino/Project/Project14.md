# Projekt 14 Bluetooth-Steuerung Roboter

![](media/image-20250908173626827.png)

**Beschreibung**

Wir haben die grundlegenden Kenntnisse über Bluetooth erworben. In dieser Lektion werden wir ein Bluetooth-Fernbedienungs-Smartcar herstellen. Im Experiment gehen wir davon aus, dass das HM-10 Bluetooth-Modul als Slave und das Mobiltelefon als Host fungiert.

keyes BT car ist eine APP, die vom keyestudio-Team entwickelt wurde. Sie können den Roboter damit problemlos steuern.

**APP**

- **Android APP**

  - Bitte laden Sie die APP hier herunter.

  ![](media/image-20250909110725934.png)

  - Tippen Sie auf das Tank_Car-Symbol, um die Bluetooth-APP zu öffnen. Wie unten gezeigt.

    ![](media/image-20250909112000615.png)

  - Nach dem Hochladen des Codes auf das UNO R3-Board verbinden Sie das Bluetooth-Modul. Die LED am Bluetooth-Modul blinkt. Tippen Sie dann auf die Option CONNECT in der APP, um nach Bluetooth zu suchen.

  ![](media/image-20250909112142944.png)

  - Klicken Sie, um das Bluetooth zu verbinden. HMSoft verbunden, die Bluetooth-LED leuchtet normal.

  ![](media/image-20250909112205361.png)

  - Tippen Sie auf die Schaltfläche![](media/image-20250909112233736.png)①, das 8x16 LED-Panel zeigt das Vorne-Symbol an. Lassen Sie die Schaltfläche los, wird „STOP" angezeigt.
  - Unten ist die Tank Robot Bluetooth APP-Oberfläche und wir haben aufgelistet, welche Funktion jede Taste hat:

  ![](media/image-20250909112313634.png)

  ![](media/image-20250909111647904.png)

  ![](media/image-20250909111712783.png)

- **iOS APP**

  - Öffnen Sie den APP Store

    ![](media/wps1.jpg)

  - Klicken Sie, um keyestudio zu suchen, und Sie werden keyes BT car sehen.

    ![](media/wps2.jpg)

  - Tippen Sie, um keyes BT car zu öffnen

  - Um Bluetooth zu öffnen, klicken Sie auf „Connect" in der oberen linken Ecke und suchen und verbinden Sie Bluetooth.

  ![](media/wps3.jpg)

  - Tippen Sie auf das Tank_Car-Symbol, um die Steueroberfläche zu öffnen.

    ![](media/wps4.jpg)

    ![](media/wps5.jpg)

  - Unten ist die Tank Robot Bluetooth APP-Oberfläche und wir haben aufgelistet, welche Funktion jede Taste hat:

![](media/wps6.jpg)

![](media/image-20250909111647904.png)

![](media/image-20250909111712783.png)

**Test-Code**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 14.1
 Bluetooth-Test
 http://www.keyestudio.com
*/
char ble_val; // Zeichenvariable, wird verwendet, um den Wert des Bluetooth-Empfangs zu speichern
void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  // Überprüfen Sie, ob sich Daten im Pufferspeicher befinden
  { ble_val = Serial.read();  // Lesen Sie die Daten aus dem seriellen Puffer
    Serial.println(ble_val);  // Geben Sie aus
  }
}//**************************************************************
```

**Entfernen Sie das Bluetooth-Modul, laden Sie den Test-Code hoch, verbinden Sie das Bluetooth-Modul erneut, öffnen Sie den seriellen Monitor und stellen Sie die Baudrate auf 9600 ein. Richten Sie das Bluetooth-Modul aus und drücken Sie die Tasten auf der APP. Das entsprechende Zeichen wird wie folgt angezeigt.**

![](media/image-20250908173736780.png)

Das erkannte Zeichen und die entsprechende Funktion:

![](media/image-20250908173843012.png)

![](media/image-20250908173856246.png)

**Schaltplan**

![](media/image-20250908173908251.png)

**Verdrahtungshinweise:**

| 8x16 LED-Panel                                               |      | Erweiterungsboard |
| ------------------------------------------------------------ | ---- | --------------- |
| GND                                                          | →    | -（GND）        |
| VCC                                                          | →    | +（VCC）        |
| SDA                                                          | →    | SDA             |
| SCL                                                          | →    | SCL             |
| Setzen Sie das Bluetooth-Modul senkrecht ein. Sie müssen seine STATE- und BRK-Pins nicht anschließen |      |                 |

**Test-Code**

**Hinweis:** Entfernen Sie das Bluetooth-Modul vor dem Hochladen des Test-Codes. Andernfalls können Sie den Test-Code nicht hochladen.

```c
/*
 keyestudio Robot Car v2.0
 Lektion 14.2
 Bluetooth-Auto
 http://www.keyestudio.com
*/

// Array, wird verwendet, um die Daten des Musters zu speichern. Sie können diese selbst berechnen oder mit dem Modulus-Tool abrufen
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Setzen Sie den Taktpin auf A5
#define SDA_Pin  A4  // Setzen Sie den Datenpin auf A4

#define ML_Ctrl 13  // Definieren Sie den Richtungssteuerpin des linken Motors
#define ML_PWM 11   // Definieren Sie den PWM-Steuerpin des linken Motors
#define MR_Ctrl 12  // Definieren Sie den Richtungssteuerpin des rechten Motors
#define MR_PWM 3    // Definieren Sie den PWM-Steuerpin des rechten Motors

char bluetooth_val; // Speichern Sie den Wert des Bluetooth-Empfangs

void setup(){
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    // Löschen Sie die Anzeige
  matrix_display(start01);  // Zeigen Sie das Startmuster an

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
     case 'F':  // Vorwärtsbefehl
        Car_front();
        matrix_display(front);  // Zeigen Sie das Vorwärtsmuster an
        break;
     case 'B':  // Rückwärtsbefehl
        Car_back();
        matrix_display(back);  // Zeigen Sie das Rückwärtsmuster an
        break;
     case 'L':  // Linksabbiegebefehl
        Car_left();
        matrix_display(left);  // Zeigen Sie das Linksabbiegesymbol an
        break;
     case 'R':  // Rechtsabbiegebefehl
        Car_right();
        matrix_display(right);  // Zeigen Sie das Rechtsabbiegesymbol an
       break;
     case 'S':  // Stoppbefehl
        Car_Stop();
        matrix_display(STOP01);  // Zeigen Sie das Stoppbild an
        break;
  }
}

/**************Die Funktion der Punktmatrix-Anzeige****************/
// Diese Funktion wird für die Punktmatrix-Anzeige verwendet
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  // Wählen Sie die Adresse
  
  for(int i = 0;i < 16;i++) // Das Mustermuster hat 16 Bits
  {
     IIC_send(matrix_value[i]); // Daten zum Übertragen von Mustern
  }
  IIC_end();   // Beenden Sie die Übertragung des Datenmusters
  
  IIC_start();
  IIC_send(0x8A);  // Anzeigesteuerung, stellen Sie die Pulsbreite auf 4/16 ein
  IIC_end();
}
// Die Bedingung zum Starten der Datenübertragung
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Datenübertragung
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Jedes Byte hat 8 Bits
  {
      digitalWrite(SCL_Pin,LOW);  // Ziehen Sie den Taktpin SCL_Pin herunter, um die Signale von SDA zu ändern
delayMicroseconds(3);
      if(send_data & 0x01)  // Stellen Sie den hohen und niedrigen Pegel von SDA_Pin gemäß 1 oder 0 jedes Bits ein
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // Ziehen Sie den Taktpin SCL_Pin hoch, um die Datenübertragung zu stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Erkennen Sie Bit für Bit, verschieben Sie also die Daten um eins nach rechts
  }
}
// Das Zeichen, das die Datenübertragung beendet
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
/*************Die Funktion zum Ausführen des Motors**************/
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

**Test-Ergebnis**

Code erfolgreich hochgeladen, DIP-Schalter nach rechts gestellt und eingeschaltet. Nach der Bluetooth-Verbindung können wir das Smartcar mit der Bluetooth-App steuern.

Drücken Sie![](media/image-20250908174053007.png), der Tank-Roboter fährt vorwärts;

Klicken Sie![](media/image-20250908174111419.png), das Smartcar fährt rückwärts;

Drücken Sie![](media/image-20250908174138113.png)-Schaltfläche, der Tank-Roboter dreht sich nach links; Klicken Sie![](media/image-20250908174152325.png), der Roboter dreht sich nach rechts;

Halten Sie![](media/image-20250908174213256.png), es stoppt.

Klicken Sie![](media/image-20250908174235827.png), um die Gravitationssteuerung zu aktivieren. Tippen Sie![](media/image-20250908174249747.png)erneut an, um die Gravitationssteuerung zu beenden. Gleichzeitig zeigt das 8x16 LED-Panel auf dem Roboter das entsprechende Muster an.
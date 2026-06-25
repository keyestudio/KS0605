# Projekt 12 Ultraschall-Verfolgungspanzer

![](media/image-20250908172315808.png)

**Beschreibung**

In Projekt 11 haben wir ein Hindernisvermeidungsauto gebaut. Tatsächlich müssen wir nur den Testcode ändern, um ein Hindernisvermeidungsauto in ein Verfolgungsauto umzuwandeln. In dieser Lektion werden wir einen Ultraschall-Verfolgungsroboter bauen. Der Ultraschallsensor erkennt den Abstand zwischen dem intelligenten Auto und dem Hindernis, um den Panzerwagen zu bewegen.

**Die spezifische Logik des Ultraschall-Verfolgungsroboters ist wie folgt dargestellt:**

| **Erkennung** | **Gemessener Abstand zu vorderen Hindernissen** | **Abstand (Einheit: cm)** |
| ------------- | ----------------------------------------------- | ------------------------- |
| Einstellungen | Servo-Winkel 90°                                |                           |
|               | 8X16 LED-Panel zeigt das Symbol "V"             |                           |
| Wenn          | 20≤ Abstand ≤60                                 |                           |
| Status        | Vorwärts fahren (PWM auf 200 setzen)            |                           |
| Wenn          | 10\<Abstand＜20                                 |                           |
|               | Abstand＞60                                     |                           |
| Status        | Stopp                                           |                           |
| Wenn          | Abstand ≤10                                     |                           |
| Status        | Stopp (PWM auf 200 setzen)                      |                           |

**Ablaufdiagramm**

![](media/image-20250908172442991.png)

**Schaltplan**

![](media/image-20250908172457017.png)

Verkabelungshinweis:

| 1.8x16 LED-Panel |      | V5 Sensor Shield |
| ---------------- | ---- | ---------------- |
| GND              | →    | -（GND）         |
| VCC              | →    | +（VCC）         |
| SDA              | →    | SDA              |
| SCL              | →    | SCL              |

**Testcode**

```c
 /*
 keyestudio Mini Tank Robot V2.1
 Lektion 12
 Ultraschall-Verfolgungspanzer
 http://www.keyestudio.com
*/ 
// Array, wird verwendet, um die Daten des Musters zu speichern, kann selbst berechnet oder mit dem Modulus-Tool erhalten werden
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Taktsignal-Pin auf A5 setzen
#define SDA_Pin  A4  // Daten-Pin auf A4 setzen

#define ML_Ctrl 13  // Richtungssteuer-Pin des linken Motors definieren
#define ML_PWM 11   // PWM-Steuer-Pin des linken Motors definieren
#define MR_Ctrl 12  // Richtungssteuer-Pin des rechten Motors definieren
#define MR_PWM 3   // PWM-Steuer-Pin des rechten Motors definieren
#define Trig 5  // Ultraschall-Trig-Pin
#define Echo 4  // Ultraschall-Echo-Pin
int distance;
int pulsewidth;
#define servoPin 9  // Servo-Pin
void setup(){
  Serial.begin(9600);
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); // Display löschen
  matrix_display(start01);  // Startmuster anzeigen
  pinMode(servoPin, OUTPUT);
  procedure(90); // Servo auf 90° setzen
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  distance = checkdistance();  // Den vom Ultraschallsensor erkannten Abstand der Variablen distance zuweisen
  if (distance >= 20 && distance <= 60) // Bereich zum Vorwärtsfahren
  {
    Car_front();
  }
  else if (distance > 10 && distance < 20)  // Bereich zum Stoppen
  {
    Car_Stop();
  }
  else if (distance <= 10)  // Bereich zum Rückwärtsfahren
  {
    Car_back();
  }
  else  // Andere Situationen, stoppen
  {
    Car_Stop();
  }
}
/***********Funktion für Motorlauf****************/
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

/******************Punktmatrix********************/
// Funktion für Punktmatrix-Anzeige
void matrix_display(unsigned char matrix_value[])
{
  IIC_start(); // Funktion aufrufen, die die Datenübertragung startet
  IIC_send(0xc0);  // Adresse wählen
  
  for(int i = 0;i < 16;i++) // Musterdaten haben 16 Bits
  {
     IIC_send(matrix_value[i]); // Daten zur Übertragung von Mustern
  }

  IIC_end();   // Datenübertragung beenden
  
  IIC_start();
  IIC_send(0x8A);  // Pulsbreite 4/16 wählen, Anzeige steuern
  IIC_end();
}

// Bedingung zum Starten der Datenübertragung
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

// Daten übertragen
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Jedes Byte hat 8 Bits
  {
      digitalWrite(SCL_Pin,LOW);  // Taktsignal-Pin SCL herunterziehen, um die Signale von SDA zu ändern      
delayMicroseconds(3);
      if(send_data & 0x01)  // Hohe und niedrige Pegel von SDA_Pin gemäß 1 oder 0 jedes Bits setzen
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // Taktsignal-Pin SCL hochziehen, um die Datenübertragung zu stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Bit für Bit erkennen, daher die Daten um eins nach rechts verschieben
  }
}
// Zeichen für das Ende der Datenübertragung
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
/***************Ende Punktmatrix-Anzeige******************/
// Funktion zur Servo-Steuerung
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }}
// Funktion zur Steuerung der Ultraschallsensorfunktion, die das Ultraschallsignal steuert
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.20;  // 58.20, das heißt, 2*29.1=58.2
  delay(10);
  return distance;
}
//****************************************************************
```

 **Testergebnis**

Code erfolgreich hochgeladen, DIP-Schalter ist auf das rechte Ende gestellt, das Servo dreht sich auf 90°, "V" wird auf dem 8X16 LED-Panel angezeigt und das intelligente Auto bewegt sich mit dem Hindernis.
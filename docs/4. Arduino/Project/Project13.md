# Projekt 13 IR-Fernbedienungs-Roboter-Panzer

![](media/image-20250908172649810.png)

**Beschreibung**

Die IR-Fernbedienung ist eine der am weitesten verbreiteten Steuerungsarten und wird in Fernsehern, Elektroventilatoren und einigen Haushaltsgeräten verwendet. In diesem Projekt werden wir ein intelligentes Auto mit IR-Fernbedienung bauen. Da wir jeden Tastenwert der IR-Fernbedienung kennen, können wir das intelligente Auto steuern und die Muster auf der Dot-Matrix über den entsprechenden Tastenwert anzeigen.

**Die spezifische Logik des Infrarot-Fernbedienungs-Roboters ist unten dargestellt:**

| Anfangseinstellung                     | Servo-Winkel 90°                        |                                     |
| -------------------------------------- | --------------------------------------- | ----------------------------------- |
|                                        | 8X16 LED-Matrix-Panel zeigt ein "V"-Symbol |                                     |
| **Fernbedienung**                      | **Tastenwert**                          | **Tastenzustand**                   |
| ![](media/image-20250908172904905.png) | FF629D                                  | Vorwärts fahren (PWM auf 200 gesetzt) |
|                                        |                                         | 8X16 LED-Panel zeigt Vorwärtssymbol |
| ![](media/image-20250908172927504.png) | FFA857                                  | Rückwärts fahren (PWM auf 200 gesetzt) |
|                                        |                                         | 8X16 LED-Panel zeigt Rückwärtssymbol |
| ![](media/image-20250908172954542.png) | FF22DD                                  | Nach links drehen                   |
|                                        |                                         | 8X16 LED-Panel zeigt Linksdrehsymbol |
| ![](media/image-20250908173027144.png) | FFC23D                                  | Nach rechts drehen                  |
|                                        |                                         | 8X16 LED-Panel zeigt Rechtsdrehsymbol |
| ![](media/image-20250908173139888.png) | FF02FD                                  | Stopp                               |
|                                        |                                         | 8X16 LED-Panel zeigt "STOP"         |
| ![](media/image-20250908173312378.png) | FF30CF                                  | Nach links rotieren (PWM auf 200 gesetzt) |
|                                        |                                         | 8X16 LED-Panel zeigt Linksdrehsymbol |
| ![](media/image-20250908173336232.png) | FF7A85                                  | Nach rechts rotieren (PWM auf 200 gesetzt) |
|                                        |                                         | 8X16 LED-Panel zeigt Rechtsdrehsymbol |

**Ablaufdiagramm**

![](media/image-20250908173443316.png)

**Schaltschema**

![](media/image-20250908173458023.png)

Achtung: GND, VCC, SDA, SCL des 8x16 LED-Panels sind jeweils mit - (GND), + (VCC), SDA, SCL verbunden. Und "-", "+" und S des IR-Empfängermoduls sind an G (GND), V (VCC) und A0 auf dem Sensor-Shield angeschlossen. Bei unzureichenden digitalen Anschlüssen können die analogen Anschlüsse als digitale Anschlüsse verwendet werden. A0 entspricht Digital 14, A1 entspricht Digital 15.

**Test-Code**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 13
 IR-Fernbedienungs-Panzer
 http://www.keyestudio.com
*/

#include <IRremoteTank.h>
IRrecv irrecv(A0);  // IRrecv irrecv auf A0 setzen
decode_results results;
long ir_rec;  // speichert den empfangenen IR-Wert

// Array, wird verwendet, um die Musterdaten zu speichern, kann selbst berechnet oder mit dem Modulus-Tool erhalten werden
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Taktpin auf A5 setzen
#define SDA_Pin  A4  // Datenpin auf A4 setzen

#define ML_Ctrl 13  // Richtungssteuerpin des linken Motors definieren
#define ML_PWM 11   // PWM-Steuerpin des linken Motors definieren
#define MR_Ctrl 12  // Richtungssteuerpin des rechten Motors definieren
#define MR_PWM 3    // PWM-Steuerpin des rechten Motors definieren

#define servoPin 9 // Pin des Servos
int pulsewidth; // speichert den Pulsbreitenwert des Servos

void setup(){
  Serial.begin(9600);
  irrecv.enableIRIn();  // IR-Empfangsbibliothek initialisieren
  
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); // Bildschirm löschen
  matrix_display(start01);  // Startbild anzeigen
  
  pinMode(servoPin, OUTPUT);
  procedure(90);  // Servo auf 90° drehen
}

void loop(){
  if (irrecv.decode(&results)) // IR-Fernbedienungswert empfangen
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
  
  if (ir_rec == 0xFF629D) // Vorwärts fahren
  {
    Car_front();
    matrix_display(front);  // Vorwärtsbild anzeigen
  }
  if (ir_rec == 0xFFA857)  // Roboter-Auto fährt rückwärts
  {
    Car_back();
    matrix_display(front);  // Rückwärts fahren
  }
  if (ir_rec == 0xFF22DD)   // Roboter-Auto dreht nach links
  {
    Car_T_left();
    matrix_display(left);  // Linksdrehbild anzeigen
  }
  if (ir_rec == 0xFFC23D)   // Roboter-Auto dreht nach rechts
  {
    Car_T_right();
    matrix_display(right);  // Rechtsdrehbild anzeigen
  }
  if (ir_rec == 0xFF02FD)   // Roboter-Auto stoppt
  { 
    Car_Stop();
    matrix_display(STOP01);  // Stoppbild anzeigen
  }
  if (ir_rec == 0xFF30CF)   // Roboter-Auto rotiert gegen den Uhrzeigersinn
  {
    Car_left();
    matrix_display(left);  // Bild der Gegenuhrzeigersinn-Rotation anzeigen
  }
  if (ir_rec == 0xFF7A85)  // Roboter-Auto rotiert im Uhrzeigersinn
  {
    Car_right();
    matrix_display(right);  // Bild der Uhrzeigersinn-Rotation anzeigen
 }
}
/******************Servo steuern*******************/
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}

/******************Dot-Matrix****************/
// Diese Funktion wird für die Dot-Matrix-Anzeige verwendet
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  // Adresse wählen
   for(int i = 0;i < 16;i++) // Das Bild hat 16 Bits
  {
     IIC_send(matrix_value[i]); // Daten zum Übertragen von Mustern
  }
  IIC_end();   // Beendigung der Musterdatenübertragung
  
  IIC_start();
  IIC_send(0x8A);  // Anzeigesteuerung, Pulsbreite auf 4/16 setzen
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

void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Jedes Byte hat 8 Bits, 8 Bits für jedes Zeichen
  {
      digitalWrite(SCL_Pin,LOW);  // Taktpin SCL_Pin herunterziehen, um die Signale von SDA zu ändern
      delayMicroseconds(3);
      if(send_data & 0x01)  // Setzen Sie das High- und Low-Level von SDA_Pin entsprechend 1 oder 0 jedes Bits
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // Taktpin SCL_Pin hochziehen, um die Datenübertragung zu stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Bit für Bit erkennen, daher die Daten um eins nach rechts verschieben
  }
}
// Das Zeichen, das das Ende der Datenübertragung anzeigt
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
/***************Die Funktion zum Ausführen des Motors***************/
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

Nach erfolgreichem Hochladen des Codes und dem Einschalten kann der intelligente Roboter durch die IR-Fernbedienung gesteuert werden. Gleichzeitig wird das entsprechende Muster auf dem 8X16 LED-Panel angezeigt.
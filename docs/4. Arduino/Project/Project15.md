# Projekt 15: Vollständiges funktionsfähiges Projekt

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 15
 Bluetooth-Panzer
 http://www.keyestudio.com
*/

//Array, wird verwendet, um die Daten des Musters zu speichern, kann selbst berechnet oder mit dem Modulus-Tool erhalten werden
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Taktsignal-Pin auf A5 setzen
#define SDA_Pin  A4  //Daten-Pin auf A4 setzen

#define ML_Ctrl 13  //Richtungssteuerungs-Pin des linken Motors definieren
#define ML_PWM 11   //PWM-Steuerungs-Pin des linken Motors definieren
#define MR_Ctrl 12  //Richtungssteuerungs-Pin des rechten Motors definieren
#define MR_PWM 3    //PWM-Steuerungs-Pin des rechten Motors definieren

char bluetooth_val; //Wert des Bluetooth-Empfangs speichern

void setup()
{
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    //Display löschen
  matrix_display(start01);  //Startmuster anzeigen

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
     case 'F':  //Vorwärts-Befehl
        Car_front();
        matrix_display(front);  //Vorwärts-Design anzeigen
        break;
     case 'B':  //Rückwärts-Befehl
        Car_back();
        matrix_display(back);  //Rückwärts-Muster anzeigen
        break;
     case 'L':  //Linksabbiege-Befehl
        Car_left();
        matrix_display(left);  //Zeichen „Linksabbiegen" anzeigen
        break;
     case 'R':  //Rechtsabbiege-Befehl
        Car_right();
        matrix_display(right);  //Zeichen „Rechtsabbiegen" anzeigen
        break;
     case 'S':  //Stopp-Befehl
        Car_Stop();
        matrix_display(STOP01);  //Stoppbild anzeigen
        break;
  }
}

/**************Die Funktion der Dot-Matrix****************/
//Diese Funktion wird für die Dot-Matrix-Anzeige verwendet
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //Adresse wählen
  
  for(int i = 0;i < 16;i++) //Musterdaten haben 16 Bits
  {
     IIC_send(matrix_value[i]); //Daten zur Übertragung von Mustern
  }
  IIC_end();   //Beendigung der Musterübertragung
  
  IIC_start();
  IIC_send(0x8A);  //Anzeigesteuerung, Impulsbreite auf 4/16 setzen
  IIC_end();
}
//Die Bedingung zum Starten der Datenübertragung
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
//Daten übertragen
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Jedes Byte hat 8 Bits
  {
      digitalWrite(SCL_Pin,LOW);  //Taktsignal-Pin SCL herunterziehen, um die Signale von SDA zu ändern
      delayMicroseconds(3);
      if(send_data & 0x01)  //Hohe und niedrige Pegel von SDA_Pin gemäß 1 oder 0 jedes Bits setzen
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //Taktsignal-Pin SCL hochziehen, um die Datenübertragung zu stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  //Bit für Bit erkennen, daher die Daten um eins nach rechts verschieben
  }
}
//Das Zeichen, dass die Datenübertragung endet
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
```

**Testergebnis**

**Hinweis:** Entfernen Sie das Bluetooth-Modul vor dem Hochladen des Testcodes. Andernfalls können Sie den Testcode nicht hochladen. Verbinden Sie das Bluetooth-Modul nach dem Hochladen des Testcodes wieder.

Laden Sie den Testcode erfolgreich hoch, setzen Sie das Bluetooth-Modul ein, schalten Sie die Stromversorgung ein und verbinden Sie sich mit Bluetooth. Der Panzerroboter kann mit der App unterschiedliche Funktionen anzeigen.

Alles klar, alle Projekte sind abgeschlossen. Bitte kontaktieren Sie uns gerne, wenn Sie auf Probleme stoßen.
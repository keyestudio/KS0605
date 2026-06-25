# Projekt 4 Servo-Steuerung

![](media/image-20250908152354194.png)

**Beschreibung**

Ein Servomotor ist ein rotierender Stellantrieb zur Positionsregelung. Er besteht hauptsächlich aus einem Gehäuse, einer Leiterplatte, einem kernlosen Motor, einem Getriebe und einem Positionssensor. Das Funktionsprinzip besteht darin, dass der Servo das von Mikrocontrollern oder Empfängern gesendete Signal empfängt und ein Referenzsignal mit einer Periode von 20 ms und einer Breite von 1,5 ms erzeugt. Anschließend wird die erfasste DC-Vorspannung mit der Spannung des Potentiometers verglichen und die Spannungsdifferenz ausgegeben.

Wenn die Motordrehzahl konstant ist, wird das Potentiometer über das mehrstufige Untersetzungsgetriebe gedreht, wodurch die Spannungsdifferenz 0 wird und der Motor stoppt. Im Allgemeinen beträgt der Drehwinkelbereich eines Servos 0°–180°.

Der Drehwinkel des Servomotors wird durch die Regelung des Tastverhältnisses des PWM-Signals (Pulsweitenmodulation) gesteuert. Die Standardperiode des PWM-Signals beträgt 20 ms (50 Hz). Theoretisch liegt die Breite zwischen 1 ms und 2 ms, in der Praxis jedoch zwischen 0,5 ms und 2,5 ms. Die Breite entspricht dem Drehwinkel von 0° bis 180°. Zu beachten ist, dass bei Motoren verschiedener Hersteller dasselbe Signal zu unterschiedlichen Drehwinkeln führen kann.

![](media/image-20250908152510007.png)

Im Allgemeinen hat ein Servo drei Leitungen in den Farben Braun, Rot und Orange. Die braune Leitung ist die Masseleitung, die rote ist der Pluspol und die orange ist die Signalleitung.

![](media/image-20250908152525491.png)

Die entsprechenden Servowinkel sind nachfolgend dargestellt:

![](media/image-20250908152558682.png)

**Technische Daten**

- Betriebsspannung: DC 4,8 V \~ 6 V
- Betriebswinkelbereich: ca. 180° (bei 500 → 2500 μsec)
- Pulsbreitenbereich: 500 → 2500 μsec
- Leerlaufdrehzahl: 0,12 ± 0,01 sek / 60° (DC 4,8 V) 0,1 ± 0,01 sek / 60° (DC 6 V)
- Leerlaufstrom: 200 ± 20 mA (DC 4,8 V) 220 ± 20 mA (DC 6 V)
- Haltemoment: 1,3 ± 0,01 kg·cm (DC 4,8 V) 1,5 ± 0,1 kg·cm (DC 6 V)
- Haltestrom: ≦ 850 mA (DC 4,8 V) ≦ 1000 mA (DC 6 V)
- Ruhestrom: 3 ± 1 mA (DC 4,8 V) 4 ± 1 mA (DC 6 V)

**Komponenten**

![](media/image-20250908152809268.png)

**Schaltplan:**

![](media/image-20250908152824930.png)**Verdrahtungshinweise:** Die braune Leitung des Servos wird mit GND (G) verbunden, die rote Leitung mit 5 V (V) und die orange Leitung mit dem digitalen Pin 9.

Der Servo muss an eine externe Stromversorgung angeschlossen werden, da er einen hohen Strombedarf für den Antrieb hat. In der Regel liefert das Entwicklungsboard nicht genügend Strom. Ohne angeschlossene Stromversorgung könnte das Entwicklungsboard beschädigt werden.

**Testcode 1**

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 4.1
Servo
http://www.keyestudio.com
*/
#define servoPin 9  //Servo-Pin
int pos; //Winkelvariable des Servos
int pulsewidth; // Pulsbreitenvariable des Servos

void setup() 
{
  pinMode(servoPin, OUTPUT);  //Servo-Pin als Ausgang setzen
  procedure(0); //Servo-Winkel auf 0° setzen
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1) // von 0 Grad bis 180 Grad
  { 
    // in Schritten von 1 Grad
    procedure(pos);              // Servo zur Position in Variable 'pos' bewegen
    delay(15);                   //Rotationsgeschwindigkeit des Servos steuern
  }
  for (pos = 180; pos >= 0; pos -= 1) // von 180 Grad bis 0 Grad
  { 
    procedure(pos);              // Servo zur Position in Variable 'pos' bewegen
    delay(15);                    
  }
}
// Funktion zur Servo-Steuerung
void procedure(int myangle) 
{
  pulsewidth = myangle * 11 + 500;  //Pulsbreitenwert berechnen
  digitalWrite(servoPin,HIGH);
  delayMicroseconds(pulsewidth);   //Die Dauer des High-Pegels entspricht der Pulsbreite
  digitalWrite(servoPin,LOW);
  delay((20 - pulsewidth / 1000));  // Die Periode beträgt 20 ms, der Low-Pegel füllt die restliche Zeit
}
```

Nach erfolgreichem Hochladen des Codes schwenkt der Servo im Bereich von 0° bis 180° hin und her.

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 4.2
 servo
 http://www.keyestudio.com
*/
#include <Servo.h>
Servo myservo;  // Servo-Objekt zur Steuerung eines Servos erstellen
// auf den meisten Boards können zwölf Servo-Objekte erstellt werden
int pos = 0;    // Variable zum Speichern der Servo-Position

void setup() 
{
  myservo.attach(9);  // Servo an Pin 9 mit dem Servo-Objekt verbinden
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1)  // von 0 Grad bis 180 Grad
  {
    // in Schritten von 1 Grad
    myservo.write(pos);              // Servo zur Position in Variable 'pos' bewegen
    delay(15);                       // 15 ms warten, bis der Servo die Position erreicht hat
  }
  for (pos = 180; pos >= 0; pos -= 1) { // von 180 Grad bis 0 Grad
    myservo.write(pos);              // Servo zur Position in Variable 'pos' bewegen
    delay(15);                       // 15 ms warten, bis der Servo die Position erreicht hat
  }
}
```

**Testergebnis**

Nach erfolgreichem Hochladen des Codes und dem Einschalten der Stromversorgung schwenkt der Servo im Bereich von 0° bis 180°.

Das Ergebnis ist identisch. In der Regel wird der Servo über eine Bibliotheksdatei gesteuert.

**Code-Erklärung**

Arduino wird mit **\#include \<Servo.h\>** geliefert (Servo-Funktionen und -Anweisungen).

Im Folgenden sind einige häufig verwendete Anweisungen der Servo-Funktion aufgeführt:

1. attach（Schnittstelle）——Servo-Schnittstelle festlegen; Port 9 und 10 sind verfügbar.
2. write（Winkel）——Anweisung zum Festlegen des Drehwinkels des Servos.
3. read（）——Anweisung zum Auslesen des Servo-Winkels; liest den Befehlswert von „write()".
4. **Hinweis:** Das oben genannte Schreibformat lautet „Servo-Variablenname.spezifische Anweisung（）", zum Beispiel: myservo.attach(9).
# Projekt 5 Ultraschallsensor

**Beschreibung**

![](media/image-20250908154003868.png)

Der HC-SR04 Ultraschallsensor verwendet Sonar zur Entfernungsmessung zu einem Objekt, ähnlich wie Fledermäuse. Er bietet eine hervorragende berührungslose Entfernungserkennung mit hoher Genauigkeit und stabilen Messwerten in einem einfach zu verwendenden Gehäuse. Er wird komplett mit Ultraschall-Sender- und Empfängermodulen geliefert.

Der HC-SR04 bzw. der Ultraschallsensor wird in einer Vielzahl von Elektronikprojekten zur Hinderniserkennung und Entfernungsmessung sowie für verschiedene andere Anwendungen eingesetzt. Hier stellen wir die einfache Methode vor, um die Entfernung mit Arduino und Ultraschallsensor zu messen und wie man den Ultraschallsensor mit Arduino verwendet.

**Technische Daten**

![](media/image-20250908154036832.png)

- Versorgungsspannung: +5V DC
- Ruhestrom: \<2mA
- Betriebsstrom: 15mA
- Effektiver Winkel: \<15°
- Messbereich: 2cm – 400 cm
- Auflösung: 0,3 cm
- Messwinkel: 30 Grad
- Trigger-Eingangsimpulsbreite: 10uS

**Komponenten**

![](media/image-20250908154147825.png)

**Das Funktionsprinzip des Ultraschallsensors**

Wie im obigen Bild dargestellt, ähnelt er zwei Augen. Eines ist der Sender, das andere der Empfänger.

Das Ultraschallmodul sendet nach dem Empfang eines Triggersignals Ultraschallwellen aus. Wenn die Ultraschallwellen auf ein Objekt treffen und reflektiert werden, gibt das Modul ein Echosignal aus. Dadurch kann die Entfernung zum Objekt anhand der Zeitdifferenz zwischen Triggersignal und Echosignal bestimmt werden.

Die Zeit t ist die Zeit, die das ausgesendete Signal benötigt, um auf ein Hindernis zu treffen und zurückzukehren. Die Ausbreitungsgeschwindigkeit des Schalls in der Luft beträgt etwa 343 m/s, und Entfernung = Geschwindigkeit × Zeit. Da die Ultraschallwelle jedoch ausgesendet wird und zurückkommt, entspricht dies der doppelten Entfernung. Daher muss durch 2 dividiert werden: Die mit Ultraschall gemessene Entfernung = (Geschwindigkeit × Zeit) / 2.

1. Verwendungsmethode und Zeitdiagramm des Ultraschallmoduls:

2. Die Verzögerungszeit des Trig-Pins des SR04 muss auf mindestens 10 μs eingestellt werden, um die Entfernungsmessung auszulösen.
3. Nach dem Auslösen sendet das Modul automatisch acht 40-kHz-Ultraschallimpulse und erkennt, ob ein Signal zurückkommt. Dieser Schritt wird automatisch vom Modul ausgeführt.
4. Wenn das Signal zurückkommt, gibt der Echo-Pin einen HIGH-Pegel aus, dessen Dauer der Zeit von der Aussendung der Ultraschallwelle bis zur Rückkehr entspricht.

![](media/image-20250908154407063.png)

Schaltplan des Ultraschallsensors:

![](media/image-20250908154422828.png)

**Anschlussdiagramm**

![](media/image-20250908154455132.png)

Verdrahtungsanleitung:

- Ultraschallsensor keyestudio V5 Sensor Shield
- VCC → 5v(V)
- Trig → 5(S)
- Echo → 4(S)
- Gnd → Gnd(G)

**Testcode**

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 5
Ultrasonic sensor
http://www.keyestudio.com
*/
int trigPin = 5; // Trigger
int echoPin = 4; // Echo
long duration, cm, inches;

void setup() 
{
    // Serielle Schnittstelle starten
    Serial.begin (9600);
    // Ein- und Ausgänge definieren
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);
}
void loop() 
{
    // Der Sensor wird durch einen HIGH-Impuls von 10 oder mehr Mikrosekunden ausgelöst.
    // Vorher einen kurzen LOW-Impuls geben, um einen sauberen HIGH-Impuls zu gewährleisten:
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    // Signal vom Sensor lesen: ein HIGH-Impuls, dessen Dauer die Zeit (in Mikrosekunden) vom Senden des Pings bis zum Empfang seines Echos von einem Objekt ist.
    duration = pulseIn(echoPin, HIGH);
    // Zeit in eine Entfernung umrechnen
    cm = (duration/2) / 29.1; // Durch 29.1 dividieren oder mit 0.0343 multiplizieren
    inches = (duration/2) / 74; // Durch 74 dividieren oder mit 0.0135 multiplizieren
    Serial.print(inches);
    Serial.print("in, ");
    Serial.print(cm);
    Serial.print("cm");
    Serial.println();
    delay(250);
}
```

**Testergebnis**

Laden Sie den Testcode auf das Entwicklungsboard hoch, öffnen Sie den seriellen Monitor und stellen Sie die Baudrate auf 9600 ein. Die gemessene Entfernung wird angezeigt, die Einheit ist cm und Zoll. Wenn Sie den Ultraschallsensor mit der Hand verdecken, wird der angezeigte Entfernungswert kleiner.

![](media/image-20250908154718663.png)

**Code-Erklärung**

**int trigPin = 5;** Dieser Pin ist für die Aussendung von Ultraschallwellen definiert, in der Regel als Ausgang.

**int echoPin = 4;** Dieser ist als Empfangspin definiert, in der Regel als Eingang.

**cm = (duration/2) / 29.1; inches = (duration/2) / 74; by 0.0135**

Die Entfernung kann mit folgender Formel berechnet werden:

Entfernung = (Laufzeit/2) × Schallgeschwindigkeit

Die Schallgeschwindigkeit beträgt: 343 m/s = 0,0343 cm/μs = 1/29,1 cm/μs

Oder in Zoll: 13503,9 in/s = 0,0135 in/μs = 1/74 in/μs

Die Laufzeit muss durch 2 dividiert werden, da berücksichtigt werden muss, dass die Welle ausgesendet wurde, auf das Objekt traf und dann zum Sensor zurückkehrte.

**Erweiterungsübung:**

Wir haben die vom Ultraschall angezeigte Entfernung gemessen. Was wäre, wenn wir eine LED mit der gemessenen Entfernung steuern würden? Versuchen wir es und schließen wir ein LED-Lichtmodul an den D10-Pin an.

![](media/image-20250908154848028.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 5
 Ultrasonic LED
 http://www.keyestudio.com
*/ 
int trigPin = 5;    // Trigger
int echoPin = 4;    // Echo
long duration, cm, inches;

void setup() 
{
  // Serielle Schnittstelle starten
  Serial.begin (9600);
  // Ein- und Ausgänge definieren
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() 
{
  // Der Sensor wird durch einen HIGH-Impuls von 10 oder mehr Mikrosekunden ausgelöst.
  // Vorher einen kurzen LOW-Impuls geben, um einen sauberen HIGH-Impuls zu gewährleisten:
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Signal vom Sensor lesen: ein HIGH-Impuls, dessen Dauer die Zeit (in Mikrosekunden) vom Senden des Pings bis zum Empfang seines Echos von einem Objekt ist.
  duration = pulseIn(echoPin, HIGH);
  // Zeit in eine Entfernung umrechnen
  cm = (duration/2) / 29.1;     // Durch 29.1 dividieren oder mit 0.0343 multiplizieren
  inches = (duration/2) / 74;   // Durch 74 dividieren oder mit 0.0135 multiplizieren
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  Serial.print("cm");
  Serial.println();
  delay(250);
  if (cm>=2 && cm<=10)
  	digitalWrite(10, HIGH);
  delay(1000);
  digitalWrite(10, LOW);
  delay(1000);
}
```

Laden Sie den Testcode auf das Entwicklungsboard hoch und verdecken Sie den Ultraschallsensor mit der Hand. Überprüfen Sie anschließend, ob die LED leuchtet.
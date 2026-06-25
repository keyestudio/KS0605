# Projekt 3 Fotowiderstand-Sensor

![](./media/image-20250902173047302.png)

 **Beschreibung**

Der Fotowiderstand ist ein spezieller Widerstand aus Halbleitermaterialien wie CdS oder Selenid-Septum. Die Oberfläche ist auch mit feuchtigkeitsbeständigem Harz beschichtet, das eine photoleitende Wirkung hat. Er ist empfindlich gegenüber Umgebungslicht. Sein Widerstand variiert je nach unterschiedlichen Lichtintensitäten.

Wir nutzen die Eigenschaften des Fotowiderstands, um die Schaltung zu entwerfen und das Fotowiderstand-Modul zu erzeugen.

Wenn Sie den Signalpin des Fotowiderstand-Moduls mit einem analogen Port verbinden, werden Sie feststellen, dass je stärker die Lichtintensität ist, desto größer die Spannung des analogen Ports und desto größer der analoge Wert ist.

Im Gegenteil, je schwächer die Lichtintensität ist, desto kleiner die Spannung des analogen Ports und desto kleiner ist der analoge Wert.

Basierend darauf können wir das Fotowiderstand-Modul verwenden, um den analogen Wert zu lesen und so die Umgebungslichtintensität zu ermitteln.

 **Spezifikation**

![](./media/image-20250902173349950.png)\

- Widerstand: 5K Ohm-0,5 MOhm
- Schnittstellentyp: analog
- Arbeitsspannung: 3,3V-5V
- Einfache Installation: mit Schraubfixierungslöchern
- Pin-Abstand: 2,54 mm

 **Komponenten**

![](./media/image-20250902173528860.png)

 **Schaltschema:**

![](./media/image-20250902173558747.png)

Die beiden Fotowiderstand-Sensoren sind mit A1 und A2 verbunden, dann wird das Experiment über den mit A1 verbundenen Fotowiderstand abgeschlossen. Lassen Sie uns seinen analogen Wert auslesen.

**Test-Code**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 3.1
 Fotowiderstand
 http://www.keyestudio.com
*/

int sensorPin = A1;    // Wählen Sie den Eingangspin für den Fotowiderstand
int sensorValue = 0;  // Variable zum Speichern des Wertes vom Sensor
void setup() 
{
	Serial.begin(9600);
}

void loop() 
{
    sensorValue = analogRead(sensorPin);  // Lesen Sie den Wert vom Sensor:
    Serial.println(sensorValue);  // Serielle Schnittstelle gibt den Widerstandswert aus
    delay(500);
}
//******************************************************
```

 **Test-Ergebnis**

Laden Sie den Code auf die Entwicklungsplatine hoch, öffnen Sie den seriellen Monitor und überprüfen Sie, ob sein Wert abnimmt, wenn Sie den Fotowiderstand abdecken. Wenn Sie ihn jedoch aufdecken, erhöht sich der Wert.

![](./media/image-20250902174159923.png)

**Code-Erklärung**

**analogRead(sensorPin):** Lesen Sie den analogen Wert des Fotowiderstands über analoge Ports.

**Serial.begin(9600):** Initialisieren Sie die serielle Schnittstelle, die Baudrate der seriellen Kommunikation beträgt 9600.

**Serial.println:** Serielle Schnittstelle gibt aus und führt einen Zeilenumbruch durch.

**Erweiterungspraktikum**

Wir haben gelernt, wie man den Wert des Fotowiderstands ausliest. Lassen Sie uns den Fotowiderstand mit einer LED kombinieren und den Status der LED beobachten.

![](./media/image-20250902174256941.png)

PWM begrenzt die Helligkeit, daher ist die LED mit PWM-Pins verbunden. Verbinden Sie die LED mit Pin 10, behalten Sie den Pin des Fotowiderstands unverändert bei und entwerfen Sie dann den Code:

```c
/*keyestudio Mini Tank Robot V2.1
Lektion 3.2
Fotowiderstand-analoger Ausgang
http://www.keyestudio.com
*/
int analogInPin = A1;  // Analoger Eingangspin, an dem der Fotowiderstand angeschlossen ist
int analogOutPin = 10; // Analoger Ausgangspin, an dem die LED angeschlossen ist
int sensorValue = 0;        // Vom Sensor gelesener Wert
int outputValue = 0;        // Wert, der an PWM (analoger Ausgang) ausgegeben wird

void setup() 
{
	Serial.begin(9600);
 }
void loop() 
{
  // Lesen Sie den analogen Eingabewert:
  sensorValue = analogRead(analogInPin);
  // Ordnen Sie ihn dem Bereich des analogen Ausgangs zu:
  outputValue = map(sensorValue, 0, 1023, 0, 255);
  // Ändern Sie den analogen Ausgabewert:
  analogWrite(analogOutPin, outputValue);
  // Warten Sie 2 Millisekunden, bevor die nächste Schleife ausgeführt wird, damit der Analog-Digital-
  // Wandler nach der letzten Messung stabilisiert:
 Serial.println(sensorValue);  // Serielle Schnittstelle gibt den Wert des Fotowiderstands aus
 delay(2);
}
//***************************************************************
```

Laden Sie den Code hoch und drücken Sie ihn mit der Hand, um die LED-Helligkeit zu beobachten.
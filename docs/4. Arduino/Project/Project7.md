# Projekt 7 Bluetooth-Fernsteuerung

**Beschreibung**

Bluetooth, ein einfaches drahtloses Kommunikationsmodul, ist in den letzten Jahrzehnten weit verbreitet und wird in den meisten batteriebetriebenen Geräten verwendet, da es benutzerfreundlich ist.

![](media/image-20250908161056017.png)

In den letzten Jahren gab es viele Upgrades des Bluetooth-Standards, um die Anforderungen der Kunden und die technologische Entwicklung sowie den Zeittrend zu erfüllen.

In den letzten Jahren haben sich viele Dinge verändert, darunter die Datenübertragungsrate, der Stromverbrauch bei tragbaren Geräten und IoT-Geräten sowie das Sicherheitssystem.

Hier werden wir das HM-10 BLE 4.0 mit dem Arduino Board kennenlernen. Das HM-10 ist ein leicht verfügbares Bluetooth 4.0-Modul. Dieses Modul wird zur Herstellung einer drahtlosen Datenkommunikation verwendet. Das Modul wurde mit dem Bluetooth Low Energy (BLE) System on Chip (SoC) CC2540 oder CC2541 von Texas Instruments entwickelt.

**Spezifikation**

- Bluetooth-Protokoll: Bluetooth-Spezifikation V4.0 BLE.
- Keine Byte-Grenze bei der seriellen Datenübertragung.
- In offener Umgebung Kommunikation über 100 m Ultrafernstrecke mit iPhone4s.
- Arbeitsfrequenz: 2,4-GHz-ISM-Band.
- Modulationsverfahren: GFSK (Gaussian Frequency Shift Keying).
- Sendeleistung: -23 dBm, -6 dBm, 0 dBm, 6 dBm, kann durch AT-Befehl geändert werden.
- Empfindlichkeit: ≤-84 dBm bei 0,1 % BER.
- Übertragungsrate: Asynchron: 6 K Bytes; Synchron: 6 K Bytes.
- Sicherheitsmerkmale: Authentifizierung und Verschlüsselung.
- Unterstützter Service: Central & Peripheral UUID FFE0, FFE1.
- Stromverbrauch: Automatischer Schlafmodus, Standby-Strom 400 µA ~ 800 µA, 8,5 mA während der Übertragung.
- Stromversorgung: 5 V DC.
- Arbeitstemperatur: –5 bis +65 Grad Celsius.

**Komponenten**

![](media/image-20250908161515087.png)

**Schaltplan**

**1. STATE:** *State-Test-Pins, verbunden mit interner LED, normalerweise nicht verbunden.*

**2. RXD:** *Serielle Schnittstelle, Empfängerterminal.*

**3. TXD:** *Serielle Schnittstelle, Senderterminal.*

**4. GND:** *Masse.*

**5. VCC:** *Positive Stromversorgung.*

**6. EN/BRK:** *Verbindungsunterbrechung, bedeutet Unterbrechung der Bluetooth-Verbindung, normalerweise nicht verbunden.*

![](media/image-20250908161703926.png)

**Test-Code**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 7.1
 Bluetooth 
http://www.keyestudio.com
*/

char ble_val; // Zeichenvariable: speichert den Wert des Bluetooth-Empfangs

void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  // Überprüfen, ob Daten im seriellen Puffer vorhanden sind
  {
    ble_val = Serial.read();  // Daten aus dem seriellen Puffer lesen
    Serial.println(ble_val);  // Ausgeben
  }
}
//*******************************************
```

(Es gibt einen Konflikt zwischen der seriellen Kommunikation des Codes und der Bluetooth-Kommunikation beim Hochladen des Codes. Daher sollte das Bluetooth-Modul vor dem Hochladen des Codes nicht angeschlossen werden.)

Nach dem Hochladen des Codes auf die Entwicklungsplatine das Bluetooth-Modul einsetzen und auf Befehle vom Mobiltelefon warten.

**APP herunterladen**

Der Code dient zum Lesen des empfangenen Signals, und wir benötigen auch ein Gerät zum Senden des Signals. In diesem Projekt senden wir Signale, um das Roboter-Auto über das Mobiltelefon zu steuern.

Dann müssen wir die APP herunterladen.

**iOS-System**

**Hinweis: Erlauben Sie der APP, auf „Standort" in den Einstellungen Ihres Mobiltelefons zuzugreifen, wenn Sie sich mit dem Bluetooth-Modul verbinden. Andernfalls funktioniert Bluetooth möglicherweise nicht.**

Gehen Sie zum APP STORE und suchen Sie nach **BLE Scanner 4.0, dann laden Sie es herunter.**

![](media/image-20250908162043691.png)

**Android-System**

Bitte laden Sie die APP hier herunter.

**Und erlauben Sie der APP, auf „Standort" zuzugreifen. Sie können „Standort" in den Einstellungen Ihres Mobiltelefons aktivieren.**

![](media/image-20250909115039773.png)

![](media/image-20250908162115901.png)

1. Öffnen Sie nach der Installation die App und aktivieren Sie die Berechtigung „Standort und Bluetooth".
2. Wir nehmen die iOS-Version als Beispiel. Die Bedienungsweise der Android-Version ist fast gleich.
3. Scannen Sie das Bluetooth-Modul, um Bluetooth BLE 4.0 zu finden. Der Name ist HMSoft. Klicken Sie dann auf „Verbinden", um sich mit Bluetooth zu verbinden und es zu verwenden.

![](media/image-20250908162157692.png)

4. Nach der Verbindung mit HMSoft klicken Sie darauf, um mehrere Optionen zu erhalten, wie z. B. Geräteinformationen, Zugriffsberechtigung, Allgemein und benutzerdefinierten Service. Wählen Sie „CUSTOM SERVICE".

![](media/image-20250908162224719.png)

5. Dann wird die folgende Seite angezeigt.

![](media/image-20250908162314786.png)

6. Klicken Sie auf (Read, Notify, WriteWithoutResponse), um die folgende Seite zu öffnen.

![](media/image-20250908162335862.png)

7. Klicken Sie auf **Write Value, es erscheint die Schnittstelle zum Eingeben von HEX oder Text.**

![](media/image-20250908162354140.png)

8. Öffnen Sie den seriellen Monitor auf Arduino und geben Sie eine 0 oder ein anderes Zeichen in der Text-Schnittstelle ein.

   ![](media/image-20250908162413278.png)

9. Klicken Sie dann auf „Write", öffnen Sie den seriellen Monitor, um zu überprüfen, ob ein „0"-Signal vorhanden ist.

   ![](media/image-20250908162441251.png)

**Code-Erklärung**

**Serial.available()** : Die aktuellen verbleibenden Zeichen beim Rückgabepuffer. Im Allgemeinen wird diese Funktion verwendet, um zu überprüfen, ob Daten im Puffer vorhanden sind. Wenn Serial.available() > 0, bedeutet dies, dass die serielle Schnittstelle Daten empfangen hat und gelesen werden können.

**Serial.read()：** Lesen Sie ein Datenbyte im Puffer der seriellen Schnittstelle. Wenn beispielsweise ein Gerät Daten über die serielle Schnittstelle an Arduino sendet, können wir die Daten mit „Serial.read()" lesen.

**Erweiterungspraxis**

Wir können einen Befehl über das Mobiltelefon senden, um eine LED ein- und auszuschalten.

D10 ist mit einer LED verbunden, wie unten gezeigt:

![](media/image-20250908162550263.png)

**Code-Erklärung**

**Serial.available()** : Die aktuellen verbleibenden Zeichen beim Rückgabepuffer. Im Allgemeinen wird diese Funktion verwendet, um zu überprüfen, ob Daten im Puffer vorhanden sind. Wenn Serial.available() > 0, bedeutet dies, dass die serielle Schnittstelle Daten empfangen hat und gelesen werden können.

**Serial.read()：** Lesen Sie ein Datenbyte im Puffer der seriellen Schnittstelle. Wenn beispielsweise ein Gerät Daten über die serielle Schnittstelle an Arduino sendet, können wir die Daten mit „Serial.read()" lesen.

**Erweiterungspraxis**

Wir können einen Befehl über das Mobiltelefon senden, um eine LED ein- und auszuschalten.

D10 ist mit einer LED verbunden, wie unten gezeigt:

![](media/image-20250908162720671.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 7.2
 Bluetooth 
 http://www.keyestudio.com
*/ 
int ledpin=11;
void setup()
{Serial.begin(9600);
 pinMode(ledpin,OUTPUT);
}
void loop()
{ int i;
  if (Serial.available())
  {i=Serial.read();
    Serial.println("DATEN EMPFANGEN:");
    if(i=='1')
    { digitalWrite(ledpin,1);
      Serial.println("LED an");
    }
    if(i=='0')
    { digitalWrite(ledpin,0);
      Serial.println("LED aus");
    }
  }
}//*******************************************
```

![](media/image-20250908162739769.png)

![](media/image-20250908162747210.png)

Klicken Sie auf „Write" in der APP. Wenn Sie 1 eingeben, leuchtet die LED auf; wenn Sie 0 eingeben, geht die LED aus. (Denken Sie daran, das Bluetooth-Modul nach Abschluss des Experiments zu entfernen, da sonst das Code-Brennen beeinträchtigt wird).
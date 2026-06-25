# 2. Produktmontage

Nachdem alle Teile dieses Bausatzes überprüft wurden, muss der Panzerroboter zusammengebaut werden. Bitte montieren Sie das Smart Car gemäß den folgenden Anweisungen.

## Montagevideo

[Video herunterladen](video.7z).

> **Hinweis:** Das Montagevideo befindet sich in der Datei `video.7z`, die diesem Paket beiliegt. Bitte entpacken Sie die Datei, um `video/KS0605.mp4` anzusehen.

**Hinweis: Entfernen Sie zunächst die Schutzfolie von der Platine, bevor Sie das Smart Car montieren.**

## Schritt 1: Unterbodenmotor einbauen

Bereiten Sie folgende Teile vor:

- M4-Mutter \* 2
- Metallmotor \*2
- Metallhalterung \*2
- Kupplung \*2
- Blaue Stützteile \*2
- M4\*12MM Innensechskantschraube \* 2
- M1,5 Innensechskantschlüssel vernickelt \*1
- M3 Innensechskantschlüssel vernickelt \*1
- M2,5 Innensechskantschlüssel vernickelt \*1
- M3\*8MM Innensechskantschraube \* 4

![TK_02](media/TK_02.png)

![TK_03](media/TK_03.png)

**Hinweis: Montieren Sie den Motor auf der anderen Seite auf dieselbe Weise.**

## Schritt 2: Antriebsrad einbauen

Bereiten Sie folgende Teile vor:

- M3*8MM Innensechskantschraube \* 2
- M4\*50MM Innensechskantschraube \* 2
- Tragrad für Panzer \* 2
- Flanschlager \* 4
- Unterlegscheibe \*2
- Raupenkette \*2
- M4 Sicherungsmutter \* 2
- M3 Innensechskantschlüssel vernickelt \*1
- M2,5 Innensechskantschlüssel vernickelt \*1

![TK_04](media/TK_04.png)

![TK_05](media/TK_05.png)

![TK_06](media/TK_06.png)

![TK_07](media/TK_07.png)

## Schritt 3: Batteriehalter einbauen

Bereiten Sie folgende Teile vor:

- Batteriehalter \*1
- M3-Mutter \* 2
- Blaue Metallhalterung \*2
- M4-Mutter \*8
- M3\*10MM Senkkopfschraube \* 2
- M4\*40MM Innensechskantschraube \*4
- M2,5 Innensechskantschlüssel vernickelt \*1
- M3 Innensechskantschlüssel vernickelt \*1
- M3\*25MM Innensechskantschraube \*4
- M3*45MM Sechskant-Abstandsbolzen (Messing) *4
- Schraubendreher

![TK_08](media/TK_08.png)

![TK_09](media/TK_09.png)

Befestigen Sie nach Abschluss des Montagevorgangs die Metallhalterung mit vier M4\*40MM-Innensechskantschrauben und vier M4-Muttern am Motorrad.

![TK_10](media/TK_10.png)

![TK_11](media/TK_11.png)

![TK_12](media/TK_12.png)

![TK_13](media/TK_13.png)

## Schritt 4: Acrylplatte und Sensoren montieren

- Acrylplatte \* 2
- L-förmige schwarze Halterung \*1
- Fotozellensenor \*2
- IR-Empfängermodul \*1
- 8X16 LED-Panel \*1
- M2-Mutter \*4
- M3-Mutter \*10
- M3\*6MM Innensechskantschraube \* 8
- M3\*8MM Innensechskantschraube \* 8
- M2,5 Innensechskantschlüssel \*1
- M3\*12MM Rundkopfschraube \*6
- M3\*10MM Sechskant-Abstandsbolzen (Messing) \*8
- M2\*10MM Rundkopfschraube \* 4
- Schraubendreher

![TK_14](media/TK_14.png)

![TK_15](media/TK_15.png)

![TK_16](media/TK_16.png)

![TK_17](media/TK_17.png)

![TK_18](media/TK_18.png)

![TK_19](media/TK_19.png)

![TK_20](media/TK_20.png)

![TK_21](media/TK_21.png)

![TK_22](media/TK_22.png)

![TK_23](media/TK_23.png)

## Schritt 5: Servo-Plattform einbauen

Bereiten Sie folgende Teile vor:

-   Servo \*1
-   Schwarzes Gimbal \*1
-   Kabelbinder \*2
-   M2x8 Rundkopf-Kreuzschlitz-Blechschraube \*2
-   Ultraschallsensor \*1
-   M2\*4-Schraube \*1
-   M1,2\*5-Schraube \*4
-   Schraubendreher

**Hinweis:** Für eine einfache Fehlersuche sollte das Ultraschallmodul geradeaus ausgerichtet und der Servowinkel auf 90° eingestellt sein. Daher muss der Servo vor der Montage der Servo-Plattform auf 90° eingestellt werden.

Kopieren Sie den folgenden 90-Grad-Code und laden Sie ihn auf das Entwicklungsboard hoch. Das an Port D9 angeschlossene Lenkgetriebe dreht sich auf 90°.

> Zum Hochladen des Codes benötigen Sie die Arduino IDE. Bitte installieren Sie zunächst die Arduino IDE gemäß den Abschnitten 4.2–4.4. (Software-Download, Arduino IDE einrichten und Bibliothek hinzufügen)

```c
#define servoPin 9 //Servo-Pin
int pos; //Winkelvariable des Servos
int pulsewidth; // Pulsbreitenvariable des Servos

void setup() 
{
    pinMode(servoPin, OUTPUT); //Servo-Pin als Ausgang setzen
    procedure(0); //Servowinkel auf 0° setzen
}

void loop() 
{
	procedure(90); // Servo auf Position 90° fahren
}

// Funktion zur Steuerung des Servos
void procedure(int myangle) 
{
    pulsewidth = myangle * 11 + 500; //Pulsbreitenwert berechnen
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth); //Die Dauer des High-Pegels entspricht der Pulsbreite
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000)); // Der Zyklus beträgt 20ms, der Low-Pegel hält die restliche Zeit an
}
```

![TK_24](media/TK_24.png)

![](media/image-20250902144145590.png)

**Hinweis:** Die M1,2\*5-Schrauben befinden sich im Beutel der Kunststoffplattform.

![TK_25](media/TK_25.png)

![TK_26](media/TK_26.png)

## Schritt 6: Sensoren und Platinen einbauen

Bereiten Sie folgende Teile vor:

- M3\*6MM Rundkopfschraube \*12
- L298P Shield \*1
- V4.0 Board \*1
- V5 Sensor Shield \*1
- Schraubendreher \*1
- Bluetooth-Modul \*1
- M2,5 Innensechskantschlüssel vernickelt \*1

![TK_27](media/TK_27.png)

![TK_28](media/TK_28.png)

![TK_29](media/TK_29.png)

![TK_30](media/TK_30.png)

![TK_31](media/TK_31.png)

![TK_32](media/TK_32.png)

![TK_33](media/TK_33.png)

![TK_34](media/TK_34.png)



## Schritt 7: Verkabelungsanleitung

![](media/image-20250902144534790.png)

![](media/image-20250902144551034.png)

![](media/image-20250902144559983.png)

![](media/image-20250902144849310.png)

![](media/image-20250902144902221.png)

## Schritt 8: LED-Panel anschließen

![](media/image-20250902145026905.png)

![](media/image-20250902145112884.png)

![](media/image-20250902145129382.png)

| LED-Panel                              | V5 Sensor Shield                       |
| -------------------------------------- | -------------------------------------- |
| GND                                    | -(GND)                                 |
| VCC                                    | +(VCC)                                 |
| SDA                                    | SDA                                    |
| SCL                                    | SCL                                    |
| ![](media/image-20250902145404151.png) | ![](media/image-20250902145414755.png) |

## Schritt 9: Alle Teile der Acrylplatte einbauen

![](media/image-20250902145506652.png)

![](media/image-20250902145615504.png)

![](media/image-20250902145822634.png)

![](media/image-20250902145854886.png)

![](media/image-20250902145934002.png)

![](media/image-20250902150004173.png)

![](media/image-20250902150032438.png)

![](media/image-20250902150052468.png)

![](media/image-20250902150217564.png)

![](media/image-20250902150508905.png)

![](media/image-20250902150522753.png)

![](media/image-20250902150532987.png)

![](media/image-20250902150711706.png)

## Schritt 10: Panzerroboter

**Hinweis:** Entfernen Sie das Bluetooth-Modul, bevor Sie den Testcode hochladen. Andernfalls schlägt das Hochladen des Testcodes fehl.

![](media/image-20250902151034545.png)

**Multifunktionaler Roboterwagen**

![](media/image-20250902151133169.png)

  **Beschreibung**

In den vorherigen Projekten hat der Panzerwagen jeweils nur eine einzelne Funktion ausgeführt. In dieser Lektion werden jedoch alle Funktionen integriert, um das Smart Car über Bluetooth zu steuern.

Nachfolgend finden Sie ein vereinfachtes Ablaufdiagramm des multifunktionalen Roboterwagens als Referenz.

![](media/image-20250902151215210.png)

  **Schaltplan**

![](media/image-20250902151230702.png)

**Achtung:** Stellen Sie sicher, dass alle Komponenten angeschlossen sind.

Verkabelungsanleitung:

| 8x16 LED-Panel | | Erweiterungsplatine |
| -------------- | ---- | --------------- |
| GND            | →    | -（GND）        |
| VCC            | →    | +（VCC）        |
| SDA            | →    | SDA             |
| SCL            | →    | SCL             |

![](media/image-20250902152539713.png)

| Ultraschallmodul  |      |        |
| ----------------- | ---- | ------ |
| VCC               | →    | 5v(V)  |
| Trig              | →    | 5(S)   |
| Echo              | →    | 4(S)   |
| Gnd               | →    | Gnd(G) |

![](media/image-20250902152857086.png)

![](media/image-20250902152906103.png)

| Servomotor      |      |        |
| --------------- | ---- | ------ |
| Servomotor      | →    | Gnd(G) |
| Rotes Kabel     | →    | 5v(V)  |
| Oranges Kabel   | →    | 9      |

![](media/image-20250902154418006.png)

![](media/image-20250902154820948.png)

| Bluetooth-Modul                                    |      |          |
| -------------------------------------------------- | ---- | -------- |
| RXD                                                | →    | TX       |
| TXD                                                | →    | RX       |
| GND                                                | →    | -（GND） |
| VCC                                                |      | +（VCC） |
| STATE- und BRK-Pins müssen nicht angeschlossen werden |      |          |

![](media/image-20250902155229663.png)

![](media/image-20250902155236836.png)

| IR-Empfängermodul  |      | Sensor Shield |
| ------------------ | ---- | ------------- |
| －                 | →    | G（GND）      |
| +                  | →    | V（VCC）      |
| S                  | →    | A0            |

![](media/image-20250902155444270.png)

![](media/image-20250902155452133.png)

| Linker Fotowiderstand  |      | Sensor Shield |
| ---------------------- | ---- | ------------- |
| －                     | →    | G（GND）      |
| ＋                     | →    | V（VCC）      |
| S                      | →    | A1            |
|                        |      |               |
| Rechter Fotowiderstand |      | Sensor Shield |
| －                     | →    | G（GND）      |
| ＋                     | →    | V（VCC）      |
| S                      | →    | A2            |

![](media/image-20250902155938106.png)

![](media/image-20250902155946213.png)

 Montage abgeschlossen.
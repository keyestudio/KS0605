# 2. Productinstallatie

Na het controleren van alle onderdelen in deze kit moeten we de tankrobot monteren. Laten we de slimme auto installeren volgens de volgende instructies.

## Assemblagevideo

[Video downloaden](video.7z).

> **Opmerking:** De assemblagevideo is beschikbaar in het bestand `video.7z` dat in dit pakket is opgenomen. Pak het uit om `video/KS0605.mp4` te bekijken.

**Opmerking: Verwijder eerst de plastic film van het bord bij het installeren van de slimme auto.**

## Stap 1: Onderste motor installeren

Bereid de volgende onderdelen voor:

- M4 Moer \* 2
- Metalen Motor \*2
- Metalen Houder \*2
- Koppeling \*2
- Blauwe Ondersteunende Onderdelen \*2
- M4\*12MM Interne Zeskantschroef \* 2
- M1.5 Zeskant Vernikkelde Inbussleutel \*1
- M3 Zeskant Vernikkelde Inbussleutel \*1
- M2.5 Zeskant Vernikkelde Inbussleutel \*1
- M3\*8MM Interne Zeskantschroef \* 4

![TK_02](media/TK_02.png)

![TK_03](media/TK_03.png)

**Opmerking: monteer de motor aan de andere kant op dezelfde manier.**

## Stap 2: Aandrijfwiel installeren

Bereid de volgende onderdelen voor:

- M3*8MM Interne Zeskantschroef \* 2
- M4\*50MM Interne Zeskantschroef \* 2
- Tankdraagwiel \* 2
- Flenslagering \* 4
- Pakking\*2
- Rupsband \*2
- M4 Zelfborgende Moer \* 2
- M3 Zeskant Vernikkelde Inbussleutel \*1
- M2.5 Zeskant Vernikkelde Inbussleutel \*1

![TK_04](media/TK_04.png)

![TK_05](media/TK_05.png)

![TK_06](media/TK_06.png)

![TK_07](media/TK_07.png)

## Stap 3: Batterijhouder installeren

Bereid de volgende onderdelen voor:

- Batterijhouder \*1
- M3 Moer \* 2
- Blauwe Metalen houder \*2
- M4 Moer \*8
- M3\*10MM Platte Kopschroef \* 2
- M4\*40MM Interne Zeskantschroef \*4
- M2.5 Zeskant Vernikkelde Inbussleutel\*1
- M3 Zeskant Vernikkelde Inbussleutel \*1
- M3\*25MM Interne Zeskantschroef \*4
- M3*45MM Zeskantige Koperen Zuil *4
- Schroevendraaier

![TK_08](media/TK_08.png)

![TK_09](media/TK_09.png)

Bevestig de metalen houder op het motorwiel met vier M4\*40MM interne zeskantschroeven en vier M4 moeren wanneer het montageproces is voltooid.

![TK_10](media/TK_10.png)

![TK_11](media/TK_11.png)

![TK_12](media/TK_12.png)

![TK_13](media/TK_13.png)

##  Stap 4: Acrylplaat en sensoren monteren

- Acrylplaat \* 2
- L-vormige Zwarte Beugel \*1
- Fotocelsensor \*2
- IR Ontvanger Module \*1
- 8X16 LED-paneel \*1
- M2 Moer \*4
- M3 Moer \*10
- M3\*6MM Interne Zeskantschroef \* 8
- M3\*8MM Interne Zeskantschroef \* 8
- M2.5 Zeskant Inbussleutel \*1
- M3\*12MM Ronde Kopschroef \*6
- M3\*10MM Zeskantige Koperen Buis \*8
- M2\*10MM Ronde Kopschroef \* 4
- Schroevendraaier

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

## Stap 5: Servomotor Platform installeren

Bereid de volgende onderdelen voor:

-   Servomotor \*1
-   Zwarte Gimbal \*1
-   Kabelbinder \*2
-   M2x8 Ronde Kopschroef met Kruis \*2
-   Ultrasone Sensor \*1
-   M2\*4 Schroef \*1
-   M1.2\*5 Schroef \*4
-   Schroevendraaier

**Opmerking: **voor gemakkelijk debuggen, houd de ultrasone module recht naar voren en de hoek van de servomotor op 90°. Daarom moeten we de servomotor op 90° instellen voordat we het servomotor platform installeren.

Stel de 90-graden code in, kopieer de code en upload deze naar het ontwikkelingsbord. De stuurinrichting die is aangesloten op poort D9 zal naar 90° draaien.

> Om code te uploaden, hebt u de Arduino IDE nodig. Installeer eerst de Arduino IDE door secties 4.2–4.4 te volgen. (Software Download, Arduino IDE instellen en bibliotheek toevoegen)

```c
#define servoPin 9 //servo Pin
int pos; //de hoekveranderlijke van servo
int pulsewidth; // pulsbreedte veranderlijke van servo

void setup() 
{
    pinMode(servoPin, OUTPUT); //stel servo pin in op OUTPUT
    procedure(0); //stel de hoek van servo in op 0°
}

void loop() 
{
	procedure(90); // zeg servo om naar positie in variabele 90° te gaan
}

// functie om servo te besturen
void procedure(int myangle) 
{
    pulsewidth = myangle * 11 + 500; //bereken de waarde van pulsbreedte
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth); //De duur van hoog niveau is pulsbreedte
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000)); // de cyclus is 20ms, het lage niveau duurt de rest van de tijd
}
```

![TK_24](media/TK_24.png)

![](media/image-20250902144145590.png)

**Opmerking: **U kunt M1.2\*5 schroeven vinden in de zak van het kunststof platform.

![TK_25](media/TK_25.png)

![TK_26](media/TK_26.png)

## Stap 6: Sensoren en borden installeren

Bereid de volgende onderdelen voor:

- M3\*6MM Ronde Kopschroef \*12
- L298P Shield \*1
- V4.0 Bord \*1
- V5 Sensor Shield \*1
- Schroevendraaier \*1
- Bluetooth Module \*1
- M2.5 Zeskant Vernikkelde Inbussleutel \*1

![TK_27](media/TK_27.png)

![TK_28](media/TK_28.png)

![TK_29](media/TK_29.png)

![TK_30](media/TK_30.png)

![TK_31](media/TK_31.png)

![TK_32](media/TK_32.png)

![TK_33](media/TK_33.png)

![TK_34](media/TK_34.png)



## Stap 7: Aansluitingsgids

![](media/image-20250902144534790.png)

![](media/image-20250902144551034.png)

![](media/image-20250902144559983.png)

![](media/image-20250902144849310.png)

![](media/image-20250902144902221.png)

##  Stap 8: LED-paneel bedraden

![](media/image-20250902145026905.png)

![](media/image-20250902145112884.png)

![](media/image-20250902145129382.png)

| LED-paneel                             | V5 Sensor Shield                       |
| -------------------------------------- | -------------------------------------- |
| GND                                    | -(GND)                                 |
| VCC                                    | +(VCC)                                 |
| SDA                                    | SDA                                    |
| SCL                                    | SCL                                    |
| ![](media/image-20250902145404151.png) | ![](media/image-20250902145414755.png) |

## Stap 9: Alle onderdelen van acrylplaat installeren

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

##  Stap 10: Tankrobot

**Opmerking:** Verwijder de Bluetooth-module voordat u testcode uploadt. Anders kunt u de testcode niet uploaden.

![](media/image-20250902151034545.png)

**Multifunctionele Robotauto**

![](media/image-20250902151133169.png)

  **Beschrijving**

In de vorige projecten voert de tankwagen slechts één functie uit. In deze les integreren we echter al zijn functies om de slimme auto via Bluetooth-besturing te besturen.

Hier is een eenvoudig stroomdiagram van de multifunctionele robotauto ter referentie.

![](media/image-20250902151215210.png)

  **Verbindingsdiagram**

![](media/image-20250902151230702.png)

**Let op：**Controleer of elk onderdeel is aangesloten.

Aansluitingsgids:

| 8x16 LED-paneel | | Uitbreidingsbord |
| -------------- | ---- | --------------- |
| GND            | →    | -（GND）        |
| VCC            | →    | +（VCC）        |
| SDA            | →    | SDA             |
| SCL            | →    | SCL             |

![](media/image-20250902152539713.png)

| Ultrasone Module |      |        |
| ----------------- | ---- | ------ |
| VCC               | →    | 5v(V)  |
| Trig              | →    | 5(S)   |
| Echo              | →    | 4(S)   |
| Gnd               | →    | Gnd(G) |

![](media/image-20250902152857086.png)

![](media/image-20250902152906103.png)

| Servomotor |      |        |
| ----------- | ---- | ------ |
| Servomotor | →    | Gnd(G) |
| Rode Draad    | →    | 5v(V)  |
| Oranje Draad | →    | 9      |

![](media/image-20250902154418006.png)

![](media/image-20250902154820948.png)

| Bluetooth Module                        |      |          |
| --------------------------------------- | ---- | -------- |
| RXD                                     | →    | TX       |
| TXD                                     | →    | RX       |
| GND                                     | →    | -（GND） |
| VCC                                     |      | +（VCC） |
| Geen noodzaak om STATE en BRK pinnen aan te sluiten |      |          |

![](media/image-20250902155229663.png)

![](media/image-20250902155236836.png)

| IR Ontvanger Module |      | Sensor Shield |
| ------------------ | ---- | ------------- |
| －                 | →    | G（GND）      |
| +                  | →    | V（VCC）      |
| S                  | →    | A0            |

![](media/image-20250902155444270.png)

![](media/image-20250902155452133.png)

| Linker fotoweerstand  |      | Sensor Shield |
| -------------------- | ---- | ------------- |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A1            |
|                      |      |               |
| Rechter Fotoweerstand |      | Sensor Shield |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A2            |

![](media/image-20250902155938106.png)

![](media/image-20250902155946213.png)

 Installatie voltooid.
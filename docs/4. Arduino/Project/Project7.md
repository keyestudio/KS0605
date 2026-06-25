# Project 7 Bluetooth Remote Control

**Beschrijving**

Bluetooth, een eenvoudige draadloze communicatiemodule, is de afgelopen decennia enorm populair geworden en wordt gebruikt in de meeste batterijgevoede apparaten vanwege de gebruiksvriendelijke functie.

![](media/image-20250908161056017.png)

In de afgelopen jaren zijn er veel upgrades van de Bluetooth-standaard geweest om aan de eisen van klanten en de technologische ontwikkeling tegemoet te komen en in te spelen op de trend van de tijd.

In de afgelopen jaren zijn er veel veranderingen opgetreden, waaronder de gegevensoverdrachtsnelheid, stroomverbruik bij draagbare en IoT-apparaten en beveiligingssystemen.

Hier gaan we meer te weten komen over HM-10 BLE 4.0 met Arduino Board. De HM-10 is een gemakkelijk verkrijgbare Bluetooth 4.0-module. Deze module wordt gebruikt voor het tot stand brengen van draadloze gegevenscommunicatie. De module is ontworpen met behulp van de Texas Instruments CC2540 of CC2541 Bluetooth low energy (BLE) System on Chip (SoC).

**Specificatie**

- Bluetooth-protocol: Bluetooth Specification V4.0 BLE.
- Geen bytelimiet in seriële poort Transceiving.
- In open omgeving, realiseer 100m ultra-afstandscommunicatie met iPhone4s.
- Werkfrequentie: 2.4GHz ISM-band.
- Modulatiemethode: GFSK (Gaussian Frequency Shift Keying).
- Transmissievermogen: -23dbm, -6dbm, 0dbm, 6dbm, kan worden gewijzigd via AT-commando.
- Gevoeligheid: ≤-84dBm bij 0,1% BER.
- Transmissiesnelheid: Asynchrone: 6K bytes; Synchrone: 6k bytes.
- Beveiligingsfunctie: Authenticatie en versleuteling.
- Ondersteunende service: Central & Peripheral UUID FFE0, FFE1.
- Stroomverbruik: Automatische slaapstand, standby-stroom 400uA~800uA, 8,5mA tijdens transmissie.
- Voeding: 5V DC.
- Werktemperatuur: –5 tot +65 graden Celsius.

**Onderdelen**

![](media/image-20250908161515087.png)

**Verbindingsschema**

**1. STATE:** *state test pins, verbonden met interne LED, over het algemeen niet verbonden.*

**2. RXD:** *seriële interface, ontvangsterminal.*

**3. TXD:** *seriële interface, verzendterminal.*

**4. GND:** *Aarde.*

**5. VCC:** *positieve pool van de stroombron.*

**6. EN/BRK:** *break connect, dit betekent het verbreken van de Bluetooth-verbinding, over het algemeen niet verbonden.*

![](media/image-20250908161703926.png)

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 7.1
 bluetooth 
http://www.keyestudio.com
*/

char ble_val; //karaktervariabele: sla de waarde van Bluetooth-ontvangst op

void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  //controleer of er gegevens in de seriële buffer staan
  {
    ble_val = Serial.read();  //Lees gegevens uit seriële buffer
    Serial.println(ble_val);  //Afdrukken
  }
}
//*******************************************
```

(Er zal een tegenstelling zijn tussen seriële communicatie van code en communicatie van Bluetooth bij het uploaden van code. Daarom moet u de Bluetooth-module niet aansluiten voordat u de code uploadt.)

Na het uploaden van code op het ontwikkelingsbord, voeg u de Bluetooth-module in en wacht u op het commando van de mobiele telefoon.

**APP downloaden**

De code is bedoeld voor het lezen van het ontvangen signaal, en we hebben ook iets nodig om signaal te verzenden. In dit project verzenden we signaal om de robotauto via mobiele telefoon te besturen.

Vervolgens moeten we de APP downloaden.

**iOS-systeem**

**Opmerking: Sta de APP toe om "locatie" in de instellingen van uw mobiele telefoon in te stellen bij het verbinden met de Bluetooth-module. Anders kan Bluetooth niet worden verbonden.**

Ga naar APP STORE en zoek naar **BLE Scanner 4.0, download het vervolgens.**

![](media/image-20250908162043691.png)

**Android-systeem**

Download de APP hier.

**En sta de APP toe om "locatie" in te stellen, u kunt "locatie" inschakelen in de instellingen van uw mobiele telefoon.**

![](media/image-20250909115039773.png)

![](media/image-20250908162115901.png)

1. Na installatie opent u de App en schakelt u de machtiging "Locatie en Bluetooth" in.
2. We nemen de iOS-versie als voorbeeld. De bedieningswijze van de Android-versie is bijna hetzelfde.
3. Scan de Bluetooth-module om Bluetooth BLE 4.0 te krijgen. De naam is HMSoft. Klik vervolgens op "connect" om verbinding te maken met Bluetooth en deze te gebruiken.

![](media/image-20250908162157692.png)

4. Na verbinding met HMSoft klikt u erop om meerdere opties te krijgen, zoals apparaatinformatie, toegangsmachtiging, algemeen en aangepaste service. Kies "CUSTOM SERVICE".

![](media/image-20250908162224719.png)

5. Vervolgens verschijnt de volgende pagina.

![](media/image-20250908162314786.png)

6. Klik op (Read, Notify, WriteWithoutResponse) om de volgende pagina in te voeren.

![](media/image-20250908162335862.png)

7. Klik op **Write Value, er verschijnt een interface om HEX of Text in te voeren.**

![](media/image-20250908162354140.png)

8. Open de seriële monitor op Arduino en voer een 0 of ander teken in op de Text-interface.

   ![](media/image-20250908162413278.png)

9. Klik vervolgens op "Write", open de seriële monitor om te controleren of er een "0"-signaal is.

   ![](media/image-20250908162441251.png)

**Codeuitleg**

**Serial.available()** : De huidige resterende tekens wanneer teruggekeerd naar buffergebied. Over het algemeen wordt deze functie gebruikt om te bepalen of er gegevens in de buffer staan. Wanneer Serial.available()>0, betekent dit dat de seriële poort de gegevens heeft ontvangen en kan worden gelezen.

**Serial.read()：**Lees één byte gegevens in de buffer van de seriële poort, bijvoorbeeld wanneer een apparaat gegevens naar Arduino via de seriële poort verzendt, kunnen we gegevens lezen met "Serial.read()".

**Uitbreidingsoefening**

We kunnen via mobiele telefoon een commando verzenden om een LED in en uit te schakelen.

D10 is verbonden met een LED, zoals hieronder weergegeven:

![](media/image-20250908162550263.png)

**Codeuitleg**

**Serial.available()** : De huidige resterende tekens wanneer teruggekeerd naar buffergebied. Over het algemeen wordt deze functie gebruikt om te bepalen of er gegevens in de buffer staan. Wanneer Serial.available()>0, betekent dit dat de seriële poort de gegevens heeft ontvangen en kan worden gelezen.

**Serial.read()：**Lees één byte gegevens in de buffer van de seriële poort, bijvoorbeeld wanneer een apparaat gegevens naar Arduino via de seriële poort verzendt, kunnen we gegevens lezen met "Serial.read()".

**Uitbreidingsoefening**

We kunnen via mobiele telefoon een commando verzenden om een LED in en uit te schakelen.

D10 is verbonden met een LED, zoals hieronder weergegeven:

![](media/image-20250908162720671.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 7.2
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
    Serial.println("DATA RECEIVED:");
    if(i=='1')
    { digitalWrite(ledpin,1);
      Serial.println("led on");
    }
    if(i=='0')
    { digitalWrite(ledpin,0);
      Serial.println("led off");
    }
  }
}//*******************************************
```

![](media/image-20250908162739769.png)

![](media/image-20250908162747210.png)

Klik op "Write" in de APP, wanneer u 1 invoert, gaat de LED aan; wanneer u 0 invoert, gaat de LED uit. (Vergeet niet de Bluetooth-module na het experiment te verwijderen, anders wordt het code-branden beïnvloed).
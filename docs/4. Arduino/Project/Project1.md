# Project 1 LED Knippert

![](media/image-20250908174750401.png)

**Beschrijving**

Voor beginners en enthousiastelingen is LED Knippert een fundamenteel programma. LED, de afkorting van lichtemitterende diodes, bestaat uit Ga, As, P, N chemische verbindingen en dergelijke. De LED kan in diverse kleuren knipperen door de vertragingstijd in de testcode aan te passen. Bij bediening, wanneer GND en VCC onder stroom staan, zal de LED aan gaan als de S-uiteinde op hoog niveau staat; echter, deze zal uit gaan.

**Specificatie**

![](./media/image-20250902164418568.png)

- Besturingsinterface: digitale poort
- Werkspanning: DC 3,3-5V
- Pinafstand: 2,54mm
- LED-weergavekleur: rood

**Componenten**

![](./media/image-20250902164804229.png)

**V5 Sensor Shield**

Het kan lastig zijn wanneer we Arduino-ontwikkelingsborden met talrijke sensoren combineren. De V5 sensor shield, compatibel met Arduino-ontwikkelingsborden, lost dit probleem echter perfect op. Stapel gewoon de V5-kaart erop.

Deze sensor shield kan in 3-pins sensormodules worden ingestoken en breekt enkele communicatiepinnen uit, zoals seriële, IIC- en SPI-communicatie.

**Pinbeschrijving**

![](./media/image-20250902165027854.png)

**Verbindingsschema**

![](./media/image-20250902165110913.png)

Zoals te zien is in het bovenstaande diagram, is de LED verbonden met D2.

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 1.1
 Knippert
 http://www.keyestudio.com
*/
void setup()
{ 
    pinMode(2, OUTPUT);// initialiseer digitale pin 2 als uitgang.
}

void loop() // de loop-functie voert zich oneindig herhaald uit
{
   digitalWrite(2, HIGH); // zet de LED aan (HIGH is het spanningsniveau)
   delay(1000); // wacht een seconde
   digitalWrite(2, LOW); // zet de LED uit door de spanning LOW in te stellen
   delay(1000); // wacht een seconde
}
```

**Testresultaat**

(Er zal een tegenspraak zijn over seriële communicatie tussen code en Bluetooth bij het uploaden van code. Daarom moet u niet met de Bluetooth-module verbinden voordat u de code uploadt.)

Upload het programma op de ontwikkelingsbord, LED knippert met een interval van 1s.

![](./media/image-20250902165335641.png)

**Code-uitleg**

**pinMode(2，OUTPUT) -** Stel pin2 in op OUTPUT

**digitalWrite(2，HIGH) -** Wanneer pin2 op HIGH-niveau (uitgang 5V) of LOW-niveau (uitgang 0V) wordt ingesteld

**Uitbreidingsoefening**

We zijn erin geslaagd de LED te laten knipperen. Laten we nu observeren wat er met de LED verandert als we de pinnen en vertragingstijd aanpassen.

**Verbindingsschema**

![](./media/image-20250902165631206.png)

We hebben de pinnen gewijzigd en de LED verbonden met D10.

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 1.2
 vertraging
 http://www.keyestudio.com
*/
void setup() // initialiseer digitale pin 10 als uitgang.
{  
   pinMode(10, OUTPUT);
}

// de loop-functie voert zich oneindig herhaald uit
void loop() 
{
   digitalWrite(10, HIGH); // zet de LED aan (HIGH is het spanningsniveau)
   delay(100); // wacht 0,1 seconde
   digitalWrite(10, LOW); // zet de LED uit door de spanning LOW in te stellen
   delay(100); // wacht 0,1 seconde
}
```

Het testresultaat toont aan dat de LED sneller knippert. Daarom kunnen we concluderen dat pinnen en tijdvertraging de knipperfrequentie beïnvloeden.
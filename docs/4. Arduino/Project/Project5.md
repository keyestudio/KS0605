# Project 5 Ultrasone Sensor

**Beschrijving**

![](media/image-20250908154003868.png)

De HC-SR04 ultrasone sensor gebruikt sonar om de afstand tot een object te bepalen, net zoals vleermuizen dat doen. Het biedt uitstekende contactloze afstandsdetectie met hoge nauwkeurigheid en stabiele metingen in een gebruiksvriendelijk pakket. Het komt compleet met ultrasone zender- en ontvanger modules.

De HC-SR04 of ultrasone sensor wordt gebruikt in een breed scala van elektronicaprojecten voor het creëren van obstakeldetectie en afstandsmeting toepassingen, evenals verschillende andere toepassingen. Hier presenteren we de eenvoudige methode om de afstand met Arduino en ultrasone sensor te meten en hoe de ultrasone sensor met Arduino te gebruiken.

**Specificatie**

![](media/image-20250908154036832.png)

- Voedingsspanning: +5V DC
- Rusttroom: <2mA
- Werkstroom: 15mA
- Effectieve hoek: <15°
- Meetbereik: 2cm – 400 cm
- Resolutie: 0,3 cm
- Meethoek: 30 graden
- Trigger ingangspulsbreedte: 10μs

**Componenten**

![](media/image-20250908154147825.png)

**Het principe van de ultrasone sensor**

Zoals in de bovenstaande afbeelding te zien is, werkt het als twee ogen. De ene is het zendgedeelte, de ander is het ontvanggedeelte.

De ultrasone module zendt ultrasone golven uit na het ontvangen van een triggersignaal. Wanneer de ultrasone golven het object tegenkomen en terugkaatsen, geeft de module een echosignaal af, zodat het de afstand van het object kan bepalen op basis van het tijdsverschil tussen het triggersignaal en het echosignaal.

De t is de tijd dat het uitgezonden signaal een obstakel tegenkomt en terugkeert. De voortplantingssnelheid van geluid in de lucht is ongeveer 343m/s, en afstand = snelheid × tijd. Echter, de ultrasone golf wordt uitgezonden en komt terug, wat 2 keer de afstand is. Daarom moet het door 2 worden gedeeld. De afstand gemeten door ultrasone golf = (snelheid × tijd)/2.

1. Gebruiksmethode en timingdiagram van de ultrasone module:

2. Stel de vertragingstijd van de Trig-pin van SR04 in op minimaal 10μs, wat het kan triggeren om afstand te detecteren.
3. Na triggering zal de module automatisch acht 40KHz ultrasone pulsen verzenden en detecteren of er een signaal terugkomt. Deze stap wordt automatisch door de module voltooid.
4. Als het signaal terugkomt, geeft de Echo-pin een hoog niveau af, en de duur van het hoge niveau is de tijd van het verzenden van de ultrasone golf tot de terugkeer.

![](media/image-20250908154407063.png)

Schakelschema van de ultrasone sensor:

![](media/image-20250908154422828.png)

**Verbindingsdiagram**

![](media/image-20250908154455132.png)

Bedrading gids:

- Ultrasone sensor keyestudio V5 sensor shield
- VCC → 5v(V)
- Trig → 5(S)
- Echo → 4(S)
- Gnd → Gnd(G)

**Testcode**

```c
/*
keyestudio Mini Tank Robot V2.1
les 5
Ultrasone sensor
http://www.keyestudio.com
*/
int trigPin = 5; // Trigger
int echoPin = 4; // Echo
long duration, cm, inches;

void setup() 
{
    // Seriële poort starten
    Serial.begin (9600);
    // Ingangen en uitgangen definiëren
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);
}
void loop() 
{
    // De sensor wordt geactiveerd door een HIGH puls van 10 of meer microseconden.
    // Geef eerst een korte LOW puls om een schone HIGH puls te garanderen:
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    // Lees het signaal van de sensor: een HIGH puls waarvan de duur de tijd (in microseconden) is van het verzenden van de ping tot de ontvangst van de echo ervan van een object.
    duration = pulseIn(echoPin, HIGH);
    // Zet de tijd om in een afstand
    cm = (duration/2) / 29.1; // Deel door 29.1 of vermenigvuldig met 0.0343
    inches = (duration/2) / 74; // Deel door 74 of vermenigvuldig met 0.0135
    Serial.print(inches);
    Serial.print("in, ");
    Serial.print(cm);
    Serial.print("cm");
    Serial.println();
    delay(250);
}
```

**Testresultaat**

Upload de testcode op het ontwikkelingsbord, open de seriële monitor en stel de baudrate in op 9600. De gedetecteerde afstand wordt weergegeven, en de eenheid is cm en inch. Blokkeer de ultrasone sensor met uw hand, de weergegeven afstandswaarde wordt kleiner.

![](media/image-20250908154718663.png)

**Code Uitleg**

**int trigPin = 5;** deze pin is gedefinieerd om ultrasone golven uit te zenden, over het algemeen uitgang.

**int echoPin = 4;** dit is gedefinieerd als de ontvangstpin, over het algemeen ingang.

**cm = (duration/2) / 29.1; inches = (duration/2) / 74; met 0.0135**

We kunnen de afstand berekenen met behulp van de volgende formule:

afstand = (reistijd/2) × geluidssnelheid

De geluidssnelheid is: 343m/s = 0.0343 cm/μs = 1/29.1 cm/μs

Of in inches: 13503.9in/s = 0.0135in/μs = 1/74in/μs

We moeten de reistijd door 2 delen omdat we rekening moeten houden met het feit dat de golf werd verzonden, het object raakte en vervolgens terugkeerde naar de sensor.

**Uitbreidingsoefening:**

We hebben de afstand gemeten die door de ultrasone sensor wordt weergegeven. Hoe zit het met het besturen van de LED met de gemeten afstand? Laten we het proberen en een LED-lichtmodule op de D10-pin aansluiten.

![](media/image-20250908154848028.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 5
 Ultrasone LED
 http://www.keyestudio.com
*/ 
int trigPin = 5;    // Trigger
int echoPin = 4;    // Echo
long duration, cm, inches;

void setup() 
{
  // Seriële poort starten
  Serial.begin (9600);
  // Ingangen en uitgangen definiëren
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() 
{
  // De sensor wordt geactiveerd door een HIGH puls van 10 of meer microseconden.
  // Geef eerst een korte LOW puls om een schone HIGH puls te garanderen:
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Lees het signaal van de sensor: een HIGH puls waarvan de duur de tijd (in microseconden) is van het verzenden van de ping tot de ontvangst van de echo ervan van een object.
  duration = pulseIn(echoPin, HIGH);
  // Zet de tijd om in een afstand
  cm = (duration/2) / 29.1;     // Deel door 29.1 of vermenigvuldig met 0.0343
  inches = (duration/2) / 74;   // Deel door 74 of vermenigvuldig met 0.0135
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

Upload de testcode naar het ontwikkelingsbord en blokkeer de ultrasone sensor met uw hand, controleer vervolgens of de LED aan is.
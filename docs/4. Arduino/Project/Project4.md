# Project 4 Servo Besturing

![](media/image-20250908152354194.png)

**Beschrijving**

Een servomotor is een roterende actuator voor positiebesturing. Hij bestaat voornamelijk uit een behuizing, een printplaat, een kernloze motor, een tandwiel en een positiesensor. Het werkingsprincipe is dat de servo het signaal ontvangt dat door MCU's of ontvangers wordt verzonden en een referentiesignaal produceert met een periode van 20ms en een breedte van 1,5ms. Vervolgens vergelijkt hij de verkregen DC-offsetspanning met de spanning van de potentiometer en verkrijgt het spanningsverschil als uitvoer.

Wanneer de motorsnelheid constant is, wordt de potentiometer via het trapsgewijze reduceertandwiel aangedreven om te draaien, waardoor het spanningsverschil 0 wordt en de motor stopt met draaien. Over het algemeen is het hoekbereik van servomotation 0°\--180°.

De rotatiehoek van de servomotor wordt geregeld door de duty cycle van het PWM-signaal (Pulse-Width Modulation) aan te passen. De standaardcyclus van een PWM-signaal is 20ms (50Hz). Theoretisch ligt de breedte tussen 1ms en 2ms, maar in de praktijk is dit tussen 0,5ms en 2,5ms. De breedte komt overeen met de rotatiehoek van 0° tot 180°. Let op: voor motoren van verschillende merken kan hetzelfde signaal verschillende rotatiehoeken opleveren.

![](media/image-20250908152510007.png)

Over het algemeen heeft een servo drie draden in bruin, rood en oranje. De bruine draad is de aardingsdraad, de rode is de positieve pool en de oranje is de signaaldraad.

![](media/image-20250908152525491.png)

De bijbehorende servohoeken worden hieronder weergegeven:

![](media/image-20250908152558682.png)

**Specificaties**

- Werkspanning: DC 4,8V \~ 6V
- Werkhoekbereik: ongeveer 180° (bij 500 → 2500 μsec)
- Pulsbreedte bereik: 500 → 2500 μsec
- Onbelaste snelheid: 0,12 ± 0,01 sec / 60 (DC 4,8V) 0,1 ± 0,01 sec / 60 (DC 6V)
- Onbelaste stroom: 200 ± 20mA (DC 4,8V) 220 ± 20mA (DC 6V)
- Stoptorque: 1,3 ± 0,01kg · cm (DC 4,8V) 1,5 ± 0,1kg · cm (DC 6V)
- Stopstroom: ≦ 850mA (DC 4,8V) ≦ 1000mA (DC 6V)
- Stand-bystroom: 3 ± 1mA (DC 4,8V) 4 ± 1mA (DC 6V)

**Componenten**

![](media/image-20250908152809268.png)

**Aansluitschema：**

![](media/image-20250908152824930.png)**Bedrading opmerkingen:** de bruine draad van de servo is verbonden met Gnd(G), de rode draad is verbonden met 5v(V) en de oranje draad is verbonden met digitale pin 9.

De servo moet worden aangesloten op een externe voeding vanwege de hoge vraag naar servostroom. Over het algemeen is de stroom van het ontwikkelbord niet voldoende. Als er geen voeding is aangesloten, kan het ontwikkelbord beschadigd raken.

**Testcode 1**

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 4.1
Servo
http://www.keyestudio.com
*/
#define servoPin 9  //servo Pin
int pos; //hoekvariabele van servo
int pulsewidth; // pulsbreedte variabele van servo

void setup() 
{
  pinMode(servoPin, OUTPUT);  //stel servo pin in als OUTPUT
  procedure(0); //stel de hoek van de servo in op 0°
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1) // gaat van 0 graden naar 180 graden
  { 
    // in stappen van 1 graad
    procedure(pos);              // vertel servo om naar positie in variabele 'pos' te gaan
    delay(15);                   //regelt de rotatiesnelheid van de servo
  }
  for (pos = 180; pos >= 0; pos -= 1) // gaat van 180 graden naar 0 graden
  { 
    procedure(pos);              // vertel servo om naar positie in variabele 'pos' te gaan
    delay(15);                    
  }
}
// functie om servo te besturen
void procedure(int myangle) 
{
  pulsewidth = myangle * 11 + 500;  //bereken de waarde van de pulsbreedte
  digitalWrite(servoPin,HIGH);
  delayMicroseconds(pulsewidth);   //De duur van het hoge niveau is de pulsbreedte
  digitalWrite(servoPin,LOW);
  delay((20 - pulsewidth / 1000));  // de cyclus is 20ms, het lage niveau duurt de resterende tijd
}
```

Na het succesvol uploaden van de code, beweegt de servo heen en weer in het bereik van 0° tot 180°.

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 4.2
 servo
 http://www.keyestudio.com
*/
#include <Servo.h>
Servo myservo;  // maak servo-object aan om een servo te besturen
// op de meeste boards kunnen twaalf servo-objecten worden aangemaakt
int pos = 0;    // variabele om de servopositie op te slaan

void setup() 
{
  myservo.attach(9);  // koppelt de servo op pin 9 aan het servo-object
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1)  // gaat van 0 graden naar 180 graden
  {
    // in stappen van 1 graad
    myservo.write(pos);              // vertel servo om naar positie in variabele 'pos' te gaan
    delay(15);                       // wacht 15ms totdat de servo de positie bereikt
  }
  for (pos = 180; pos >= 0; pos -= 1) { // gaat van 180 graden naar 0 graden
    myservo.write(pos);              // vertel servo om naar positie in variabele 'pos' te gaan
    delay(15);                       // wacht 15ms totdat de servo de positie bereikt
  }
}
```

**Testresultaat**

Na het succesvol uploaden van de code en het inschakelen van de voeding, beweegt de servo heen en weer in het bereik van 0° tot 180°.

Het resultaat is hetzelfde. Normaal gesproken besturen we de servo via een bibliotheekbestand.

**Code Uitleg**

Arduino wordt geleverd met **\#include \<Servo.h\>** (servofunctie en instructies)

Hieronder staan enkele veelgebruikte instructies van de servofunctie:

1. attach（interface）——Stel de servo-interface in, poort 9 en 10 zijn beschikbaar.
2. write（angle）——De instructie om de rotatiehoek van de servo in te stellen.
3. read（）——De instructie om de hoek van de servo te lezen; leest de commandowaarde van "write()".
4. **Let op:** Het bovenstaande schrijfformaat is "servo variabelenaam, specifieke instructie（）", bijvoorbeeld: myservo.attach(9).
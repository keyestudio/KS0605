# Project 2 LED-helderheid aanpassen

**(1) Beschrijving**

In de vorige les hebben we de LED in- en uitgeschakeld en laten knipperen.

In dit project zullen we de helderheid van de LED regelen via PWM om ademhalingseffecten te simuleren. Op dezelfde manier kunt u de stapgrootte en vertragingstijd in de code wijzigen om verschillende ademhalingseffecten aan te tonen.

PWM is een manier om analoge uitvoer via digitale middelen te regelen. Digitale besturing wordt gebruikt om vierkantgolven met verschillende duty cycles (een signaal dat constant tussen hoog en laag niveau schakelt) te genereren om de analoge uitvoer te regelen. Over het algemeen zijn de ingangsspanningen van poorten 0V en 5V. Wat als 3V vereist is? Of een keuze tussen 1V, 3V en 3,5V? We kunnen niet constant weerstanden veranderen. Daarom gebruiken we PWM.

![](./media/bbcfcb9ae56abb7e80ee587246fc4be9.GIF)

Voor de Arduino digitale poortspanningsuitvoer zijn er slechts twee toestanden: LOW en HIGH, die overeenkomen met spanningsuitvoeren van 0V en 5V. U kunt LOW als 0 en HIGH als 1 definiëren, en de Arduino vijfhonderd 0- of 1-signalen binnen 1 seconde laten uitvoeren.

Als u vijfhonderd 1-signalen uitvoert, dat is 5V; als dit allemaal 1 is, dat is 0V. Als u op deze manier 010101010101 uitvoert, is de uitvoerpoort 2,5V, wat lijkt op het weergeven van een film. De films die we kijken zijn niet volledig continu. Het voert eigenlijk 25 beelden per seconde uit. In dit geval kan de mens het niet onderscheiden, en PWM ook niet. Als u een ander voltage wilt, moet u de verhouding van 0- en 1-signalen regelen. Hoe meer 0- en 1-signalen per tijdseenheid worden uitgevoerd, hoe nauwkeuriger de regeling.

**(2) Specificatie**

- Besturingsinterface: digitale poort
- Werkingsspanning: DC 3,3-5V
- Pinafstand: 2,54 mm
- Weergavekleur: rood

**(3) Componenten**

![](./media/image-20250902170952089.png)

 **(4) Verbindingsschema**

![](./media/image-20250902171013917.png)

 **(5) Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 2.2
 pwm-langzaam
 http://www.keyestudio.com
*/
int ledPin = 10; // Definieer de LED-pin op D10
int value;

void setup () 
{
	pinMode (ledPin, OUTPUT); // initialiseer ledpin als uitvoer.
}

void loop () 
{
    for (value = 0; value <255; value = value + 1)
    {
        analogWrite (ledPin, value); // LED licht geleidelijk op
        delay (30); // vertraging 30MS
    }
    for (value = 255; value> 0; value = value-1)
    {
        analogWrite (ledPin, value); // LED gaat geleidelijk uit
        delay (30); // vertraging 30MS
	}
}
```

**Testresultaat**

Na het succesvol uploaden van de testcode verandert de LED geleidelijk van helder naar donker, zoals de ademhaling van een mens, in plaats van onmiddellijk in- of uit te schakelen.

**Codeverklaring**

Wanneer we bepaalde instructies moeten herhalen, kunnen we de FOR-instructie gebruiken.

De FORMAT van de FOR-instructie wordt hieronder weergegeven:

![](./media/image-20250902171421873.png)

OF cyclische volgorde:

Ronde 1：1 → 2 → 3 → 4

Ronde 2：2 → 3 → 4

…

Tot nummer 2 niet meer waar is, is de "for"-lus voorbij.

Na deze volgorde te begrijpen, gaan we terug naar de code:

**for (int value = 0; value < 255; value=value+1){**

**...**

**}**

**for (int value = 255; value >0; value=value-1){**

**...**

**}**

De twee "for"-instructies zorgen ervoor dat value van 0 naar 255 toeneemt, vervolgens van 255 naar 0 afneemt, vervolgens naar 255 toeneemt, .... oneindige lus.

Er is een nieuwe functie in het volgende ----- analogWrite()

We weten dat de digitale poort slechts twee toestanden heeft: 0 en 1. Hoe sturen we een analoge waarde naar een digitale waarde? Hier is deze functie nodig. Laten we het Arduino-bord observeren en de 6 pinnen vinden die zijn gemarkeerd met "\~" en PWM-signalen kunnen uitvoeren.

**Functie-indeling als volgt:**

**analogWrite(pin,value)**

analogWrite() wordt gebruikt om een analoge waarde van 0\~255 naar de PWM-poort te schrijven, dus de waarde ligt in het bereik van 0\~255. Let op dat u alleen de digitale pinnen met PWM-functie schrijft, zoals pin 3, 5, 6, 9, 10, 11.

PWM is een technologie om analoge grootheden via digitale methoden te verkrijgen. Digitale besturing vormt een vierkantgolf, en het vierkantgolfsignaal heeft slechts twee toestanden: aan en uit (dat wil zeggen, hoog of laag niveau). Door de verhouding van de duur van aan en uit te regelen, kan een spanning van 0 tot 5V worden gesimuleerd. De tijd dat het aan staat (academisch gezegd hoog niveau) wordt pulsbreedte genoemd, dus PWM wordt ook pulsbreedte-modulatie genoemd.

Via de volgende vijf vierkantgolven krijgen we meer inzicht in PWM.

![](./media/image-20250902172304373.png)

In de bovenstaande afbeelding vertegenwoordigt de groene lijn een periode, en de waarde van analogWrite() komt overeen met een percentage dat ook Duty Cycle wordt genoemd.

Duty cycle betekent dat de duur van het hoge niveau wordt gedeeld door de duur van het lage niveau in een cyclus. Van boven naar beneden is de duty cycle van de eerste vierkantgolf 0% en de bijbehorende waarde is 0. De LED-helderheid is het laagst, dat wil zeggen uitgeschakeld. Hoe langer het hoge niveau duurt, hoe helderder de LED. Daarom is de laatste duty cycle 100%, wat overeenkomt met 255, en de LED is het helderst. 25% betekent donkerder.

PWM wordt meestal gebruikt voor het aanpassen van de LED-helderheid of de rotatiesnelheid van een motor.

Het speelt een vitale rol bij het besturen van slimme robotauto's. Ik geloof dat u niet kunt wachten om het volgende project in te gaan.
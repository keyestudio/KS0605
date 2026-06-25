# Project 3 Fotoresistor Sensor

![](./media/image-20250902173047302.png)

 **Beschrijving**

De fotoresistor is een speciale weerstand gemaakt van halfgeleidermaterialen zoals CdS of Selenide septum. Het oppervlak is ook bedekt met vochtbestendige hars, die een fotogeleidend effect heeft. Het is gevoelig voor omgevingslicht. De weerstand varieert afhankelijk van verschillende lichtintensiteiten.

We gebruiken de eigenschappen van de fotoresistor om het circuit te ontwerpen en de fotoresistor module te genereren.

Als u de signaalpin van de fotocellmodule aansluit op een analoge poort, zult u merken dat hoe sterker de lichtintensiteit, hoe groter de spanning van de analoge poort, en hoe groter de analoge waarde.

Omgekeerd, hoe zwakker de lichtintensiteit, hoe kleiner de spanning van de analoge poort, hoe kleiner de analoge waarde is.

Op basis daarvan kunnen we de fotocellmodule gebruiken om de analoge waarde uit te lezen en zo de omgevingslichtintensiteit bepalen.

 **Specificatie**

![](./media/image-20250902173349950.png)\

- Weerstand：5K ohm-0.5Mohm
- Interfacetype: analoog
- Werkspanning: 3.3V-5V
- Gemakkelijke installatie: met schroefbevestigingsgaten
- Pinafstand: 2.54mm

 **Componenten**

![](./media/image-20250902173528860.png)

 **Verbindingsschema：**

![](./media/image-20250902173558747.png)

De twee fotoresistorsensoren zijn verbonden met A1 en A2, voltooi vervolgens het experiment via fotoresistor verbonden met A1. Laten we de analoge waarde ervan uitlezen.

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 3.1
 fotocell
 http://www.keyestudio.com
*/

int sensorPin = A1;    // selecteer de ingangspin voor de fotocell
int sensorValue = 0;  // variabele om de waarde van de sensor op te slaan
void setup() 
{
	Serial.begin(9600);
}

void loop() 
{
    sensorValue = analogRead(sensorPin);  // lees de waarde van de sensor:
    Serial.println(sensorValue);  // seriële poort drukt de weerstandswaarde af
    delay(500);
}
//******************************************************
```

 **Testresultaat**

Upload de code op het ontwikkelingsbord, open de seriële monitor en controleer of de waarde afneemt wanneer u de fotoresistor bedekt. Echter, de waarde neemt toe wanneer deze niet bedekt is.

![](./media/image-20250902174159923.png)

**Code-uitleg**

**analogRead(sensorPin)：** lees de analoge waarde van de fotoresistor via analoge poorten.

**Serial.begin(9600):** initialiseer de seriële poort, de baudrate van seriële communicatie is 9600.

**Serial.println** : seriële poort drukt af en voert regelomslag uit.

**Uitbreidingsoefening**

We weten nu hoe we de waarde van de fotoresistor kunnen uitlezen. Laten we de fotoresistor combineren met een LED en de status van de LED bekijken.

![](./media/image-20250902174256941.png)

PWM beperkt de helderheid, dus LED is verbonden met PWM-pinnen. Sluit LED aan op pin 10, houd de pin van de fotoresistor ongewijzigd, ontwerp vervolgens de code:

```c
/*keyestudio Mini Tank Robot V2.1
les 3.2
fotocell-analoge uitgang
http://www.keyestudio.com
*/
int analogInPin = A1;  // Analoge ingangspin waaraan de fotocell is aangesloten
int analogOutPin = 10; // Analoge uitgangspin waaraan de LED is aangesloten
int sensorValue = 0;        // waarde gelezen van de pot
int outputValue = 0;        // waarde uitgevoerd naar de PWM (analoge uitgang)

void setup() 
{
	Serial.begin(9600);
 }
void loop() 
{
  // lees de analoge ingangswaarde:
  sensorValue = analogRead(analogInPin);
  // map het naar het bereik van de analoge uitgang:
  outputValue = map(sensorValue, 0, 1023, 0, 255);
  // wijzig de analoge uitgangswaarde:
  analogWrite(analogOutPin, outputValue);
  // wacht 2 milliseconden voordat de volgende lus voor de analoog-naar-digitaal
  // converter zich stabiliseert na de laatste meting:
 Serial.println(sensorValue);  // seriële poort drukt de waarde van de fotoresistor af
 delay(2);
}
//***************************************************************
```

Upload de code en druk er met uw hand op om de LED-helderheid waar te nemen.
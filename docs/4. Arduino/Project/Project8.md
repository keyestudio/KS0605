# Project 8 Motorbesturing en Snelheidsregeling

**Beschrijving**

![](media/image-20250908162844748.png)

Er zijn veel manieren om een motor aan te sturen. Onze robotauto gebruikt de meest voorkomende oplossing--L298P--een uitstekende krachtige motorbesturings-IC van STMicroelectronics. Deze kan gelijkstroommotoren, twee-fase en vier-fase stapelmotoren rechtstreeks aansturen. De aandrijfstroom bedraagt tot 2A, en de uitgangsaansluiting van de motor is beveiligd met acht snelle Schottky-diodes.

We hebben een shield ontworpen op basis van het L298P-circuit. Het gestapelde ontwerp vermindert de technische moeilijkheid van het gebruik en de besturing van de motor.

**Specificatie**

Schakelschema voor L298P-board

![](media/image-20250908163017604.png)

1. Logisch deel ingansspanning: DC5V
2. Aandrijfdeel ingansspanning: DC 7-12V
3. Logisch deel werkstroom: \<36mA
4. Aandrijfdeel werkstroom: \<2A
5. Maximaal vermogensverlies: 25W (T=75℃)
6. Werktemperatuur: -25℃～＋130℃
7. Besturingssignaal ingangsniveau: hoog niveau 2.3V\<Vin\<5V, laag niveau\0.3V\<Vin\<1.5V

![](media/image-20250908163151925.png)

**Robot Laten Bewegen**

Via het bovenstaande schakelschema is de richtingspincode van motor A D12, en de snelheidspincode is D3; D13 is de richtingspincode van motor B, D11 is de snelheidspincode.

We weten hoe we digitale poorten kunnen besturen volgens het volgende schema.

PWM zorgt ervoor dat 2 motoren inschakelen om de robotauto aan te drijven. De PWM-waarde ligt in het bereik van 0-255. Hoe groter het getal, hoe sneller de motor draait.

| Tankrobot       | Motor (A)           | Motor (B)           |
| --------------- | ------------------- | ------------------- |
| Vooruit         | Rechtsom draaien    |                     |
| Achteruit       | Linksom draaien     |                     |
| Naar links      | Linksom draaien     | Rechtsom draaien    |
| Naar rechts     | Rechtsom draaien    | Linksom draaien     |
| Stop            | Stop                | Stop                |

**Onderdelen**

![](media/image-20250908163739200.png)

**Verbindingsschema**

![](media/d35ffe6c0c275548f40bcafb42a93da1.jpeg)

**Opmerking:** het 4-pins terminalblok is gemarkeerd met zijdedruk 1234. De rode draad van de rechterachtermotor is verbonden met aansluiting 1, zwarte draad is verbonden met aansluiting 2. De rode draad van de linkervoorkantmotor is verbonden met aansluiting 3, zwarte draad is verbonden met aansluiting 4.

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 8.1
 motorbesturing
 http://www.keyestudio.com
*/ 

#define ML_Ctrl 13  //definieer de richtingsbesturingspincode van de linkermotor
#define ML_PWM 11   //definieer de PWM-besturingspincode van de linkermotor
#define MR_Ctrl 12  //definieer de richtingsbesturingspincode van de rechtermotor
#define MR_PWM 3   //definieer de PWM-besturingspincode van de rechtermotor

void setup()
{
  pinMode(ML_Ctrl, OUTPUT);//definieer de richtingsbesturingspincode van de linkermotor als uitvoer
  pinMode(ML_PWM, OUTPUT);//definieer de PWM-besturingspincode van de linkermotor als uitvoer
  pinMode(MR_Ctrl, OUTPUT);//definieer de richtingsbesturingspincode van de rechtermotor als uitvoer
  pinMode(MR_PWM, OUTPUT);//definieer de PWM-besturingspincode van de rechtermotor als uitvoer
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//stel de richtingsbesturingspincode van de linkermotor in op LAAG
  analogWrite(ML_PWM,200);//stel de PWM-besturingsnelheid van de linkermotor in op 200
  digitalWrite(MR_Ctrl,LOW);//stel de richtingsbesturingspincode van de rechtermotor in op LAAG
  analogWrite(MR_PWM,200);//stel de PWM-besturingsnelheid van de rechtermotor in op 200

  //vooruit
  delay(2000);//vertraging van 2s
   digitalWrite(ML_Ctrl,HIGH);//stel de richtingsbesturingspincode van de linkermotor in op HOOG
  analogWrite(ML_PWM,200);//stel de PWM-besturingsnelheid van de linkermotor in op 200  
digitalWrite(MR_Ctrl,HIGH);//stel de richtingsbesturingspincode van de rechtermotor in op HOOG
  analogWrite(MR_PWM,200);//stel de PWM-besturingsnelheid van de rechtermotor in op 200

   //achteruit
  delay(2000);//vertraging van 2s 
  digitalWrite(ML_Ctrl,HIGH);//stel de richtingsbesturingspincode van de linkermotor in op HOOG
  analogWrite(ML_PWM,200);//stel de PWM-besturingsnelheid van de linkermotor in op 200
  digitalWrite(MR_Ctrl,LOW);//stel de richtingsbesturingspincode van de rechtermotor in op LAAG
  analogWrite(MR_PWM,200);//stel de PWM-besturingsnelheid van de rechtermotor in op 200

    //links
  delay(2000);//vertraging van 2s
   digitalWrite(ML_Ctrl,LOW);//stel de richtingsbesturingspincode van de linkermotor in op LAAG
  analogWrite(ML_PWM,200);//stel de PWM-besturingsnelheid van de linkermotor in op 200
  digitalWrite(MR_Ctrl,HIGH);//stel de richtingsbesturingspincode van de rechtermotor in op HOOG
  analogWrite(MR_PWM,200);//stel de PWM-besturingsnelheid van de rechtermotor in op 200

   //rechts
  delay(2000);//vertraging van 2s
  analogWrite(ML_PWM,0);//stel de PWM-besturingsnelheid van de linkermotor in op 0
  analogWrite(MR_PWM,0);//stel de PWM-besturingsnelheid van de rechtermotor in op 0

    //stop
  delay(2000);//vertraging van 2s
}//*****************************************
```

**Testresultaat**

Maak verbinding volgens het verbindingsschema, upload de code en schakel in. De slimme auto gaat 2s vooruit en achteruit, draait 2s naar links en rechts, stopt 2s en herhaalt dit afwisselend.

**Codeuitleg**

**digitalWrite(ML_Ctrl,LOW):** de draairichting van de motor wordt bepaald door het hoog/laag niveau en de pinnen die de draairichting bepalen zijn digitale pinnen.

**analogWrite(ML_PWM,200):** de snelheid van de motor wordt geregeld door PWM, en de pinnen die de snelheid van de motor bepalen moeten PWM-pinnen zijn.

**Uitbreidingsoefening**

Pas de snelheid aan die PWM de motor bestuurt, en maak verbinding op dezelfde manier.

![](media/d35ffe6c0c275548f40bcafb42a93da1-1757321401643-2.jpeg)

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 8.2
 motorbesturing pwm
 http://www.keyestudio.com
*/ 
#define ML_Ctrl 13  //definieer de richtingsbesturingspincode van de linkermotor
#define ML_PWM 11   //definieer de PWM-besturingspincode van de linkermotor
#define MR_Ctrl 12  //definieer de richtingsbesturingspincode van de rechtermotor
#define MR_PWM 3   //definieer de PWM-besturingspincode van de rechtermotor

void setup()
{ 
  pinMode(ML_Ctrl, OUTPUT);//definieer de richtingsbesturingspincode van de linkermotor als UITVOER
  pinMode(ML_PWM, OUTPUT);//definieer de PWM-besturingspincode van de linkermotor als UITVOER
  pinMode(MR_Ctrl, OUTPUT);//definieer de richtingsbesturingspincode van de rechtermotor als UITVOER
  pinMode(MR_PWM, OUTPUT);//definieer de PWM-besturingspincode van de rechtermotor als UITVOER
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//stel de richtingsbesturingspincode van de linkermotor in op LAAG
  analogWrite(ML_PWM,100);//stel de PWM-besturingsnelheid van de linkermotor in op 100
  digitalWrite(MR_Ctrl,LOW);//stel de richtingsbesturingspincode van de rechtermotor in op LAAG
  analogWrite(MR_PWM,100);//stel de PWM-besturingsnelheid van de rechtermotor in op 100
  //vooruit
  delay(2000);//definieer 2s
  digitalWrite(ML_Ctrl,HIGH);//stel de richtingsbesturingspincode van de linkermotor in op HOOG
  analogWrite(ML_PWM,250);//stel de PWM-besturingsnelheid van de linkermotor in op 100
  digitalWrite(MR_Ctrl,HIGH);//stel de richtingsbesturingspincode van de rechtermotor in op HOOG
  analogWrite(MR_PWM,250);//stel de PWM-besturingsnelheid van de rechtermotor in op 100
   //achteruit
  delay(2000);//definieer 2s
  digitalWrite(ML_Ctrl,HIGH);//stel de richtingsbesturingspincode van de linkermotor in op HOOG
  analogWrite(ML_PWM,250);//stel de PWM-besturingsnelheid van de linkermotor in op 100
  digitalWrite(MR_Ctrl,LOW);//stel de richtingsbesturingspincode van de rechtermotor in op LAAG
  analogWrite(MR_PWM,250);//stel de PWM-besturingsnelheid van de rechtermotor in op 100
    //links
  delay(2000);//definieer 2s
   digitalWrite(ML_Ctrl,LOW);//stel de richtingsbesturingspincode van de linkermotor in op LAAG
  analogWrite(ML_PWM,250);//stel de PWM-besturingsnelheid van de linkermotor in op 200
  digitalWrite(MR_Ctrl,HIGH);//stel de richtingsbesturingspincode van de rechtermotor in op HOOG
  analogWrite(MR_PWM,250);//stel de PWM-besturingsnelheid van de rechtermotor in op 100
   //rechts
  delay(2000);//definieer 2s
  analogWrite(ML_PWM,0);//stel de PWM-besturingsnelheid van de linkermotor in op 0
  analogWrite(MR_PWM,0);//stel de PWM-besturingsnelheid van de rechtermotor in op 0

    //stop
  delay(2000);//definieer 2s
}//******************************************************************
```

Code succesvol geüpload, de motoren draaien sneller.
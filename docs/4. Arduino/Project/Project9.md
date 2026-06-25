# Project 9 8*16 LED Board

**Beschrijving**

![](media/image-20250908165452592.png)

Als u een 8\*16 LED-bord aan de robot toevoegt, zal het geweldig zijn. De 8\*16 dot matrix van Keyestudio kan aan deze vereiste voldoen. Hiermee kunt u zelf gezichtsuitdrukkingen, patronen of andere interessante displays maken. Dit 8\*16 LED-lichtkbord is voorzien van 128 LED's. De gegevens van de microprocessor (Arduino) communiceren met de AiP1640 via de twee-draads businterface, zodat de 128 LED's op de module kunnen worden bestuurd, die de patronen die u nodig hebt op de dot matrix produceren. Voor gemakkelijke bedrading is een HX-2.54 4-pins bedrading voorzien.

**Specificatie**

- Werkspanning: DC 3,3-5V
- Stroomverbruik: 400mW
- Oscillatiefrequentie: 450KHz
- Stuurstroom: 200mA
- Werktemperatuur: -40\~80℃
- Communicatiemethode: twee-draads bus

**Componenten**

![](media/image-20250908165719665.png)

**8*16 Dot Matrix Display**

Schakelschema

![](media/image-20250908165746529.png)

**Het principe van 8\*16 dot matrix:**

Hoe kunt u elke LED van de 8\*16 dot matrix besturen? We weten dat een byte 8 bits heeft, en elke bit is 0 of 1. Wanneer een bit 0 is, schakelt u de LED uit en wanneer een bit 1 is, schakelt u de LED in. Daardoor kan één byte de LED in één rij van de dot matrix besturen, dus 16 bytes kunnen 16 kolommen LED-lichten besturen, dat wil zeggen een 8\*16 dot matrix.

**Interfacebeschrijving en communicatieprotocol:**

De gegevens van de microprocessor (Arduino) communiceren met de AiP1640 via de twee-draads businterface.

Het communicatieprotocoldiagram wordt hieronder weergegeven:

(SCLK) is SCL, (DIN) is SDA:

![](media/image-20250908165823763.png)

①De startvoorwaarde voor gegevensinvoer: SCL is hoog niveau en SDA verandert van hoog naar laag.

②Voor gegevenscommandoinstelling zijn er methoden zoals weergegeven in de onderstaande afbeelding:

In ons voorbeeldprogramma selecteren we de manier om **1 aan het adres automatisch toe te voegen**, de binaire waarde is 0100 0000 en de overeenkomstige hexadecimale waarde is 0x40.

![](media/image-20250908165925549.png)

③Voor adrescommandoinstelling kan het adres als volgt worden geselecteerd.

Het eerste 00H is in ons voorbeeldprogramma geselecteerd, en het binaire getal 1100 0000 komt overeen met hexadecimaal 0xc0.

![](media/image-20250908165938702.png)

④De vereiste voor gegevensinvoer is dat SCL hoog niveau is bij het invoeren van gegevens, en het signaal op SDA moet ongewijzigd blijven. Alleen wanneer het kloksignaal op SCL laag niveau is, kan het signaal op SDA worden gewijzigd. De gegevensinvoer is eerst laag-order, daarna hoog-order.

⑤ De voorwaarde om gegevenstransmissie te beëindigen is dat wanneer SCL laag is, SDA laag is, en wanneer SCL hoog is, het SDA-niveau ook hoog wordt.

⑥ Weergavebesturing, stel verschillende pulsbreedten in, de pulsbreedten kunnen als volgt worden geselecteerd.

In dit voorbeeld kiezen we pulsbreedten 4/16, en de hexadecimale waarde komt overeen met 1000 1010 is 0x8A.

![](media/image-20250908170005446.png)

4\. Inleiding voor Modulus Tool

De online versie van dot matrix modulus tool:

[http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)

①Open links om de volgende pagina in te voeren.

![](media/image-20250908170027144.png)

②De dot matrix is 8\*16 in dit project, dus stel de hoogte in op 8, breedte op 16, zoals hieronder weergegeven.

![](media/image-20250908170040925.png)

③ Genereer hexadecimale gegevens uit het patroon

Zoals hieronder weergegeven, druk op de linkermuisknop om te selecteren, de rechterknop om te annuleren, teken het gewenste patroon, klik op **Generate**, en de hexadecimale gegevens die we nodig hebben, worden gegenereerd.

![](media/image-20250908170144895.png)

**Verbindingsschema**

![](media/image-20250908170158402.png)

Bedradingsopmerkingen: De GND, VCC, SDA en SCL van het 8x16 LED-paneel zijn respectievelijk verbonden met -(GND), + (VCC), A4 en A5 van het keyestudio sensoruitbreidingsbord voor twee-draads seriële communicatie. (Opmerking: Deze pin is verbonden met Arduino IIC, maar deze module is geen IIC-communicatie. Deze kan met elke twee pinnen worden verbonden.)

**Testcode**

De code die een glimlachend gezicht toont.

```c
/*
 keyestudio Mini Tank Robot V2.1
 les 9.1
 Matrix gezicht
 http://www.keyestudio.com
*/ 
// de gegevens van glimlach van modulus tool
unsigned char smile[] = {0x00, 0x00, 0x1c, 0x02, 0x02, 0x02, 0x5c, 0x40, 0x40, 0x5c, 0x02, 0x02, 0x02, 0x1c, 0x00, 0x00};

#define SCL_Pin  A5  // Stel klokpin in op A5
#define SDA_Pin  A4  // Stel gegevenspin in op A4

void setup()
{
  // Stel pin in op uitvoer
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // wis de weergave
  // matrix_display(clear);
}

void loop()
{
  matrix_display(smile);  // glimlachend gezicht weergeven
}
// de functie voor dot matrix weergave

void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // gebruik de functie van de startvoorwaarde voor gegevenstransmissie
  IIC_send(0xc0);  // selecteer adres
  
  for(int i = 0;i < 16;i++) // patroongegevens hebben 16 bits
  {
     IIC_send(matrix_value[i]); // draag de patroongegevens over
  }

  IIC_end();   // beëindig de transmissie van patroongegevens  
  IIC_start();
  IIC_send(0x8A);  // weergavebesturing, stel pulsbreedten in op 4/16 s
  IIC_end();
}

// de voorwaarde om gegevenstransmissie te starten
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Draag gegevens over
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Elke byte heeft 8 bits 8bit voor elk teken
  {
      digitalWrite(SCL_Pin,LOW);  // trek klokpin SCL_Pin omlaag om het signaal van SDA te wijzigen
      delayMicroseconds(3);
      if(send_data & 0x01)  // stel hoog en laag niveau van SDA_Pin in volgens 1 of 0 van elke bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // trek klokpin SCL_Pin omhoog om transmissie te stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  // detecteer bit voor bit, verschuif de gegevens naar rechts met één
  }
}

// Het teken van beëindiging van gegevenstransmissie
void IIC_end()
{
  digitalWrite(SCL_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
}
//******************************************************
```

**Testresultaat**

Bedraad volgens verbindingsschema. De DIP-schakelaar wordt naar het rechtereinde gedraaid en ingeschakeld, het glimlachende gezicht verschijnt op de dot matrix.

![](media/image-20250908170421220.png)

**Uitbreidingsoefening**

We gebruiken de modulo tool ([http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)) om de dot matrix afwisselend voor, stop en duidelijke patronen weer te geven, en het tijdsinterval is 2000 milliseconden.

![](media/image-20250908170445861.png)

![](media/image-20250908170452897.png)

![](media/image-20250908170459213.png)

Verkrijg de grafische code die via modulus tool moet worden weergegeven.

**Start：** 0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01

**Ga vooruit：** 0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Ga achteruit：** 0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Draai links：** 0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00

**Draai rechts：** 0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00

**Stop：** 0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00

**Wis de weergavecode**：0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00

De code die meerdere patronen verschuift:

```c
/* keyestudio Mini Tank Robot V2.1
 les 9.2
 Matrix lus
 http://www.keyestudio.com
*/ 
// Array, gebruikt om de gegevens van het patroon op te slaan, kan zelf worden berekend of verkregen van de modulus tool
unsigned char start01[] = 
{0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = 
{0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = 
{0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Stel klokpin in op A5
#define SDA_Pin  A4  // Stel gegevenspin in op A4
void setup(){
  // Stel pinnen in op uitvoer
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // Wis de weergave
  matrix_display(clear);
}
void loop(){
  matrix_display(start01);  // Startpatroon weergeven
  delay(2000);
  matrix_display(front);    // Voorpatroon
  delay(2000);
  matrix_display(STOP01);   // Stoppatroon
  delay(2000);
  matrix_display(clear);    // Wis de weergave Wis het scherm
  delay(2000);
}
// Deze functie wordt gebruikt voor weergave van dot matrix
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // roep de functie aan die gegevenstransmissie start  
  IIC_send(0xc0);  // Kies adres
  
  for(int i = 0;i < 16;i++) // patroongegevens hebben 16 bits
  {
     IIC_send(matrix_value[i]); // gegevens om patronen over te dragen 
  }
  IIC_end();   // beëindig de transmissie van patroongegevensBeeindigen
  IIC_start();
  IIC_send(0x8A);  // weergavebesturing, stel pulsbreedten in op 4/16
  IIC_end();
}
// De voorwaarde om gegevenstransmissie te starten
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Draag gegevens over
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Elke byte heeft 8 bits
  {
      digitalWrite(SCL_Pin,LOW);  // trek klokpin SCL omlaag om de signalen van SDA te wijzigen      
delayMicroseconds(3);
      if(send_data & 0x01)  // stel hoog en laag niveau van SDA_Pin in volgens 1 of 0 van elke bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // trek klokpin SCL_Pin omhoog om gegevenstransmissie te stoppen
      delayMicroseconds(3);
      send_data = send_data >> 1;  // detecteer bit voor bit, dus verschuif de gegevens naar rechts met één
  }}
// Het teken dat gegevenstransmissie eindigt
void IIC_end()
{
  digitalWrite(SCL_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);} 
//*****************************************************
```

Upload code op het ontwikkelingsbord, 8\*16 dot matrix geeft voor-, achter- en stoppatronen afwisselend weer.

![](media/image-20250908170902116.png)

![](media/image-20250908170913925.png)

![](media/image-20250908170921862.png)
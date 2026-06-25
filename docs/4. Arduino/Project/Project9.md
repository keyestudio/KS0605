# Progetto 9 Pannello LED 8*16

**Descrizione**

![](media/image-20250908165452592.png)

Se aggiungi un pannello LED 8\*16 al robot, sarà straordinario. La matrice di punti 8\*16 di Keyestudio può soddisfare questo requisito. Grazie ad essa, puoi creare emoticon facciali, motivi o altri display interessanti tutto da solo. Questo pannello luminoso LED 8\*16 è dotato di 128 LED. I dati del microprocessore (Arduino) comunicano con l'AiP1640 attraverso l'interfaccia del bus a due fili, in modo da controllare i 128 LED sul modulo, che producono i motivi di cui hai bisogno sulla matrice di punti. Per facilitare il cablaggio, è fornito un cablaggio HX-2.54 a 4 pin.

**Specifiche**

- Tensione di lavoro: CC 3,3-5V
- Perdita di potenza: 400mW
- Frequenza di oscillazione: 450KHz
- Corrente di azionamento: 200mA
- Temperatura di lavoro: -40\~80℃
- Metodo di comunicazione: bus a due fili

**Componenti**

![](media/image-20250908165719665.png)

**Display a matrice di punti 8*16**

Diagramma del circuito

![](media/image-20250908165746529.png)

**Il principio della matrice di punti 8\*16:**

Come controllare ogni LED della matrice di punti 8\*16? Sappiamo che un byte ha 8 bit, e ogni bit è 0 o 1. Quando un bit è 0, il LED si spegne e quando un bit è 1, il LED si accende. Pertanto, un byte può controllare il LED in una riga della matrice di punti, quindi 16 byte possono controllare 16 colonne di LED, cioè una matrice di punti 8\*16.

**Descrizione dell'interfaccia e protocollo di comunicazione:**

I dati del microprocessore (Arduino) comunicano con l'AiP1640 attraverso l'interfaccia del bus a due fili.

Il diagramma del protocollo di comunicazione è mostrato di seguito:

(SCLK) è SCL, (DIN) è SDA:

![](media/image-20250908165823763.png)

①La condizione di avvio per l'ingresso dei dati: SCL è a livello alto e SDA cambia da alto a basso.

②Per l'impostazione del comando dati, ci sono metodi come mostrato nella figura seguente:

Nel nostro programma di esempio, seleziona il modo di **aggiungere 1 all'indirizzo automaticamente**, il valore binario è 0100 0000 e il valore esadecimale corrispondente è 0x40.

![](media/image-20250908165925549.png)

③Per l'impostazione del comando di indirizzo, l'indirizzo può essere selezionato come mostrato di seguito.

Nel nostro programma di esempio è selezionato il primo 00H, e il numero binario 1100 0000 corrisponde all'esadecimale 0xc0.

![](media/image-20250908165938702.png)

④Il requisito per l'ingresso dei dati è che SCL sia a livello alto durante l'ingresso dei dati, e il segnale su SDA deve rimanere invariato. Solo quando il segnale di clock su SCL è a livello basso, il segnale su SDA può essere alterato. L'ingresso dei dati è prima l'ordine basso, poi l'ordine alto.

⑤ La condizione per terminare la trasmissione dei dati è che quando SCL è basso, SDA è basso, e quando SCL è alto, il livello di SDA diventa anche alto.

⑥ Controllo del display, impostare diverse larghezze di impulso, la larghezza di impulso può essere selezionata come mostrato di seguito.

In questo esempio, scegliamo la larghezza di impulso 4/16, e l'esadecimale corrispondente a 1000 1010 è 0x8A.

![](media/image-20250908170005446.png)

4\. Introduzione allo strumento Modulus

La versione online dello strumento modulus della matrice di punti:

[http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)

①Apri i link per accedere alla pagina seguente.

![](media/image-20250908170027144.png)

②La matrice di punti è 8\*16 in questo progetto, quindi imposta l'altezza a 8, la larghezza a 16, come mostrato di seguito.

![](media/image-20250908170040925.png)

③ Genera dati esadecimali dal motivo

Come mostrato di seguito, premi il pulsante sinistro del mouse per selezionare, il pulsante destro per annullare, disegna il motivo che desideri, fai clic su **Generate**, e i dati esadecimali di cui abbiamo bisogno verranno prodotti.

![](media/image-20250908170144895.png)

**Diagramma di collegamento**

![](media/image-20250908170158402.png)

Nota sul cablaggio: GND, VCC, SDA e SCL del pannello LED 8x16 sono rispettivamente collegati a -(GND), + (VCC), A4 e A5 della scheda di espansione sensori keyestudio per la comunicazione seriale a due fili. (Nota: questo pin è collegato all'IIC di Arduino, ma questo modulo non è una comunicazione IIC. Può essere collegato con due pin qualsiasi.)

**Codice di prova**

Il codice che mostra il viso sorridente.

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 9.1
 Matrix  face
 http://www.keyestudio.com
*/ 
// i dati del viso sorridente dallo strumento modulus
unsigned char smile[] = {0x00, 0x00, 0x1c, 0x02, 0x02, 0x02, 0x5c, 0x40, 0x40, 0x5c, 0x02, 0x02, 0x02, 0x1c, 0x00, 0x00};

#define SCL_Pin  A5  // Imposta il pin di clock a A5
#define SDA_Pin  A4  // Imposta il pin dati a A4

void setup()
{
  // Imposta il pin come output
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // cancella il display
  // matrix_display(clear);
}

void loop()
{
  matrix_display(smile);  // visualizza il viso sorridente
}
// la funzione per il display della matrice di punti

void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // utilizza la funzione della condizione di avvio della trasmissione dei dati
  IIC_send(0xc0);  // seleziona l'indirizzo
  
  for(int i = 0;i < 16;i++) // i dati del motivo hanno 16 bit
  {
     IIC_send(matrix_value[i]); // trasmetti i dati del motivo
  }

  IIC_end();   // termina la trasmissione dei dati del motivo  
  IIC_start();
  IIC_send(0x8A);  // controllo del display, imposta la larghezza di impulso a 4/16 s
  IIC_end();
}

// la condizione per iniziare a trasmettere i dati
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Trasmetti dati
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Ogni byte ha 8 bit 8bit per ogni carattere
  {
      digitalWrite(SCL_Pin,LOW);  // abbassa il pin di clock SCL_Pin per cambiare il segnale di SDA
      delayMicroseconds(3);
      if(send_data & 0x01)  // imposta il livello alto e basso di SDA_Pin secondo 1 o 0 di ogni bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // alza il pin di clock SCL_Pin per interrompere la trasmissione
      delayMicroseconds(3);
      send_data = send_data >> 1;  // rileva bit per bit, sposta i dati a destra di uno
  }
}

// Il segno della fine della trasmissione dei dati
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

**Risultato della prova**

Collega secondo il diagramma di collegamento. L'interruttore DIP è ruotato verso l'estremità destra e accendi l'alimentazione, il viso sorridente appare sulla matrice di punti.

![](media/image-20250908170421220.png)

**Pratica di estensione**

Utilizziamo lo strumento modulus ([http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)) per fare in modo che la matrice di punti visualizzi alternativamente i motivi di avanzamento e arresto, quindi cancella i motivi, e l'intervallo di tempo è 2000 millisecondi.

![](media/image-20250908170445861.png)

![](media/image-20250908170452897.png)

![](media/image-20250908170459213.png)

Ottieni il codice grafico da visualizzare tramite lo strumento modulus.

**Avvio：** 0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01

**Avanza：** 0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Indietro：** 0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Gira a sinistra：** 0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00

**Gira a destra：** 0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00

**Arresto：** 0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00

**Codice per cancellare il display**：0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00

Il codice per lo scorrimento di più motivi:

```c
/* keyestudio Mini Tank Robot V2.1
 lesson 9.2
 Matrix loop
 http://www.keyestudio.com
*/ 
// Array, utilizzato per memorizzare i dati del motivo, può essere calcolato da te o ottenuto dallo strumento modulus
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
#define SCL_Pin  A5  // Imposta il pin di clock a A5
#define SDA_Pin  A4  // Imposta il pin dati a A4
void setup(){
  // Imposta i pin come output
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // Cancella il display
  matrix_display(clear);
}
void loop(){
  matrix_display(start01);  // Visualizza il motivo di avvio
  delay(2000);
  matrix_display(front);    // Motivo di avanzamento
  delay(2000);
  matrix_display(STOP01);   // Motivo di arresto
  delay(2000);
  matrix_display(clear);    // Cancella il display Cancella lo schermo
  delay(2000);
}
// Questa funzione è utilizzata per il display della matrice di punti
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // chiama la funzione di avvio della trasmissione dei dati  
  IIC_send(0xc0);  // Scegli l'indirizzo
  
  for(int i = 0;i < 16;i++) // i dati del motivo hanno 16 bit
  {
     IIC_send(matrix_value[i]); // dati per trasmettere i motivi 
  }
  IIC_end();   // termina la trasmissione dei dati del motivo Fine
  IIC_start();
  IIC_send(0x8A);  // controllo del display, imposta la larghezza di impulso a 4/16
  IIC_end();
}
// La condizione per iniziare a trasmettere i dati
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Trasmetti dati
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Ogni byte ha 8 bit
  {
      digitalWrite(SCL_Pin,LOW);  // abbassa il pin di clock SCL per cambiare i segnali di SDA      
delayMicroseconds(3);
      if(send_data & 0x01)  // imposta il livello alto e basso di SDA_Pin secondo 1 o 0 di ogni bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // alza il pin di clock SCL per interrompere la trasmissione dei dati
      delayMicroseconds(3);
      send_data = send_data >> 1;  // rileva bit per bit, quindi sposta i dati a destra di uno
  }}
// Il segno che la trasmissione dei dati termina
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

Carica il codice sulla scheda di sviluppo, la matrice di punti 8\*16 visualizza alternativamente i motivi di avanzamento, indietro e arresto.

![](media/image-20250908170902116.png)

![](media/image-20250908170913925.png)

![](media/image-20250908170921862.png)
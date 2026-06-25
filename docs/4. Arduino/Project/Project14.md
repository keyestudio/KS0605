# Progetto 14 Controllo del Robot via Bluetooth

![](media/image-20250908173626827.png)

**Descrizione**

Abbiamo imparato le conoscenze di base del Bluetooth. In questa lezione, realizzeremo un'auto intelligente controllata da remoto via Bluetooth. Nell'esperimento, consideriamo il modulo Bluetooth HM-10 come Slave e il cellulare come Host.

keyes BT car è un'APP sviluppata dal team keyestudio. Puoi controllare facilmente l'auto robot con essa.

**APP**

- **APP Android**

  - Scarica l'APP da qui.

  ![](media/image-20250909110725934.png)

  - Tocca l'icona Tank_Car per accedere all'APP Bluetooth. Come mostrato di seguito.

    ![](media/image-20250909112000615.png)

  - Dopo aver caricato il codice sulla scheda UNO R3, collega il modulo Bluetooth, il LED sul modulo Bluetooth lampeggerà. Quindi tocca l'opzione CONNECT sull'APP per cercare il Bluetooth.

  ![](media/image-20250909112142944.png)

  - Fai clic per connettere il Bluetooth. HMSoft connesso, il LED del Bluetooth si accenderà normalmente.

  ![](media/image-20250909112205361.png)

  - Tocca il pulsante![](media/image-20250909112233736.png)①, il pannello LED 8x16 visualizzerà l'icona anteriore. Rilascia il pulsante, visualizza "STOP".
  - Di seguito è riportata l'interfaccia dell'APP Bluetooth del Robot Tank e abbiamo elencato la funzione di ogni tasto:

  ![](media/image-20250909112313634.png)

  ![](media/image-20250909111647904.png)

  ![](media/image-20250909111712783.png)

- **APP iOS**

  - Apri l'APP store

    ![](media/wps1.jpg)

  - Fai clic per cercare keyestudio e vedrai keyes BT car.

    ![](media/wps2.jpg)

  - Tocca per aprire keyes BT car

  - Per aprire il Bluetooth, fai clic su "Connect" nell'angolo in alto a sinistra, cerca e connetti il Bluetooth.

  ![](media/wps3.jpg)

  - Tocca l'icona Tank_Car per accedere all'interfaccia di controllo.

    ![](media/wps4.jpg)

    ![](media/wps5.jpg)

  - Di seguito è riportata l'interfaccia dell'APP Bluetooth del Robot Tank e abbiamo elencato la funzione di ogni tasto:

![](media/wps6.jpg)

![](media/image-20250909111647904.png)

![](media/image-20250909111712783.png)

**Codice di Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 14.1
 test bluetooth
 http://www.keyestudio.com
*/
char ble_val; //variabile carattere, utilizzata per salvare il valore ricevuto dal Bluetooth
void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  //verifica se ci sono dati nell'area buffer
  { ble_val = Serial.read();  //leggi i dati dal buffer seriale
    Serial.println(ble_val);  //stampa
  }
}//**************************************************************
```

**Scollega il modulo Bluetooth, carica il codice di test, ricollega il modulo Bluetooth, apri il monitor seriale e imposta il baud rate a 9600. Punta verso il modulo Bluetooth e premi i tasti sull'APP, quindi il carattere corrispondente è mostrato di seguito.**

![](media/image-20250908173736780.png)

Il carattere rilevato e la funzione corrispondente:

![](media/image-20250908173843012.png)

![](media/image-20250908173856246.png)

**Diagramma di Collegamento**

![](media/image-20250908173908251.png)

**Attenzione al Cablaggio:**

| Pannello LED 8x16                                            |      | Scheda di Espansione |
| ------------------------------------------------------------ | ---- | -------------------- |
| GND                                                          | →    | -（GND）             |
| VCC                                                          | →    | +（VCC）             |
| SDA                                                          | →    | SDA                  |
| SCL                                                          | →    | SCL                  |
| Inserisci il modulo Bluetooth verticalmente, non è necessario collegare i pin STATE e BRK |      |                      |

**Codice di Test**

**Nota:** Rimuovi il modulo Bluetooth prima di caricare il codice di test. Altrimenti, il caricamento del codice di test avrà esito negativo.

```c
/*
 keyestudio Robot Car v2.0
 lezione 14.2
 auto bluetooth
 http://www.keyestudio.com
*/

//Array, utilizzato per memorizzare i dati del modello, può essere calcolato da te o ottenuto dallo strumento modulo
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Imposta il pin di clock a A5
#define SDA_Pin  A4  //Imposta il pin di dati a A4

#define ML_Ctrl 13  //definisci il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definisci il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definisci il pin di controllo della direzione del motore destro
#define MR_PWM 3    //definisci il pin di controllo PWM del motore destro

char bluetooth_val; //salva il valore ricevuto dal Bluetooth

void setup(){
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    //Cancella il display
  matrix_display(start01);  //visualizza il modello di avvio

  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}

void loop(){
  if (Serial.available())
  {
    bluetooth_val = Serial.read();
    Serial.println(bluetooth_val);
  }
  switch (bluetooth_val) 
  {
     case 'F':  //comando avanti
        Car_front();
        matrix_display(front);  //mostra il design di avanzamento
        break;
     case 'B':  //comando indietro
        Car_back();
        matrix_display(back);  //mostra il modello di retromarcia
        break;
     case 'L':  //istruzione di svolta a sinistra
        Car_left();
        matrix_display(left);  //mostra il segno di "svolta a sinistra"
        break;
     case 'R':  //istruzione di svolta a destra
        Car_right();
        matrix_display(right);  //visualizza il segno di svolta a destra
       break;
     case 'S':  //comando di arresto
        Car_Stop();
        matrix_display(STOP01);  //mostra l'immagine di arresto
        break;
  }
}

/**************La funzione della matrice di punti****************/
//questa funzione è utilizzata per il display della matrice di punti
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //Scegli l'indirizzo
  
  for(int i = 0;i < 16;i++) //i dati del modello hanno 16 bit
  {
     IIC_send(matrix_value[i]); //dati per trasmettere i modelli
  }
  IIC_end();   //termina la trasmissione del modello di dati
  
  IIC_start();
  IIC_send(0x8A);  //controllo del display, imposta la larghezza dell'impulso a 4/16
  IIC_end();
}
//La condizione per iniziare la trasmissione dei dati
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
//trasmetti dati
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Ogni byte ha 8 bit
  {
      digitalWrite(SCL_Pin,LOW);  //abbassa il pin di clock SCL_Pin per cambiare i segnali di SDA    
delayMicroseconds(3);
      if(send_data & 0x01)  //imposta il livello alto e basso di SDA_Pin in base a 1 o 0 di ogni bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //alza il pin di clock SCL_Pin per interrompere la trasmissione dei dati
      delayMicroseconds(3);
      send_data = send_data >> 1;  //Rileva bit per bit, quindi sposta i dati a destra di uno
  }
}
//Il segno che la trasmissione dei dati è terminata
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
/*************la funzione per far funzionare il motore**************/
void Car_front()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_back()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,200);
}
void Car_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,255);
}
void Car_right()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,255);
}
void Car_Stop()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,0);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,0);
}
void Car_T_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,180);
}
void Car_T_right()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,180);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,255);
}
  //****************************************************************
```

**Risultato del Test**

Carica il codice con successo, l'interruttore DIP è posizionato all'estremità destra e accendi l'alimentazione. Dopo aver connesso il Bluetooth, puoi guidare l'auto intelligente tramite l'App Bluetooth.

Premi![](media/image-20250908174053007.png), il robot tank va avanti;

fai clic su![](media/image-20250908174111419.png), l'auto intelligente va indietro;

premi il pulsante![](media/image-20250908174138113.png), il robot tank gira a sinistra; fai clic su![](media/image-20250908174152325.png), il robot gira a destra;

tieni premuto![](media/image-20250908174213256.png), si ferma.

Fai clic su![](media/image-20250908174235827.png)per abilitare il controllo gravitazionale, tocca di nuovo![](media/image-20250908174249747.png), termina il controllo gravitazionale. Allo stesso tempo, il pannello LED 8X16 sul robot car visualizza il modello corrispondente.
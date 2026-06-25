# Progetto 13 Robot Carro Controllato da Telecomando IR

![](media/image-20250908172649810.png)

**Descrizione**

Il controllo remoto a infrarossi è uno dei controlli più diffusi, applicato in televisori, ventilatori elettrici e alcuni elettrodomestici. In questo progetto, realizzeremo un'auto intelligente controllata da telecomando IR. Poiché conosciamo ogni valore di tasto sul telecomando IR, potremmo controllare l'auto intelligente e visualizzare i modelli sulla matrice di punti tramite il valore di tasto corrispondente.

**La logica specifica del robot controllato da telecomando a infrarossi è mostrata di seguito:**

| Configurazione iniziale                | Angolo servo 90°                        |                                     |
| -------------------------------------- | --------------------------------------- | ----------------------------------- |
|                                        | Pannello matrice LED 8X16 mostra l'icona "V" |                                     |
| **Telecomando**                        | **Valore tasto**                        | **Stato tasto**                     |
| ![](media/image-20250908172904905.png) | FF629D                                  | Vai avanti (PWM impostato a 200)     |
|                                        |                                         | Pannello LED 8X16 mostra icona avanti |
| ![](media/image-20250908172927504.png) | FFA857                                  | Vai indietro (PWM impostato a 200)   |
|                                        |                                         | Pannello LED 8X16 mostra icona indietro |
| ![](media/image-20250908172954542.png) | FF22DD                                  | Gira a sinistra                     |
|                                        |                                         | Pannello LED 8X16 mostra icona sinistra |
| ![](media/image-20250908173027144.png) | FFC23D                                  | Gira a destra                       |
|                                        |                                         | Pannello LED 8X16 mostra icona destra |
| ![](media/image-20250908173139888.png) | FF02FD                                  | Ferma                               |
|                                        |                                         | Pannello LED 8X16 mostra "STOP"     |
| ![](media/image-20250908173312378.png) | FF30CF                                  | Ruota a sinistra (PWM impostato a 200) |
|                                        |                                         | Pannello LED 8X16 mostra icona sinistra |
| ![](media/image-20250908173336232.png) | FF7A85                                  | Ruota a destra (PWM impostato a 200) |
|                                        |                                         | Pannello LED 8X16 mostra icona destra |

**Diagramma di flusso**

![](media/image-20250908173443316.png)

**Diagramma di collegamento**

![](media/image-20250908173458023.png)

Attenzione: GND, VCC, SDA, SCL del pannello LED 8x16 sono rispettivamente collegati con - (GND), + (VCC), SDA, SCL. E "-", "+" e S del modulo ricevitore IR sono collegati a G (GND), V (VCC) e A0 sulla scheda sensore. Nel caso di porte digitali insufficienti, le porte analogiche possono essere utilizzate come porte digitali. A0 corrisponde al digitale 14, A1 corrisponde al digitale 15.

**Codice di prova**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 13
 Carro controllato da telecomando IR
 http://www.keyestudio.com
*/

#include <IRremoteTank.h>
IRrecv irrecv(A0);  //imposta IRrecv irrecv su A0
decode_results results;
long ir_rec;  //salva il valore IR ricevuto

//Array, utilizzato per memorizzare i dati del modello, può essere calcolato da te o ottenuto dallo strumento modulo
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Imposta il pin di clock su A5
#define SDA_Pin  A4  //Imposta il pin dati su A4

#define ML_Ctrl 13  //definisci il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definisci il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definisci il pin di controllo della direzione del motore destro
#define MR_PWM 3    //definisci il pin di controllo PWM del motore destro

#define servoPin 9 //pin del servo
int pulsewidth; //salva il valore della larghezza dell'impulso del servo

void setup(){
  Serial.begin(9600);
  irrecv.enableIRIn();  //Inizializza la libreria di ricezione IR
  
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); //Pulisci lo schermo
  matrix_display(start01);  //mostra l'immagine di avvio
  
  pinMode(servoPin, OUTPUT);
  procedure(90);  //Il servo ruota a 90°
}

void loop(){
  if (irrecv.decode(&results)) //ricevi il valore del telecomando IR
  {
    ir_rec=results.value;
    String type="UNKNOWN";
    String typelist[14]={"UNKNOWN", "NEC", "SONY", "RC5", "RC6", "DISH", "SHARP", "PANASONIC", "JVC", "SANYO", "MITSUBISHI", "SAMSUNG", "LG", "WHYNTER"};
    if(results.decode_type>=1&&results.decode_type<=13){
      type=typelist[results.decode_type];
    }
    Serial.print("IR TYPE:"+type+"  ");
    Serial.println(ir_rec,HEX);
    irrecv.resume();
  }
  
  if (ir_rec == 0xFF629D) //Vai avanti
  {
    Car_front();
    matrix_display(front);  //Visualizza immagine avanti
  }
  if (ir_rec == 0xFFA857)  //Il carro robot va indietro
  {
    Car_back();
    matrix_display(front);  //Vai indietro
  }
  if (ir_rec == 0xFF22DD)   //Il carro robot gira a sinistra
  {
    Car_T_left();
    matrix_display(left);  //Visualizza immagine di svolta a sinistra
  }
  if (ir_rec == 0xFFC23D)   //Il carro robot gira a destra
  {
    Car_T_right();
    matrix_display(right);  //Visualizza immagine di svolta a destra
  }
  if (ir_rec == 0xFF02FD)   //Il carro robot si ferma
  { 
    Car_Stop();
    matrix_display(STOP01);  //mostra immagine di arresto
  }
  if (ir_rec == 0xFF30CF)   //il carro robot ruota in senso antiorario
  {
    Car_left();
    matrix_display(left);  //mostra immagine di rotazione antioraria
  }
  if (ir_rec == 0xFF7A85)  //il carro robot ruota in senso orario
  {
    Car_right();
    matrix_display(right);  //mostra immagine di rotazione oraria
 }
}
/******************Controllo Servo*******************/
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}

/******************Matrice di punti****************/
// questa funzione è utilizzata per la visualizzazione della matrice di punti
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //Scegli indirizzo
   for(int i = 0;i < 16;i++) //L'immagine ha 16 bit
  {
     IIC_send(matrix_value[i]); //dati per trasmettere modelli
  }
  IIC_end();   //termina la trasmissione del modello dati
  
  IIC_start();
  IIC_send(0x8A);  //controllo visualizzazione, imposta larghezza impulso a 4/16
  IIC_end();
}

//La condizione per iniziare la trasmissione dati
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Ogni byte ha 8 bit, 8 bit per ogni carattere
  {
      digitalWrite(SCL_Pin,LOW);  //abbassa il pin di clock SCL per cambiare i segnali di SDA      
      delayMicroseconds(3);
      if(send_data & 0x01)  //imposta il livello alto e basso di SDA_Pin secondo 1 o 0 di ogni bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //alza il pin di clock SCL_Pin per interrompere la trasmissione dati
      delayMicroseconds(3);
      send_data = send_data >> 1;  //rileva bit per bit, quindi sposta i dati a destra di uno
  }
}
//Il segno che la trasmissione dati è terminata
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
/***************la funzione per eseguire il motore***************/
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

**Risultato della prova**

Carica il codice con successo e accendi il robot intelligente, che può essere controllato dal telecomando IR. Allo stesso tempo, il modello corrispondente viene visualizzato sul pannello LED 8X16.
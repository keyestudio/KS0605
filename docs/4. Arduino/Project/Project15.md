# Progetto 15: Progetto Finale Completamente Funzionale

**Codice di Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 15
 serbatoio bluetooth
 http://www.keyestudio.com
*/

//Array, utilizzato per memorizzare i dati del modello, può essere calcolato da solo o ottenuto dallo strumento modulo
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Impostare il pin dell'orologio su A5
#define SDA_Pin  A4  //Impostare il pin dei dati su A4

#define ML_Ctrl 13  //definire il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definire il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definire il pin di controllo della direzione del motore destro
#define MR_PWM 3    //definire il pin di controllo PWM del motore destro

char bluetooth_val; //salvare il valore della ricezione Bluetooth

void setup()
{
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    //Cancellare il display
  matrix_display(start01);  //visualizzare il modello di avvio

  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}

void loop()
{
  if (Serial.available())
  {
    bluetooth_val = Serial.read();
    Serial.println(bluetooth_val);
  }
  switch (bluetooth_val) 
  {
     case 'F':  //comando di avanzamento
        Car_front();
        matrix_display(front);  // mostra il design di avanzamento
        break;
     case 'B':  //comando di retromarcia
        Car_back();
        matrix_display(back);  //mostra il modello di retromarcia
        break;
     case 'L':  // istruzione di svolta a sinistra
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
//questa funzione è utilizzata per la visualizzazione della matrice di punti
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //Scegliere l'indirizzo
  
  for(int i = 0;i < 16;i++) //i dati del modello hanno 16 bit
  {
     IIC_send(matrix_value[i]); //dati per trasmettere i modelli
  }
  IIC_end();   //terminare la trasmissione del modello di dati
  
  IIC_start();
  IIC_send(0x8A);  //controllo del display, impostare la larghezza dell'impulso su 4/16
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
//trasmettere i dati
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Ogni byte ha 8 bit
  {
      digitalWrite(SCL_Pin,LOW);  //abbassare il pin dell'orologio SCL Pin per cambiare i segnali di SDA      
      delayMicroseconds(3);
      if(send_data & 0x01)  //impostare il livello alto e basso di SDA_Pin secondo 1 o 0 di ogni bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //alzare il pin dell'orologio SCL_Pin per interrompere la trasmissione dei dati
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Rilevare bit per bit, quindi spostare i dati a destra di uno
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
```

**Risultato del Test**

**Nota:** Rimuovere il modulo Bluetooth prima di caricare il codice di test. Altrimenti, non riuscirai a caricare il codice di test. Ricollega il modulo Bluetooth dopo aver caricato il codice di test

Carica il codice di test con successo, inserisci il modulo Bluetooth, accendi e connettiti a Bluetooth. Il robot serbatoio può mostrare funzioni distinte tramite l'App.

Bene, tutti i progetti sono terminati. Non esitare a contattarci se riscontri dei problemi.
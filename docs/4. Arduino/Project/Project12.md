# Progetto 12 Carro Seguente a Ultrasuoni

![](media/image-20250908172315808.png)

**Descrizione**

Nel progetto 11, abbiamo realizzato un'auto che evita gli ostacoli. In realtà, abbiamo solo bisogno di modificare il codice di test per trasformare un'auto che evita gli ostacoli in un'auto seguente. In questa lezione, realizzeremo un robot seguente a ultrasuoni. Il sensore a ultrasuoni rileva la distanza tra lo smart car e l'ostacolo per guidare il carro serbatoio a muoversi.

**La logica specifica del robot seguente a ultrasuoni è mostrata di seguito:**

| **Rilevamento** | **Distanza misurata degli ostacoli frontali** | **Distanza (unità: cm)** |
| --------------- | --------------------------------------------- | ----------------------- |
| Impostazioni    | Angolo servo 90°                              |                         |
|                 | Pannello LED 8X16 mostra l'icona "V"         |                         |
| Se              | 20≤ distanza ≤60                              |                         |
| Stato           | Vai avanti（imposta PWM a 200）               |                         |
| Se              | 10\<distanza＜20                              |                         |
|                 | distanza＞60                                  |                         |
| Stato           | Ferma                                         |                         |
| Se              | distanza ≤10                                  |                         |
| Stato           | Ferma（imposta PWM a 200）                    |                         |

**Diagramma di flusso**

![](media/image-20250908172442991.png)

**Diagramma di Collegamento**

![](media/image-20250908172457017.png)

Nota sul collegamento:

| Pannello LED 8x16 |      | Sensore V5 Shield |
| ----------------- | ---- | ----------------- |
| GND               | →    | -（GND）          |
| VCC               | →    | +（VCC）          |
| SDA               | →    | SDA               |
| SCL               | →    | SCL               |

**Codice di Test**

```c
 /*
 keyestudio Mini Tank Robot V2.1
 lesson 12
 ultrasonic follow tank
 http://www.keyestudio.com
*/ 
//Array, utilizzato per memorizzare i dati del pattern, può essere calcolato da te o ottenuto dallo strumento modulo
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Imposta il pin dell'orologio su A5
#define SDA_Pin  A4  //Imposta il pin dei dati su A4

#define ML_Ctrl 13  //definisci il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definisci il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definisci il pin di controllo della direzione del motore destro
#define MR_PWM 3   //definisci il pin di controllo PWM del motore destro
#define Trig 5  //pin Trig ultrasuoni
#define Echo 4  //pin Echo ultrasuoni
int distance;
int pulsewidth;
#define servoPin 9  //pin servo
void setup(){
  Serial.begin(9600);
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); //Cancella il display
  matrix_display(start01);  //mostra il pattern di avvio
  pinMode(servoPin, OUTPUT);
  procedure(90); //imposta il servo a 90°
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  distance = checkdistance();  //assegna la distanza rilevata dal sensore a ultrasuoni a distance
  if (distance >= 20 && distance <= 60) //intervallo per andare avanti
  {
    Car_front();
  }
  else if (distance > 10 && distance < 20)  //intervallo per fermarsi
  {
    Car_Stop();
  }
  else if (distance <= 10)  //intervallo per andare indietro
  {
    Car_back();
  }
  else  //altre situazioni, ferma
  {
    Car_Stop();
  }
}
/***********la funzione per il funzionamento del motore****************/
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
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,200);
}
void Car_right()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_Stop()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,0);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,0);
}

/******************dot matrix********************/
// la funzione per il display della matrice di punti
void matrix_display(unsigned char matrix_value[])
{
  IIC_start(); // chiama la funzione che inizia la trasmissione dei dati
  IIC_send(0xc0);  //Scegli l'indirizzo
  
  for(int i = 0;i < 16;i++) //i dati del pattern hanno 16 bit
  {
     IIC_send(matrix_value[i]); //dati per trasmettere i pattern
  }

  IIC_end();   //termina la trasmissione del pattern di dati
  
  IIC_start();
  IIC_send(0x8A);  //seleziona la larghezza dell'impulso 4/16, controlla il display
  IIC_end();
}

//La condizione per iniziare a trasmettere i dati
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

// trasmetti i dati
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Ogni byte ha 8 bit
  {
      digitalWrite(SCL_Pin,LOW);  //abbassa il pin dell'orologio SCL Pin per cambiare i segnali di SDA      
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
      digitalWrite(SCL_Pin,HIGH); //alza il pin dell'orologio SCL_Pin per smettere di trasmettere i dati
      delayMicroseconds(3);
      send_data = send_data >> 1;  // rileva bit per bit, quindi sposta i dati a destra di uno
  }
}
//Il segno che la trasmissione dei dati termina
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
/***************fine del display della matrice di punti******************/
//La funzione per controllare il servo
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }}
//La funzione per controllare la funzione del sensore a ultrasuoni che controlla gli ultrasuoni
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.20;  //58.20, cioè, 2*29.1=58.2
  delay(10);
  return distance;
}
//****************************************************************
```

 **Risultato del Test**

Carica il codice con successo, l'interruttore DIP è ruotato all'estremità destra, il servo ruota a 90°, "V" è mostrato sul pannello LED 8X16 e lo smart car si muove mentre l'ostacolo si muove.
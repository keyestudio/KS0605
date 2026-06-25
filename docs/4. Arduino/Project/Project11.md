# Progetto 11 Serbatoio di Evitamento Ultrasonico

![](media/image-20250908171729897.png)

**Descrizione**

In questo programma, il sensore ultrasonico rileva la distanza dell'ostacolo per inviare segnali che controllano l'auto robot. Successivamente, ti mostreremo come realizzare un'auto con evitamento degli ostacoli.

**La logica specifica del robot di evitamento ultrasonico è mostrata di seguito:**

![](media/image-20250908171756879.png)

 **Diagramma di flusso**

![](media/image-20250908171812532.png)

**Diagramma di Connessione:**

![](media/image-20250908171829321.png)

Nota: i pin "-", "+" e "S" del servo sono rispettivamente collegati a G（GND）, V（VCC）e D9 della scheda di espansione. VCC, Trig, Echo e Gnd del sensore ultrasonico sono collegati con 5v(V), 5(S), Echo e Gnd(G) della scheda di espansione.

**Codice di Test:**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 11
 ultrasonic_avoid_tank
 http://www.keyestudio.com
*/
int random2;
int a;
int a1;
int a2;
#define ML_Ctrl 13  //definire il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definire il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definire il pin di controllo della direzione del motore destro
#define MR_PWM 3   //definire il pin di controllo PWM del motore destro

#define Trig 5  //pin Trig ultrasonico
#define Echo 4  //pin Echo ultrasonico
int distance;
#define servoPin 9  //pin Servo
int pulsewidth;
/************la funzione per far funzionare il motore**************/
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

//La funzione per controllare il servo
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}
//La funzione per controllare il sensore ultrasonico
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.00;  //58.20, cioè, 2*29.1=58.2
  delay(10);
  return distance;
}
  //****************************************************************
void setup(){
  pinMode(servoPin, OUTPUT);
  procedure(90); //impostare il servo a 90°
  
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  random2 = random(1, 100);
  a = checkdistance();  //assegnare la distanza frontale rilevata dal sensore ultrasonico alla variabile a
  
  if (a < 20) //quando la distanza frontale rilevata è inferiore a 20 
  {
      Car_Stop();  //il robot si ferma
      delay(500); //ritardo di 500ms
      procedure(160);  //la piattaforma ultrasonica gira a sinistra
      for (int j = 1; j <= 10; j = j + (1)) { //istruzione for, i dati saranno più accurati se il sensore ultrasonico rileva più volte.
        a1 = checkdistance();  //assegnare la distanza sinistra rilevata dal sensore ultrasonico alla variabile a1
      }
      delay(300);
      procedure(20); //la piattaforma ultrasonica gira a destra
      for (int k = 1; k <= 10; k = k + (1)) {
        a2 = checkdistance(); //assegnare la distanza destra rilevata dal sensore ultrasonico alla variabile a2
      }
      
      if (a1 < 50 || a2 < 50)  //il robot girerà verso il lato con distanza maggiore quando la distanza sinistra o destra è inferiore a 50cm. 
      {
        if (a1 > a2) //la distanza sinistra è maggiore del lato destro      
        {
          procedure(90);  //la piattaforma ultrasonica gira di nuovo verso destra         
Car_left();  //il robot gira a sinistra
          delay(500);  //gira a sinistra per 500ms
          Car_front(); //vai avanti
        } 
        else 
        {
          procedure(90);
          Car_right(); //il robot gira a destra
          delay(500);
          Car_front();  //vai avanti
        }
      } 
      else  //Se entrambi i lati sono maggiori o uguali a 50cm, gira a sinistra o a destra casualmente
      {
        if ((long) (random2) % (long) (2) == 0)  //Quando il numero casuale è pari
        {
          procedure(90);
          Car_left(); //il robot serbatoio gira a sinistra
          delay(500);
          Car_front(); //vai avanti
        } 
        else 
        {
          procedure(90);
          Car_right(); //il robot gira a destra
          delay(500);
          Car_front(); //vai avanti
       }
     }
  } 
  else  //Se la distanza frontale è maggiore o uguale a 20cm, l'auto robot andrà avanti
  {
      Car_front(); //vai avanti
  }
}
```

 **Risultato del Test**

Carica il codice con successo, il selettore DIP viene spostato all'estremità destra e accendi l'alimentazione, il robot serbatoio va avanti ed evita automaticamente l'ostacolo.
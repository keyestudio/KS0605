# Progetto 10 Robot Seguace della Luce

![](media/image-20250908171131879.png)

**Descrizione**

Abbiamo introdotto come utilizzare vari sensori e moduli.

In questa lezione, combiniamo le conoscenze hardware -- modulo fotoresistenza, azionamento motore, per costruire un robot seguace della luce!

Basta utilizzare 2 moduli fotoresistenza per rilevare l'intensità luminosa su entrambi i lati del robot. Leggere il valore analogico per ruotare i 2 motori, così da far muovere il robot carro armato.

**La logica specifica del robot seguace della luce è mostrata nella tabella sottostante:**

![](media/image-20250908171219561.png)

Abbiamo creato un diagramma di flusso basato sulla tabella logica sopra, come mostrato di seguito:

![](media/image-20250908171232654.png)

**Diagramma di Collegamento**

![](media/image-20250908171305946.png)

**Attenzione:**

Il blocco terminale a 4 pin è contrassegnato con serigrafia 1234. Il filo rosso del motore posteriore destro è collegato al terminale 1, il filo nero è collegato all'estremità 2. Il filo rosso del motore anteriore sinistro è collegato al terminale 3, il filo nero è collegato alla porta 4.

| Fotoresistenza sinistra  |      | Sensor Shield     |
| ------------------------ | ---- | ----------------- |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A1                |
|                          |      |                   |
| **Fotoresistenza destra** |      | **Sensor Shield** |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A2                |

**Codice di Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 10
 Carro armato seguace della luce
 http://www.keyestudio.com
*/ 
#define light_L_Pin A1   //definire il pin della fotoresistenza sinistra
#define light_R_Pin A2   //definire il pin della fotoresistenza destra
#define ML_Ctrl 13  //definire il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definire il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definire il pin di controllo della direzione del motore destro
#define MR_PWM 3   //definire il pin di controllo PWM del motore destro
int left_light; 
int right_light;
void setup(){
  Serial.begin(9600);
  pinMode(light_L_Pin, INPUT);
  pinMode(light_R_Pin, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  left_light = analogRead(light_L_Pin);
  right_light = analogRead(light_R_Pin);
  Serial.print("left_light_value = ");
  Serial.println(left_light);
  Serial.print("right_light_value = ");
  Serial.println(right_light);
  if (left_light > 650 && right_light > 650) //il valore rilevato dalla fotoresistenza, vai avanti
  {  
    Car_front();
  } 
  else if (left_light > 650 && right_light <= 650)  //il valore rilevato dalla fotoresistenza, gira a sinistra
  {
    Car_left();
  } 
  else if (left_light <= 650 && right_light > 650) //il valore rilevato dalla fotoresistenza, gira a destra
  {
    Car_right();
  } 
  else  //altre situazioni, ferma
  {
    Car_Stop();
  }
}
void Car_front()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
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
//****************************************************************
```

**Risultato del Test**

Caricare il codice sulla scheda di sviluppo keyestudio V4.0, l'interruttore DIP è posizionato all'estremità destra e accendere l'alimentazione, il robot intelligente segue la luce per muoversi.
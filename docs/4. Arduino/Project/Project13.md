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
IRrecv irrecv(A0);  //set IRrecv irrecv to A0
decode_results results;
long ir_rec;  //save the IR value received

//Array, used to store the data of the pattern, can be calculated by yourself or obtained from the modulus tool
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Set clock pin to A5
#define SDA_Pin  A4  //Set data pin to A4

#define ML_Ctrl 13  //define the direction control pin of left motor
#define ML_PWM 11   //define PWM control pin of left motor
#define MR_Ctrl 12  //define the direction control pin of right motor
#define MR_PWM 3    //define PWM control pin of right motor

#define servoPin 9 //pin of servo
int pulsewidth; //save the pulse width value of servo

void setup(){
  Serial.begin(9600);
  irrecv.enableIRIn();  //Initialize the IR reception library
  
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); //Clear Screen
  matrix_display(start01);  //show start picture
  
  pinMode(servoPin, OUTPUT);
  procedure(90);  //Servo rotates to 90°
}

void loop(){
  if (irrecv.decode(&results)) //receive the IR remote value
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
  
  if (ir_rec == 0xFF629D) //Go forward
  {
    Car_front();
    matrix_display(front);  //Display front image
  }
  if (ir_rec == 0xFFA857)  //Robot car goes back
  {
    Car_back();
    matrix_display(back);  //Go back
  }
  if (ir_rec == 0xFF22DD)   //Robot car turns left
  {
    Car_T_left();
    matrix_display(left);  //Display left-turning image
  }
  if (ir_rec == 0xFFC23D)   //Robot car turns right
  {
    Car_T_right();
    matrix_display(right);  //Display right-turning image
  }
  if (ir_rec == 0xFF02FD)   //Robot car stops
  { 
    Car_Stop();
    matrix_display(STOP01);  //show stop image
  }
  if (ir_rec == 0xFF30CF)   //robot car rotates anticlockwise
  {
    Car_left();
    matrix_display(left);  //show anticlockwise rotation picture
  }
  if (ir_rec == 0xFF7A85)  //robot car rotates clockwise
  {
    Car_right();
    matrix_display(right);  //show clockwise rotation picture
 }
}
/******************Control Servo*******************/
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}

/******************Dot Matrix****************/
// this function is used for dot matrix display 
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //Choose address
   for(int i = 0;i < 16;i++) //The picture has 16 bits
  {
     IIC_send(matrix_value[i]); //data to convey patterns
  }
  IIC_end();   //end to convey data pattern
  
  IIC_start();
  IIC_send(0x8A);  //display control, set pulse width to 4/16
  IIC_end();
}

//The condition starting to transmit data
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
  for(char i = 0;i < 8;i++)  //Each byte has 8 bits 8bits for every character
  {
      digitalWrite(SCL_Pin,LOW);  //pull down clock pin SCL Pin to change the signals of SDA      
      delayMicroseconds(3);
      if(send_data & 0x01)  //set high and low level of SDA_Pin according to 1 or 0 of every bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //pull up clock pin SCL_Pin to stop transmitting data
      delayMicroseconds(3);
      send_data = send_data >> 1;  // detect bit by bit, so move the data right by one
  }
}
//The sign that data transmission ends
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
/***************the function to run motor***************/
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
  analogWrite(ML_PWM,150);
}
void Car_T_right()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,150);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,255);
}
 //****************************************************************
```

**Risultato della prova**

Carica il codice con successo e accendi il robot intelligente, che può essere controllato dal telecomando IR. Allo stesso tempo, il modello corrispondente viene visualizzato sul pannello LED 8X16.

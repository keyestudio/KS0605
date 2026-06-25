# Projet 13 Robot Tank Télécommandé IR

![](media/image-20250908172649810.png)

**Description**

La télécommande infrarouge est l'un des systèmes de contrôle les plus omniprésents, utilisé dans les téléviseurs, les ventilateurs électriques et certains appareils électroménagers. Dans ce projet, nous allons créer une voiture intelligente télécommandée par infrarouge. Puisque nous connaissons chaque valeur de touche sur la télécommande IR, nous pouvons contrôler la voiture intelligente et afficher les motifs sur la matrice de points en fonction de la valeur de touche correspondante.

**La logique spécifique du robot télécommandé infrarouge est présentée ci-dessous :**

| Configuration initiale                 | Angle du servo 90°                      |                                     |
| -------------------------------------- | --------------------------------------- | ----------------------------------- |
|                                        | Le panneau matrice LED 8X16 affiche une icône "V" |                                     |
| **Télécommande**                       | **Valeur de touche**                    | **État de la touche**               |
| ![](media/image-20250908172904905.png) | FF629D                                  | Avancer（PWM réglé à 200）          |
|                                        |                                         | Le panneau LED 8X16 affiche l'icône avant |
| ![](media/image-20250908172927504.png) | FFA857                                  | Reculer（PWM réglé à 200）          |
|                                        |                                         | Le panneau LED 8X16 affiche l'icône arrière |
| ![](media/image-20250908172954542.png) | FF22DD                                  | Tourner à gauche                    |
|                                        |                                         | Le panneau LED 8X16 affiche l'icône vers la gauche |
| ![](media/image-20250908173027144.png) | FFC23D                                  | Tourner à droite                    |
|                                        |                                         | Le panneau LED 8X16 affiche l'icône vers la droite |
| ![](media/image-20250908173139888.png) | FF02FD                                  | Arrêter                             |
|                                        |                                         | Le panneau LED 8X16 affiche "STOP" |
| ![](media/image-20250908173312378.png) | FF30CF                                  | Rotation vers la gauche（PWM réglé à 200） |
|                                        |                                         | Le panneau LED 8X16 affiche l'icône vers la gauche |
| ![](media/image-20250908173336232.png) | FF7A85                                  | Rotation vers la droite（PWM réglé à 200） |
|                                        |                                         | Le panneau LED 8X16 affiche l'icône vers la droite |

 **Organigramme**

![](media/image-20250908173443316.png)

**Schéma de connexion**

![](media/image-20250908173458023.png)

Attention : GND, VCC, SDA, SCL du panneau LED 8x16 sont respectivement connectés à -（GND), +（VCC), SDA, SCL. Et "-", "+" et S du module récepteur IR sont attachés à G（GND), V（VCC) et A0 sur le bouclier capteur. En cas de ports numériques insuffisants, les ports analogiques peuvent être traités comme des ports numériques. A0 équivaut à la broche numérique 14, A1 est comme la broche numérique 15.

**Code de test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 13
 Robot tank télécommandé IR
 http://www.keyestudio.com
*/

#include <IRremoteTank.h>
IRrecv irrecv(A0);  // définir IRrecv irrecv à A0
decode_results results;
long ir_rec;  // sauvegarder la valeur IR reçue

// Tableau, utilisé pour stocker les données du motif, peut être calculé par vous-même ou obtenu à partir de l'outil de modulus
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Définir la broche d'horloge à A5
#define SDA_Pin  A4  // Définir la broche de données à A4

#define ML_Ctrl 13  // définir la broche de contrôle de direction du moteur gauche
#define ML_PWM 11   // définir la broche de contrôle PWM du moteur gauche
#define MR_Ctrl 12  // définir la broche de contrôle de direction du moteur droit
#define MR_PWM 3    // définir la broche de contrôle PWM du moteur droit

#define servoPin 9 // broche du servo
int pulsewidth; // sauvegarder la valeur de largeur d'impulsion du servo

void setup(){
  Serial.begin(9600);
  irrecv.enableIRIn();  // Initialiser la bibliothèque de réception IR
  
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); // Effacer l'écran
  matrix_display(start01);  // afficher l'image de démarrage
  
  pinMode(servoPin, OUTPUT);
  procedure(90);  // Le servo tourne à 90°
}

void loop(){
  if (irrecv.decode(&results)) // recevoir la valeur de la télécommande IR
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
  
  if (ir_rec == 0xFF629D) // Avancer
  {
    Car_front();
    matrix_display(front);  // Afficher l'image avant
  }
  if (ir_rec == 0xFFA857)  // La voiture robot recule
  {
    Car_back();
    matrix_display(front);  // Reculer
  }
  if (ir_rec == 0xFF22DD)   // La voiture robot tourne à gauche
  {
    Car_T_left();
    matrix_display(left);  // Afficher l'image de virage à gauche
  }
  if (ir_rec == 0xFFC23D)   // La voiture robot tourne à droite
  {
    Car_T_right();
    matrix_display(right);  // Afficher l'image de virage à droite
  }
  if (ir_rec == 0xFF02FD)   // La voiture robot s'arrête
  { 
    Car_Stop();
    matrix_display(STOP01);  // afficher l'image d'arrêt
  }
  if (ir_rec == 0xFF30CF)   // la voiture robot tourne dans le sens antihoraire
  {
    Car_left();
    matrix_display(left);  // afficher l'image de rotation dans le sens antihoraire
  }
  if (ir_rec == 0xFF7A85)  // la voiture robot tourne dans le sens horaire
  {
    Car_right();
    matrix_display(right);  // afficher l'image de rotation dans le sens horaire
 }
}
/******************Contrôle du servo*******************/
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}

/******************Matrice de points****************/
// cette fonction est utilisée pour l'affichage de la matrice de points
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  // Choisir l'adresse
   for(int i = 0;i < 16;i++) // L'image a 16 bits
  {
     IIC_send(matrix_value[i]); // données pour transmettre les motifs
  }
  IIC_end();   // fin de la transmission du motif de données
  
  IIC_start();
  IIC_send(0x8A);  // contrôle d'affichage, définir la largeur d'impulsion à 4/16
  IIC_end();
}

// La condition pour commencer à transmettre les données
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
  for(char i = 0;i < 8;i++)  // Chaque octet a 8 bits, 8 bits pour chaque caractère
  {
      digitalWrite(SCL_Pin,LOW);  // abaisser la broche d'horloge SCL Pin pour changer les signaux de SDA      
      delayMicroseconds(3);
      if(send_data & 0x01)  // définir le niveau haut et bas de SDA_Pin selon 1 ou 0 de chaque bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // relever la broche d'horloge SCL_Pin pour arrêter la transmission de données
      delayMicroseconds(3);
      send_data = send_data >> 1;  // détecter bit par bit, donc décaler les données vers la droite d'un
  }
}
// Le signe que la transmission de données se termine
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
/***************la fonction pour faire fonctionner le moteur***************/
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

**Résultat du test**

Après avoir téléchargé le code avec succès et mis sous tension, le robot intelligent peut être contrôlé par la télécommande IR. En même temps, le motif correspondant s'affiche sur le panneau LED 8X16.
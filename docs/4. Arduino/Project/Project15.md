# Projet 15 : Projet Final Entièrement Fonctionnel

**Code de Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 15
 réservoir bluetooth
 http://www.keyestudio.com
*/

//Tableau, utilisé pour stocker les données du motif, peut être calculé par vous-même ou obtenu à partir de l'outil de modulus
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Définir la broche d'horloge sur A5
#define SDA_Pin  A4  //Définir la broche de données sur A4

#define ML_Ctrl 13  //définir la broche de contrôle de direction du moteur gauche
#define ML_PWM 11   //définir la broche de contrôle PWM du moteur gauche
#define MR_Ctrl 12  //définir la broche de contrôle de direction du moteur droit
#define MR_PWM 3    //définir la broche de contrôle PWM du moteur droit

char bluetooth_val; //sauvegarder la valeur de la réception Bluetooth

void setup()
{
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    //Effacer l'affichage
  matrix_display(start01);  //afficher le motif de démarrage

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
     case 'F':  //commande d'avance
        Car_front();
        matrix_display(front);  // afficher le motif d'avance
        break;
     case 'B':  //commande de recul
        Car_back();
        matrix_display(back);  //afficher le motif de recul
        break;
     case 'L':  // instruction de virage à gauche
        Car_left();
        matrix_display(left);  //afficher le signe de virage à gauche
        break;
     case 'R':  //instruction de virage à droite
        Car_right();
        matrix_display(right);  //afficher le signe de virage à droite
        break;
     case 'S':  //commande d'arrêt
        Car_Stop();
        matrix_display(STOP01);  //afficher l'image d'arrêt
        break;
  }
}

/**************La fonction de la matrice de points****************/
//cette fonction est utilisée pour l'affichage de la matrice de points
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //Choisir l'adresse
  
  for(int i = 0;i < 16;i++) //les données du motif ont 16 bits
  {
     IIC_send(matrix_value[i]); //données pour transmettre les motifs
  }
  IIC_end();   //terminer la transmission du motif de données
  
  IIC_start();
  IIC_send(0x8A);  //contrôle d'affichage, définir la largeur d'impulsion sur 4/16
  IIC_end();
}
//La condition pour commencer la transmission de données
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
//transmettre les données
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Chaque octet a 8 bits
  {
      digitalWrite(SCL_Pin,LOW);  //abaisser la broche d'horloge SCL_Pin pour modifier les signaux de SDA
      delayMicroseconds(3);
      if(send_data & 0x01)  //définir le niveau haut et bas de SDA_Pin selon le 1 ou 0 de chaque bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //relever la broche d'horloge SCL_Pin pour arrêter la transmission de données
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Détecter bit par bit, donc décaler les données vers la droite d'un
  }
}
//Le signe que la transmission de données se termine
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
/*************la fonction pour faire fonctionner le moteur**************/
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

**Résultat du Test**

**Remarque :** Retirez le module Bluetooth avant de télécharger le code de test. Sinon, vous échouerez à télécharger le code de test. Reconnectez le module Bluetooth après le téléchargement du code de test.

Téléchargez le code de test avec succès, insérez le module Bluetooth, allumez l'appareil et connectez-vous à Bluetooth. Le robot réservoir peut afficher des fonctions distinctes via l'application.

Très bien, tous les projets sont terminés. N'hésitez pas à nous contacter si vous rencontrez des problèmes.
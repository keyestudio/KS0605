# Projet 14 Contrôle du Robot par Bluetooth

![](media/image-20250908173626827.png)

**Description**

Nous avons appris les connaissances de base sur le Bluetooth. Dans cette leçon, nous allons créer une voiture intelligente télécommandée par Bluetooth. Dans l'expérience, nous considérons par défaut le module Bluetooth HM-10 comme Esclave et le téléphone portable comme Maître.

keyes BT car est une application développée par l'équipe keyestudio. Vous pouvez contrôler facilement la voiture robot avec celle-ci.

**APPLICATION**

- **Application Android**

  - Veuillez télécharger l'application ici.

  ![](media/image-20250909110725934.png)

  - Appuyez sur l'icône Tank_Car pour accéder à l'application Bluetooth. Comme indiqué ci-dessous.

    ![](media/image-20250909112000615.png)

  - Après avoir téléchargé le code sur la carte UNO R3, connectez le module Bluetooth, la LED du module Bluetooth clignotera. Ensuite, appuyez sur l'option CONNECT dans l'application pour rechercher le Bluetooth.

  ![](media/image-20250909112142944.png)

  - Cliquez pour connecter le Bluetooth. HMSoft connecté, la LED Bluetooth s'allumera normalement.

  ![](media/image-20250909112205361.png)

  - Appuyez sur le bouton ![](media/image-20250909112233736.png)①, le panneau LED 8x16 affichera l'icône avant. Relâchez le bouton, affichage « STOP ». 
  - Ci-dessous se trouve l'interface de l'application Bluetooth du robot Tank et nous avons listé la fonction de chaque touche :

  ![](media/image-20250909112313634.png)

  ![](media/image-20250909111647904.png)

  ![](media/image-20250909111712783.png)

- **Application iOS**

  - Ouvrez l'App Store

    ![](media/wps1.jpg)

  - Cliquez pour rechercher keyestudio, et vous verrez keyes BT car.

    ![](media/wps2.jpg)

  - Appuyez pour ouvrir keyes BT car

  - Pour ouvrir le Bluetooth, cliquez sur « Connect » dans le coin supérieur gauche, recherchez et connectez le Bluetooth.

  ![](media/wps3.jpg)

  - Appuyez sur l'icône Tank_Car pour accéder à l'interface de contrôle.

    ![](media/wps4.jpg)

    ![](media/wps5.jpg)

  - Ci-dessous se trouve l'interface de l'application Bluetooth du robot Tank et nous avons listé la fonction de chaque touche :

![](media/wps6.jpg)

![](media/image-20250909111647904.png)

![](media/image-20250909111712783.png)

**Code de Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 14.1
 test bluetooth
 http://www.keyestudio.com
*/
char ble_val; //variable de caractère, utilisée pour enregistrer la valeur reçue par Bluetooth
void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  //vérifier s'il y a des données dans la zone tampon
  { ble_val = Serial.read();  //lire les données du tampon série
    Serial.println(ble_val);  //afficher
  }
}//**************************************************************
```

**Retirez le module Bluetooth, téléchargez le code de test, reconnectez le module Bluetooth, ouvrez le moniteur série et réglez la vitesse de transmission à 9600. Pointez vers le module Bluetooth et appuyez sur les touches de l'application, le caractère correspondant s'affiche comme ci-dessous.**

![](media/image-20250908173736780.png)

Le caractère détecté et la fonction correspondante :

![](media/image-20250908173843012.png)

![](media/image-20250908173856246.png)

**Schéma de Connexion**

![](media/image-20250908173908251.png)

**Attention au Câblage :**

| Panneau LED 8x16                                             |      | Carte d'Extension |
| ------------------------------------------------------------ | ---- | --------------- |
| GND                                                          | →    | -（GND）        |
| VCC                                                          | →    | +（VCC）        |
| SDA                                                          | →    | SDA             |
| SCL                                                          | →    | SCL             |
| Insérez le module Bluetooth verticalement, vous n'avez pas besoin de connecter ses broches STATE et BRK |      |                 |

 **Code de Test**

**Remarque :** Retirez le module Bluetooth avant de télécharger le code de test. Sinon, le téléchargement du code de test échouera.

```c
/*
 keyestudio Robot Car v2.0
 lesson 14.2
 voiture bluetooth
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
#define SCL_Pin  A5  //Définir la broche d'horloge à A5
#define SDA_Pin  A4  //Définir la broche de données à A4

#define ML_Ctrl 13  //définir la broche de contrôle de direction du moteur gauche
#define ML_PWM 11   //définir la broche de contrôle PWM du moteur gauche
#define MR_Ctrl 12  //définir la broche de contrôle de direction du moteur droit
#define MR_PWM 3    //définir la broche de contrôle PWM du moteur droit

char bluetooth_val; //enregistrer la valeur reçue par Bluetooth

void setup(){
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

void loop(){
  if (Serial.available())
  {
    bluetooth_val = Serial.read();
    Serial.println(bluetooth_val);
  }
  switch (bluetooth_val) 
  {
     case 'F':  //commande d'avance
        Car_front();
        matrix_display(front);  //afficher le motif d'avance
        break;
     case 'B':  //commande de recul
        Car_back();
        matrix_display(back);  //afficher le motif de recul
        break;
     case 'L':  //commande de virage à gauche
        Car_left();
        matrix_display(left);  //afficher le signe de virage à gauche
        break;
     case 'R':  //commande de virage à droite
        Car_right();
        matrix_display(right);  //afficher le signe de virage à droite
       break;
     case 'S':  //commande d'arrêt
        Car_Stop();
        matrix_display(STOP01);  //afficher l'image d'arrêt
        break;
  }
}

/**************Fonction du panneau de points**************/
//cette fonction est utilisée pour l'affichage du panneau de points
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
  IIC_send(0x8A);  //contrôle d'affichage, définir la largeur d'impulsion à 4/16
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
      send_data = send_data >> 1;  //Détecter bit par bit, donc décaler les données vers la droite d'un
  }
}
//Le signe que la transmission de données est terminée
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
  //****************************************************************
```

**Résultat du Test**

Après avoir téléchargé le code avec succès, réglez le commutateur DIP à l'extrémité droite et mettez sous tension. Après la connexion Bluetooth, vous pouvez contrôler la voiture intelligente pour se déplacer via l'application Bluetooth.

Appuyez sur ![](media/image-20250908174053007.png), le robot tank avance ;

cliquez sur ![](media/image-20250908174111419.png), la voiture intelligente recule ;

appuyez sur le bouton ![](media/image-20250908174138113.png), le robot tank tourne à gauche ; cliquez sur ![](media/image-20250908174152325.png), le robot tourne à droite ;

maintenez ![](media/image-20250908174213256.png), il s'arrête.

Cliquez sur ![](media/image-20250908174235827.png) pour activer le contrôle gravitationnel, appuyez sur ![](media/image-20250908174249747.png) à nouveau pour désactiver le contrôle gravitationnel. En même temps, le panneau LED 8X16 sur la voiture robot affiche le motif correspondant.
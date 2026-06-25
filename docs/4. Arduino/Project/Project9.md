# Projet 9 Panneau LED 8*16

**Description**

![](media/image-20250908165452592.png)

Si vous ajoutez un panneau LED 8\*16 au robot, ce sera impressionnant. La matrice de points 8\*16 de Keyestudio peut répondre à cette exigence. Grâce à elle, vous pouvez créer des émoticônes faciales, des motifs ou d'autres affichages intéressants par vous-même. Ce panneau lumineux LED 8\*16 est équipé de 128 LED. Les données du microprocesseur (Arduino) communiquent avec l'AiP1640 via l'interface de bus à deux fils, afin de contrôler les 128 LED du module, qui produisent les motifs dont vous avez besoin sur la matrice de points. Pour faciliter le câblage, un câblage HX-2.54 4 broches est fourni.

**Spécifications**

- Tension de fonctionnement : CC 3,3-5V
- Perte de puissance : 400mW
- Fréquence d'oscillation : 450KHz
- Courant de commande : 200mA
- Température de fonctionnement : -40\~80℃
- Méthode de communication : bus à deux fils

**Composants**

![](media/image-20250908165719665.png)

**Affichage matriciel de points 8*16**

Schéma de circuit

![](media/image-20250908165746529.png)

**Le principe de la matrice de points 8\*16 :**

Comment contrôler chaque LED de la matrice de points 8\*16 ? Nous savons qu'un octet a 8 bits, et chaque bit est 0 ou 1. Quand un bit est 0, la LED s'éteint et quand un bit est 1, la LED s'allume. Ainsi, un octet peut contrôler les LED d'une ligne de la matrice de points, donc 16 octets peuvent contrôler 16 colonnes de LED, c'est-à-dire une matrice de points 8\*16.

**Description de l'interface et protocole de communication :**

Les données du microprocesseur (Arduino) communiquent avec l'AiP1640 via l'interface de bus à deux fils.

Le diagramme du protocole de communication est présenté ci-dessous :

(SCLK) est SCL, (DIN) est SDA :

![](media/image-20250908165823763.png)

①La condition de démarrage pour l'entrée de données : SCL est au niveau haut et SDA passe du niveau haut au niveau bas.

②Pour le paramétrage de la commande de données, il existe des méthodes comme indiqué dans la figure ci-dessous :

Dans notre programme exemple, sélectionnez la méthode **ajouter 1 à l'adresse automatiquement**, la valeur binaire est 0100 0000 et la valeur hexadécimale correspondante est 0x40.

![](media/image-20250908165925549.png)

③Pour le paramétrage de la commande d'adresse, l'adresse peut être sélectionnée comme indiqué ci-dessous.

Le premier 00H est sélectionné dans notre programme exemple, et le nombre binaire 1100 0000 correspond à l'hexadécimal 0xc0.

![](media/image-20250908165938702.png)

④L'exigence pour l'entrée de données est que SCL soit au niveau haut lors de l'entrée de données, et le signal sur SDA doit rester inchangé. Ce n'est que lorsque le signal d'horloge sur SCL est au niveau bas que le signal sur SDA peut être modifié. L'entrée de données est d'abord le poids faible, puis le poids fort.

⑤ La condition pour terminer la transmission de données est que lorsque SCL est bas, SDA est bas, et lorsque SCL est haut, le niveau SDA devient également haut.

⑥ Contrôle d'affichage, définissez différentes largeurs d'impulsion, la largeur d'impulsion peut être sélectionnée comme indiqué ci-dessous.

Dans cet exemple, nous choisissons une largeur d'impulsion de 4/16, et l'hexadécimal correspondant à 1000 1010 est 0x8A.

![](media/image-20250908170005446.png)

4\. Introduction de l'outil de modulation

La version en ligne de l'outil de modulation de matrice de points :

[http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)

①Ouvrez les liens pour accéder à la page suivante.

![](media/image-20250908170027144.png)

②La matrice de points est 8\*16 dans ce projet, donc définissez la hauteur à 8, la largeur à 16, comme indiqué ci-dessous.

![](media/image-20250908170040925.png)

③ Générer des données hexadécimales à partir du motif

Comme indiqué ci-dessous, appuyez sur le bouton gauche de la souris pour sélectionner, le bouton droit pour annuler, dessinez le motif que vous souhaitez, cliquez sur **Générer**, et les données hexadécimales dont nous avons besoin seront produites.

![](media/image-20250908170144895.png)

**Schéma de connexion**

![](media/image-20250908170158402.png)

Remarque sur le câblage : Les GND, VCC, SDA et SCL du panneau LED 8x16 sont respectivement connectés à -(GND), + (VCC), A4 et A5 de la carte d'extension de capteur keyestudio pour la communication série à deux fils. (Remarque : Cette broche est connectée à l'IIC Arduino, mais ce module n'est pas une communication IIC. Il peut être lié avec n'importe quelles deux broches.)

**Code de test**

Le code qui affiche un visage souriant.

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 9.1
 Visage matriciel
 http://www.keyestudio.com
*/ 
// Les données du visage souriant proviennent de l'outil de modulation
unsigned char smile[] = {0x00, 0x00, 0x1c, 0x02, 0x02, 0x02, 0x5c, 0x40, 0x40, 0x5c, 0x02, 0x02, 0x02, 0x1c, 0x00, 0x00};

#define SCL_Pin  A5  // Définir la broche d'horloge à A5
#define SDA_Pin  A4  // Définir la broche de données à A4

void setup()
{
  // Définir la broche en sortie
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // Effacer l'affichage
  // matrix_display(clear);
}

void loop()
{
  matrix_display(smile);  // Afficher le visage souriant
}
// La fonction pour l'affichage de la matrice de points

void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // Utiliser la fonction de la condition de démarrage de transmission de données
  IIC_send(0xc0);  // Sélectionner l'adresse
  
  for(int i = 0;i < 16;i++) // Les données de motif ont 16 bits
  {
     IIC_send(matrix_value[i]); // Transmettre les données de motif
  }

  IIC_end();   // Terminer la transmission des données de motif  
  IIC_start();
  IIC_send(0x8A);  // Contrôle d'affichage, définir la largeur d'impulsion à 4/16 s
  IIC_end();
}

// La condition pour commencer à transmettre des données
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Transmettre des données
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Chaque octet a 8 bits
  {
      digitalWrite(SCL_Pin,LOW);  // Tirer vers le bas la broche d'horloge SCL_Pin pour modifier le signal de SDA
      delayMicroseconds(3);
      if(send_data & 0x01)  // Définir le niveau haut et bas de SDA_Pin selon 1 ou 0 de chaque bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // Tirer vers le haut la broche d'horloge SCL_Pin pour arrêter la transmission
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Détecter bit par bit, décaler les données vers la droite d'un
  }
}

// Le signe de la fin de la transmission de données
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
//******************************************************
```

**Résultat du test**

Câblez selon le schéma de connexion. Le commutateur DIP est basculé vers l'extrémité droite et l'alimentation est activée, le visage souriant apparaît sur la matrice de points.

![](media/image-20250908170421220.png)

**Pratique d'extension**

Nous utilisons l'outil de modulation ([http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)) pour faire afficher alternativement à la matrice de points les motifs d'avance, d'arrêt et d'effacement, et l'intervalle de temps est de 2000 millisecondes.

![](media/image-20250908170445861.png)

![](media/image-20250908170452897.png)

![](media/image-20250908170459213.png)

Obtenez le code graphique à afficher via l'outil de modulation.

**Démarrage :** 0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01

**Avancer :** 0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Reculer :** 0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Tourner à gauche :** 0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00

**Tourner à droite :** 0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00

**Arrêt :** 0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00

**Code pour effacer l'affichage** : 0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00

Le code pour le décalage de plusieurs motifs :

```c
/* keyestudio Mini Tank Robot V2.1
 leçon 9.2
 Boucle matricielle
 http://www.keyestudio.com
*/ 
// Tableau, utilisé pour stocker les données du motif, peut être calculé par vous-même ou obtenu à partir de l'outil de modulation
unsigned char start01[] = 
{0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = 
{0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = 
{0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Définir la broche d'horloge à A5
#define SDA_Pin  A4  // Définir la broche de données à A4
void setup(){
  // Définir les broches en sortie
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // Effacer l'affichage
  matrix_display(clear);
}
void loop(){
  matrix_display(start01);  // Afficher le motif de démarrage
  delay(2000);
  matrix_display(front);    // Motif d'avance
  delay(2000);
  matrix_display(STOP01);   // Motif d'arrêt
  delay(2000);
  matrix_display(clear);    // Effacer l'affichage Effacer l'écran
  delay(2000);
}
// Cette fonction est utilisée pour l'affichage de la matrice de points
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // Appeler la fonction de démarrage de transmission de données  
  IIC_send(0xc0);  // Choisir l'adresse
  
  for(int i = 0;i < 16;i++) // Les données de motif ont 16 bits
  {
     IIC_send(matrix_value[i]); // Données pour transmettre les motifs 
  }
  IIC_end();   // Terminer la transmission des données de motif Fin
  IIC_start();
  IIC_send(0x8A);  // Contrôle d'affichage, définir la largeur d'impulsion à 4/16
  IIC_end();
}
// La condition pour commencer à transmettre des données
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Transmettre des données
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Chaque octet a 8 bits
  {
      digitalWrite(SCL_Pin,LOW);  // Tirer vers le bas la broche d'horloge SCL Pin pour modifier les signaux de SDA      
delayMicroseconds(3);
      if(send_data & 0x01)  // Définir le niveau haut et bas de SDA_Pin selon 1 ou 0 de chaque bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // Tirer vers le haut la broche d'horloge SCL_Pin pour arrêter la transmission de données
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Détecter bit par bit, donc décaler les données vers la droite d'un
  }}
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
  delayMicroseconds(3);} 
//*****************************************************
```

Téléchargez le code sur la carte de développement, la matrice de points 8\*16 affiche alternativement les motifs d'avance, de recul et d'arrêt.

![](media/image-20250908170902116.png)

![](media/image-20250908170913925.png)

![](media/image-20250908170921862.png)
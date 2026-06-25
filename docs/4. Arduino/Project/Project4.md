# Projet 4 Contrôle du Servo-moteur

![](media/image-20250908152354194.png)

**Description**

Le servo-moteur est un actionneur rotatif à contrôle de position. Il se compose principalement d'un boîtier, d'une carte de circuit imprimé, d'un moteur sans noyau, d'un engrenage et d'un capteur de position. Son principe de fonctionnement est le suivant : le servo reçoit le signal envoyé par les microcontrôleurs ou les récepteurs et produit un signal de référence avec une période de 20 ms et une largeur de 1,5 ms, puis compare la tension de polarisation CC acquise à la tension du potentiomètre pour obtenir une sortie de différence de tension.

Lorsque la vitesse du moteur est constante, le potentiomètre est entraîné en rotation par le réducteur en cascade, ce qui fait que la différence de tension est de 0 et que le moteur s'arrête de tourner. En général, la plage d'angle de rotation du servo est de 0° à 180°.

L'angle de rotation du servo-moteur est contrôlé en réglant le rapport cyclique du signal PWM (Modulation de Largeur d'Impulsion). La période standard du signal PWM est de 20 ms (50 Hz). Théoriquement, la largeur est distribuée entre 1 ms et 2 ms, mais en pratique, elle est comprise entre 0,5 ms et 2,5 ms. La largeur correspond à l'angle de rotation de 0° à 180°. Notez cependant que pour des moteurs de marques différentes, le même signal peut produire des angles de rotation différents.

![](media/image-20250908152510007.png)

En général, le servo possède trois fils de couleur marron, rouge et orange. Le fil marron est la masse, le rouge est le fil d'alimentation positive et l'orange est le fil de signal.

![](media/image-20250908152525491.png)

Les angles correspondants du servo sont indiqués ci-dessous :

![](media/image-20250908152558682.png)

**Spécifications**

- Tension de fonctionnement : DC 4,8 V \~ 6 V
- Plage d'angle de fonctionnement : environ 180° (à 500 → 2500 μsec)
- Plage de largeur d'impulsion : 500 → 2500 μsec
- Vitesse à vide : 0,12 ± 0,01 sec / 60 (DC 4,8 V) 0,1 ± 0,01 sec / 60 (DC 6 V)
- Courant à vide : 200 ± 20 mA (DC 4,8 V) 220 ± 20 mA (DC 6 V)
- Couple de blocage : 1,3 ± 0,01 kg · cm (DC 4,8 V) 1,5 ± 0,1 kg · cm (DC 6 V)
- Courant de blocage : ≦ 850 mA (DC 4,8 V) ≦ 1000 mA (DC 6 V)
- Courant en veille : 3 ± 1 mA (DC 4,8 V) 4 ± 1 mA (DC 6 V)

**Composants**

![](media/image-20250908152809268.png)

**Schéma de connexion :**

![](media/image-20250908152824930.png)**Notes de câblage :** le fil marron du servo est relié à Gnd(G), le fil rouge est connecté à 5v(V) et le fil orange est branché sur la broche numérique 9.

Le servo doit être connecté à une alimentation externe en raison de sa forte demande en courant. En général, le courant fourni par la carte de développement n'est pas suffisant. Sans alimentation externe connectée, la carte de développement pourrait être endommagée.

**Code de test 1**

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 4.1
Servo
http://www.keyestudio.com
*/
#define servoPin 9  //broche du servo
int pos; //variable d'angle du servo
int pulsewidth; // variable de largeur d'impulsion du servo

void setup() 
{
  pinMode(servoPin, OUTPUT);  //définir la broche du servo en SORTIE
  procedure(0); //régler l'angle du servo à 0°
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1) // va de 0 degré à 180 degrés
  { 
    // par pas de 1 degré
    procedure(pos);              // demander au servo d'aller à la position dans la variable 'pos'
    delay(15);                   //contrôler la vitesse de rotation du servo
  }
  for (pos = 180; pos >= 0; pos -= 1) // va de 180 degrés à 0 degré
  { 
    procedure(pos);              // demander au servo d'aller à la position dans la variable 'pos'
    delay(15);                    
  }
}
// fonction pour contrôler le servo
void procedure(int myangle) 
{
  pulsewidth = myangle * 11 + 500;  //calculer la valeur de la largeur d'impulsion
  digitalWrite(servoPin,HIGH);
  delayMicroseconds(pulsewidth);   //La durée du niveau haut correspond à la largeur d'impulsion
  digitalWrite(servoPin,LOW);
  delay((20 - pulsewidth / 1000));  // le cycle est de 20ms, le niveau bas dure le reste du temps
}
```

Une fois le code téléversé avec succès, le servo oscille dans la plage de 0° à 180°.

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 4.2
 servo
 http://www.keyestudio.com
*/
#include <Servo.h>
Servo myservo;  // créer un objet servo pour contrôler un servo-moteur
// douze objets servo peuvent être créés sur la plupart des cartes
int pos = 0;    // variable pour stocker la position du servo

void setup() 
{
  myservo.attach(9);  // associe le servo sur la broche 9 à l'objet servo
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1)  // va de 0 degré à 180 degrés
  {
    // par pas de 1 degré
    myservo.write(pos);              // demander au servo d'aller à la position dans la variable 'pos'
    delay(15);                       // attend 15ms pour que le servo atteigne la position
  }
  for (pos = 180; pos >= 0; pos -= 1) { // va de 180 degrés à 0 degré
    myservo.write(pos);              // demander au servo d'aller à la position dans la variable 'pos'
    delay(15);                       // attend 15ms pour que le servo atteigne la position
  }
}
```

**Résultat du test**

Une fois le code téléversé avec succès et l'alimentation activée, le servo oscille dans la plage de 0° à 180°.

Le résultat est identique. Nous le contrôlons habituellement via un fichier de bibliothèque.

**Explication du code**

Arduino est livré avec **\#include \<Servo.h\>** (fonctions et instructions du servo)

Voici quelques instructions courantes de la fonction servo :

1. attach（interface）——Définit l'interface du servo ; les ports 9 et 10 sont disponibles.
2. write（angle）——L'instruction pour définir l'angle de rotation du servo.
3. read（）——L'instruction pour lire l'angle du servo ; lit la valeur de commande de « write() ».
4. **Remarque :** Le format d'écriture ci-dessus est « nom de la variable servo, instruction spécifique（）», par exemple : myservo.attach(9).
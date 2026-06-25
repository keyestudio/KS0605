# Projet 5 Capteur Ultrasonique

**Description**

![](media/image-20250908154003868.png)

Le capteur ultrasonique HC-SR04 utilise le sonar pour déterminer la distance d'un objet, à la manière des chauves-souris. Il offre une excellente détection de distance sans contact, avec une haute précision et des lectures stables, dans un boîtier facile à utiliser. Il est livré complet avec des modules émetteur et récepteur ultrasoniques.

Le HC-SR04 ou capteur ultrasonique est utilisé dans une large gamme de projets électroniques pour créer des applications de détection d'obstacles et de mesure de distance, ainsi que diverses autres applications. Nous présentons ici une méthode simple pour mesurer la distance avec Arduino et un capteur ultrasonique, ainsi que la façon d'utiliser le capteur ultrasonique avec Arduino.

**Spécifications**

![](media/image-20250908154036832.png)

- Alimentation : +5V DC
- Courant de repos : \<2mA
- Courant de fonctionnement : 15mA
- Angle effectif : \<15°
- Distance de mesure : 2cm – 400 cm
- Résolution : 0.3 cm
- Angle de mesure : 30 degrés
- Largeur d'impulsion d'entrée du déclencheur : 10uS

**Composants**

![](media/image-20250908154147825.png)

**Principe du capteur ultrasonique**

Comme le montre l'image ci-dessus, il ressemble à deux yeux. L'un est l'extrémité émettrice, l'autre est l'extrémité réceptrice.

Le module ultrasonique émet des ondes ultrasoniques après le déclenchement d'un signal. Lorsque les ondes ultrasoniques rencontrent un objet et sont réfléchies, le module génère un signal d'écho, ce qui lui permet de déterminer la distance de l'objet à partir de la différence de temps entre le signal de déclenchement et le signal d'écho.

Le temps t est le temps que met le signal émis pour rencontrer l'obstacle et revenir. La vitesse de propagation du son dans l'air est d'environ 343m/s, et distance = vitesse \* temps. Cependant, l'onde ultrasonique est émise puis revient, ce qui représente 2 fois la distance. Il faut donc diviser par 2 : la distance mesurée par l'onde ultrasonique = (vitesse \* temps)/2.

1. Méthode d'utilisation et chronogramme du module ultrasonique :

2. Régler le délai de la broche Trig du SR04 à au moins 10μs, ce qui permet de déclencher la détection de distance.
3. Après le déclenchement, le module envoie automatiquement huit impulsions ultrasoniques à 40KHz et détecte si un signal revient. Cette étape est effectuée automatiquement par le module.
4. Si le signal revient, la broche Echo génère un niveau haut, et la durée de ce niveau haut correspond au temps écoulé entre la transmission de l'onde ultrasonique et son retour.

![](media/image-20250908154407063.png)

Schéma du circuit du capteur ultrasonique :

![](media/image-20250908154422828.png)

**Schéma de connexion**

![](media/image-20250908154455132.png)

Guide de câblage :

- Capteur ultrasonique → Bouclier de capteur keyestudio V5
- VCC → 5v(V)
- Trig → 5(S)
- Echo → 4(S)
- Gnd → Gnd(G)

**Code de test**

```c
/*
keyestudio Mini Tank Robot V2.1
leçon 5
Capteur ultrasonique
http://www.keyestudio.com
*/
int trigPin = 5; // Déclencheur
int echoPin = 4; // Écho
long duration, cm, inches;

void setup() 
{
    // Initialisation du port série
    Serial.begin (9600);
    // Définition des entrées et sorties
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);
}
void loop() 
{
    // Le capteur est déclenché par une impulsion HIGH de 10 microsecondes ou plus.
    // Envoyer une courte impulsion LOW au préalable pour garantir une impulsion HIGH propre :
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    // Lire le signal du capteur : une impulsion HIGH dont la durée est le temps (en microsecondes) entre l'envoi du ping et la réception de son écho sur un objet.
    duration = pulseIn(echoPin, HIGH);
    // Convertir le temps en distance
    cm = (duration/2) / 29.1; // Diviser par 29.1 ou multiplier par 0.0343
    inches = (duration/2) / 74; // Diviser par 74 ou multiplier par 0.0135
    Serial.print(inches);
    Serial.print("in, ");
    Serial.print(cm);
    Serial.print("cm");
    Serial.println();
    delay(250);
}
```

**Résultat du test**

Téléversez le code de test sur la carte de développement, ouvrez le moniteur série et réglez le débit en bauds sur 9600. La distance détectée s'affiche en cm et en pouces. En plaçant la main devant le capteur ultrasonique, la valeur de distance affichée diminue.

![](media/image-20250908154718663.png)

**Explication du code**

**int trigPin = 5 ;** cette broche est définie pour émettre des ondes ultrasoniques, généralement en sortie.

**int echoPin = 4 ;** cette broche est définie pour la réception, généralement en entrée.

**cm = (duration/2) / 29.1 ; inches = (duration/2) / 74 ; par 0.0135**

Nous pouvons calculer la distance à l'aide de la formule suivante :

distance = (temps de trajet/2) x vitesse du son

La vitesse du son est : 343m/s = 0.0343 cm/uS = 1/29.1 cm/uS

Ou en pouces : 13503.9in/s = 0.0135in/uS = 1/74in/uS

Nous devons diviser le temps de trajet par 2 car il faut tenir compte du fait que l'onde a été envoyée, a frappé l'objet, puis est revenue vers le capteur.

**Pratique avancée :**

Nous avons mesuré la distance affichée par l'ultrason. Que diriez-vous de contrôler une LED avec la distance mesurée ? Essayons et connectons un module LED à la broche D10.

![](media/image-20250908154848028.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 5
 LED ultrasonique
 http://www.keyestudio.com
*/ 
int trigPin = 5;    // Déclencheur
int echoPin = 4;    // Écho
long duration, cm, inches;

void setup() 
{
  // Initialisation du port série
  Serial.begin (9600);
  // Définition des entrées et sorties
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() 
{
  // Le capteur est déclenché par une impulsion HIGH de 10 microsecondes ou plus.
  // Envoyer une courte impulsion LOW au préalable pour garantir une impulsion HIGH propre :
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Lire le signal du capteur : une impulsion HIGH dont la durée est le temps (en microsecondes) entre l'envoi du ping et la réception de son écho sur un objet.
  duration = pulseIn(echoPin, HIGH);
  // Convertir le temps en distance
  cm = (duration/2) / 29.1;     // Diviser par 29.1 ou multiplier par 0.0343
  inches = (duration/2) / 74;   // Diviser par 74 ou multiplier par 0.0135
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  Serial.print("cm");
  Serial.println();
  delay(250);
  if (cm>=2 && cm<=10)
  	digitalWrite(10, HIGH);
  delay(1000);
  digitalWrite(10, LOW);
  delay(1000);
}
```

Téléversez le code de test sur la carte de développement et placez la main devant le capteur ultrasonique, puis vérifiez si la LED s'allume.
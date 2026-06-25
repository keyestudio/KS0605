# Projet 3 Capteur Photorésistance

![](./media/image-20250902173047302.png)

 **Description**

La photorésistance est une résistance spéciale fabriquée à partir de matériaux semi-conducteurs tels que le CdS ou le sélénium. La surface est également revêtue d'une résine imperméable, qui possède un effet photoconducteur. Elle est sensible à la lumière ambiante. Sa résistance varie en fonction de différentes intensités lumineuses.

Nous utilisons les caractéristiques de la photorésistance pour concevoir le circuit et générer le module de photorésistance.

En connectant la broche de signal du module photocellule au port analogique, vous constaterez que plus l'intensité lumineuse est forte, plus la tension du port analogique est élevée, et plus la valeur analogique est grande.

À l'inverse, plus l'intensité lumineuse est faible, plus la tension du port analogique est faible, et plus la valeur analogique est petite.

Sur la base de cela, nous pouvons utiliser le module photocellule pour lire la valeur analogique et ainsi obtenir l'intensité de la lumière ambiante.

 **Spécifications**

![](./media/image-20250902173349950.png)

- Résistance : 5K ohm - 0,5 Mohm
- Type d'interface : analogique
- Tension de fonctionnement : 3,3V - 5V
- Installation facile : avec trous de fixation à vis
- Espacement des broches : 2,54 mm

 **Composants**

![](./media/image-20250902173528860.png)

 **Schéma de connexion :**

![](./media/image-20250902173558747.png)

Les deux capteurs photorésistance sont reliés aux ports A1 et A2. Nous terminerons l'expérience via la photorésistance connectée à A1. Lisons sa valeur analogique.

**Code de test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 3.1
 photocellule
 http://www.keyestudio.com
*/

int sensorPin = A1;    // sélectionner la broche d'entrée pour la photocellule
int sensorValue = 0;  // variable pour stocker la valeur provenant du capteur
void setup() 
{
	Serial.begin(9600);
}

void loop() 
{
    sensorValue = analogRead(sensorPin);  // lire la valeur du capteur
    Serial.println(sensorValue);  // le port série affiche la valeur de résistance
    delay(500);
}
//******************************************************
```

 **Résultat du test**

Téléchargez le code sur la carte de développement, ouvrez le moniteur série et vérifiez si sa valeur diminue lorsque vous couvrez la photorésistance. Cependant, la valeur augmente lorsqu'elle est découverte.

![](./media/image-20250902174159923.png)

**Explication du code**

**analogRead(sensorPin) :** lire la valeur analogique de la photorésistance via les ports analogiques.

**Serial.begin(9600) :** initialiser le port série, la vitesse de transmission en bauds de la communication série est 9600.

**Serial.println :** le port série affiche et fait un retour à la ligne.

**Pratique d'extension**

Nous avons appris à lire la valeur de la photorésistance. Combinons maintenant la photorésistance avec une LED et observons l'état de la LED.

![](./media/image-20250902174256941.png)

La PWM contrôle la luminosité, donc la LED est reliée aux broches PWM. Connectez la LED à la broche 10, gardez la broche de la photorésistance inchangée, puis concevez le code :

```c
/*keyestudio Mini Tank Robot V2.1
leçon 3.2
photocellule - sortie analogique
http://www.keyestudio.com
*/
int analogInPin = A1;  // broche d'entrée analogique à laquelle la photocellule est connectée
int analogOutPin = 10; // broche de sortie analogique à laquelle la LED est connectée
int sensorValue = 0;        // valeur lue du capteur
int outputValue = 0;        // valeur envoyée à la PWM (sortie analogique)

void setup() 
{
	Serial.begin(9600);
 }
void loop() 
{
  // lire la valeur d'entrée analogique :
  sensorValue = analogRead(analogInPin);
  // la mapper à la plage de la sortie analogique :
  outputValue = map(sensorValue, 0, 1023, 0, 255);
  // modifier la valeur de sortie analogique :
  analogWrite(analogOutPin, outputValue);
  // attendre 2 millisecondes avant la boucle suivante pour que le convertisseur
  // analogique-numérique se stabilise après la dernière lecture :
 Serial.println(sensorValue);  // le port série affiche la valeur de la photorésistance
 delay(2);
}
//***************************************************************
```

Téléchargez le code et appuyez dessus avec votre main pour observer la luminosité de la LED.
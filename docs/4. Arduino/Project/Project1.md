# Projet 1 LED Clignote

![](media/image-20250908174750401.png)

**Description**

Pour les débutants et les passionnés, le clignotement LED est un programme fondamental. LED, l'abréviation de diodes électroluminescentes, est composée de composés chimiques Ga, As, P, N, etc. La LED peut clignoter dans diverses couleurs en modifiant le temps de délai dans le code de test. Lors de la commande, en mettant sous tension GND et VCC, la LED s'allume si la broche S est au niveau haut ; néanmoins, elle s'éteindra.

**Spécification**

![](./media/image-20250902164418568.png)

- Interface de contrôle : port numérique
- Tension de fonctionnement : DC 3.3-5V
- Espacement des broches : 2.54mm
- Couleur d'affichage LED : rouge

**Composants**

![](./media/image-20250902164804229.png)

**Bouclier de capteur V5**

Il serait fastidieux de combiner les cartes de développement Arduino avec de nombreux capteurs. Cependant, le bouclier de capteur V5, compatible avec la carte de développement Arduino, résout parfaitement ce problème. Il suffit de l'empiler dessus.

Ce bouclier de capteur peut être inséré dans des modules de capteur 3 broches et expose certaines broches de communication, comme la communication série, IIC et SPI.

**Description des broches**

![](./media/image-20250902165027854.png)

**Schéma de connexion**

![](./media/image-20250902165110913.png)

Selon le schéma ci-dessus, la LED est connectée à D2.

**Code de test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 1.1
 Clignotement
 http://www.keyestudio.com
*/
void setup()
{ 
    pinMode(2, OUTPUT);// initialiser la broche numérique 2 comme sortie.
}

void loop() // la fonction loop s'exécute indéfiniment
{
   digitalWrite(2, HIGH); // allumer la LED (HIGH est le niveau de tension)
   delay(1000); // attendre une seconde
   digitalWrite(2, LOW); // éteindre la LED en mettant la tension à LOW
   delay(1000); // attendre une seconde
}
```

**Résultat du test**

(Il y aura une contradiction concernant la communication série entre le code et le Bluetooth lors du téléchargement du code. Par conséquent, ne connectez pas le module Bluetooth avant de télécharger le code.)

Téléchargez le programme sur la carte de développement, la LED clignote à l'intervalle de 1s.

![](./media/image-20250902165335641.png)

**Explication du code**

**pinMode(2，OUTPUT) -** Définir la broche 2 en OUTPUT

**digitalWrite(2，HIGH) -** Lorsque la broche 2 est définie à HIGH (sortie 5V) ou à LOW (sortie 0V)

**Pratique d'extension**

Nous avons réussi à faire clignoter la LED. Ensuite, observons comment la LED changera si nous modifions les broches et le temps de délai.

**Schéma de connexion**

![](./media/image-20250902165631206.png)

Nous avons modifié les broches et connecté la LED à D10.

**Code de test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 1.2
 délai
 http://www.keyestudio.com
*/
void setup() // initialiser la broche numérique 10 comme sortie.
{  
   pinMode(10, OUTPUT);
}

// la fonction loop s'exécute indéfiniment
void loop() 
{
   digitalWrite(10, HIGH); // allumer la LED (HIGH est le niveau de tension)
   delay(100); // attendre 0.1 seconde
   digitalWrite(10, LOW); // éteindre la LED en mettant la tension à LOW
   delay(100); // attendre 0.1 seconde
}
```

Le résultat du test montre que la LED clignote plus rapidement. Par conséquent, nous pouvons conclure que les broches et le délai de temps affectent la fréquence de clignotement.
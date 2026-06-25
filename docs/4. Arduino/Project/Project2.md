# Projet 2 Ajuster la luminosité de la LED

**(1) Description**

Dans la leçon précédente, nous avons contrôlé l'allumage et l'extinction de la LED et l'avons fait clignoter.

Dans ce projet, nous contrôlerons la luminosité de la LED via PWM pour simuler des effets de respiration. De même, vous pouvez modifier la longueur des étapes et le temps de délai dans le code pour démontrer différents effets de respiration.

PWM est un moyen de contrôler la sortie analogique par des moyens numériques. Le contrôle numérique est utilisé pour générer des ondes carrées avec différents rapports cycliques (un signal qui bascule constamment entre les niveaux haut et bas) pour contrôler la sortie analogique. En général, les tensions d'entrée des ports sont 0V et 5V. Que se passe-t-il si 3V est requis ? Ou un commutateur entre 1V, 3V et 3,5V ? Nous ne pouvons pas changer constamment les résistances. Pour cette raison, nous recourons à PWM.

![](./media/bbcfcb9ae56abb7e80ee587246fc4be9.GIF)

Pour la sortie de tension du port numérique Arduino, il n'y a que LOW et HIGH, qui correspondent aux sorties de tension de 0V et 5V. Vous pouvez définir LOW comme 0 et HIGH comme 1, et laisser Arduino générer cinq cents signaux 0 ou 1 en 1 seconde.

Si vous générez cinq cents 1, c'est 5V ; si tous sont 1, c'est 0V. Si vous générez 010101010101 de cette façon, le port de sortie est 2,5V, ce qui est comme regarder un film. Les films que nous regardons ne sont pas complètement continus. Il génère en fait 25 images par seconde. Dans ce cas, l'humain ne peut pas le remarquer, pas plus que PWM. Si vous voulez une tension différente, vous devez contrôler le rapport de 0 et 1. Plus vous générez de signaux 0 et 1 par unité de temps, plus vous contrôlez avec précision.

**(2) Spécifications**

- Interface de contrôle : port numérique
- Tension de fonctionnement : DC 3,3-5V
- Espacement des broches : 2,54mm
- Couleur d'affichage : rouge

**(3) Composants**

![](./media/image-20250902170952089.png)

 **(4) Schéma de connexion**

![](./media/image-20250902171013917.png)

 **(5) Code de test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 2.2
 pwm-slow
 http://www.keyestudio.com
*/
int ledPin = 10; // Définir la broche LED à D10
int value;

void setup () 
{
	pinMode (ledPin, OUTPUT); // initialiser ledpin comme sortie.
}

void loop () 
{
    for (value = 0; value <255; value = value + 1)
    {
        analogWrite (ledPin, value); // La LED s'allume progressivement
        delay (30); // délai 30MS
    }
    for (value = 255; value> 0; value = value-1)
    {
        analogWrite (ledPin, value); // La LED s'éteint progressivement
        delay (30); // délai 30MS
	}
}
```

**Résultat du test**

Après avoir téléchargé le code de test avec succès, la LED change progressivement de lumineux à sombre, comme la respiration humaine, plutôt que de s'allumer et s'éteindre immédiatement.

**Explication du code**

Lorsque nous devons répéter certaines instructions, nous pouvons utiliser l'instruction FOR.

Le format de l'instruction FOR est montré ci-dessous :

![](./media/image-20250902171421873.png)

OU séquence cyclique :

Tour 1 : 1 → 2 → 3 → 4

Tour 2 : 2 → 3 → 4

…

Jusqu'à ce que le numéro 2 ne soit pas établi, la boucle « for » est terminée,

Après avoir connu cet ordre, revenez au code :

**for (int value = 0; value < 255; value=value+1){**

**...**

**}**

**for (int value = 255; value >0; value=value-1){**

**...**

**}**

Les deux instructions « for » font augmenter la valeur de 0 à 255, puis diminuer de 255 à 0, puis augmenter à 255, .... boucle infinie.

Il y a une nouvelle fonction dans ce qui suit ----- analogWrite()

Nous savons que le port numérique n'a que deux états : 0 et 1. Alors comment envoyer une valeur analogique à une valeur numérique ? Ici, cette fonction est nécessaire. Observons la carte Arduino et trouvons 6 broches marquées « \~ » qui peuvent générer des signaux PWM.

**Le format de la fonction est le suivant :**

**analogWrite(pin,value)**

analogWrite() est utilisé pour écrire une valeur analogique de 0 à 255 pour le port PWM, donc la valeur est dans la plage de 0 à 255. Attention, vous ne devez écrire que les broches numériques avec la fonction PWM, telles que les broches 3, 5, 6, 9, 10, 11.

PWM est une technologie pour obtenir une quantité analogique par une méthode numérique. Le contrôle numérique forme une onde carrée, et le signal d'onde carrée n'a que deux états : l'activation et la désactivation (c'est-à-dire les niveaux haut ou bas). En contrôlant le rapport de la durée d'activation et de désactivation, une tension variant de 0 à 5V peut être simulée. Le temps d'activation (académiquement appelé niveau haut) s'appelle largeur d'impulsion, donc PWM s'appelle aussi modulation de largeur d'impulsion.

À travers les cinq ondes carrées suivantes, apprenons-en plus sur PWM.

![](./media/image-20250902172304373.png)

Dans la figure ci-dessus, la ligne verte représente une période, et la valeur de analogWrite() correspond à un pourcentage appelé Duty Cycle (rapport cyclique).

Le rapport cyclique implique que la durée du niveau haut est divisée par la durée du niveau bas dans un cycle. De haut en bas, le rapport cyclique de la première onde carrée est 0% et sa valeur correspondante est 0. La luminosité de la LED est la plus faible, c'est-à-dire l'extinction. Plus le niveau haut dure longtemps, plus la LED est brillante. Par conséquent, le dernier rapport cyclique est 100%, ce qui correspond à 255, la LED est la plus brillante. 25% signifie plus sombre.

PWM est principalement utilisé pour ajuster la luminosité de la LED ou la vitesse de rotation du moteur.

Il joue un rôle vital dans le contrôle de la voiture robot intelligente. Je crois que vous avez hâte d'entrer dans le prochain projet.
# Projet 6 Réception IR

**Description**

Il ne fait aucun doute que la télécommande infrarouge est omniprésente dans la vie quotidienne. Elle est utilisée pour contrôler diverses appareils ménagers, tels que les téléviseurs, les chaînes stéréo, les magnétoscopes et les récepteurs de signaux satellites. La télécommande infrarouge est composée de systèmes de transmission et de réception infrarouge, c'est-à-dire une télécommande infrarouge et un module de réception infrarouge ainsi qu'un microcontrôleur monocip capable de décoder.

![](media/image-20250908155801467.png)

Le signal porteur infrarouge 38K émis par la télécommande est codé par la puce de codage dans la télécommande. Il se compose d'une section de code pilote, de code utilisateur, de code inverse utilisateur, de code de données et de code inverse de données. L'intervalle de temps de l'impulsion est utilisé pour distinguer s'il s'agit d'un signal 0 ou 1 et le codage est composé de ces signaux 0, 1.

Le code utilisateur de la même télécommande reste inchangé tandis que le code de données peut distinguer la touche.

Lorsque le bouton de la télécommande est enfoncé, la télécommande envoie un signal porteur infrarouge. Lorsque le récepteur IR reçoit le signal, le programme décode le signal porteur et détermine quelle touche est enfoncée. Le MCU décode le signal 01 reçu, déterminant ainsi quelle touche est enfoncée par la télécommande.

Le récepteur infrarouge que nous utilisons est un module récepteur infrarouge. Composé principalement d'une tête réceptrice infrarouge, c'est un appareil qui intègre la réception, l'amplification et la démodulation. Son IC interne a complété la démodulation et peut réaliser la réception infrarouge à la sortie et être compatible avec les signaux TTL.

De plus, il convient à la télécommande infrarouge et à la transmission de données infrarouge. Le module de réception infrarouge fabriqué par le récepteur n'a que trois broches, la ligne de signal, VCC et GND. Il est très pratique de communiquer avec Arduino et d'autres microcontrôleurs.

**Spécification**

![](media/image-20250908160124669.png)

![](media/image-20250908160132699.png)

- Tension de fonctionnement : 3.3-5V（DC）
- Interface : 3PIN
- Signal de sortie : Signal numérique
- Angle de réception : 90 degrés
- Fréquence : 38khz
- Distance de réception : 10m

**Composants**

![](media/image-20250908160309873.png)

**Schéma de connexion**

![](media/image-20250908160331260.png)

Reliez respectivement « - », « + » et S du module récepteur IR avec G(GND）, V（VCC) et A0 de la carte de développement keyestudio.

**Attention :** À condition que les ports numériques ne soient pas disponibles, les ports analogiques peuvent être considérés comme des ports numériques. A0 équivaut à D14, A1 équivaut au port numérique 15.

**Code de test**

Importez d'abord le fichier de bibliothèque du module récepteur IR (consultez comment importer le fichier de bibliothèque Arduino) avant de concevoir le code.

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 6
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>     // Déclaration de la bibliothèque IRremote
int RECV_PIN = A0;        // Définir les broches du récepteur IR comme A0
IRrecv irrecv(RECV_PIN);   
decode_results results;   // Les résultats de décodage existent dans « result » de « decode results »
void setup()  
  {
      Serial.begin(9600);  
      irrecv.enableIRIn(); // Activer le récepteur
  }  
 void loop() {  
    if (irrecv.decode(&results))// Décodage réussi, recevoir un ensemble de signaux infrarouge
    {  
      Serial.println(results.value, HEX);// Envelopper le mot en HEX 16 pour afficher et recevoir le code
      irrecv.resume(); // Recevoir la valeur suivante
    }  
    delay(100);  
  }
```

 **Résultat du test**

Téléchargez le code de test, ouvrez le moniteur série et réglez la vitesse en bauds sur 9600, pointez la télécommande vers le récepteur IR et la valeur correspondante s'affichera. Si vous appuyez longtemps, des codes d'erreur apparaîtront.

![](media/image-20250908160550590.png)

Ci-dessous, nous avons listé la valeur de chaque bouton de la télécommande keyestudio. Vous pouvez la conserver à titre de référence.

![](media/image-20250908160603853.png)

**Explication du code**

**irrecv.enableIRIn() :** Après activation du décodage IR, les signaux IR seront reçus, puis la fonction « decode() » vérifiera continuellement si le décodage est réussi.

**irrecv.decode(&results) :** Après un décodage réussi, cette fonction reviendra à « true » et conservera le résultat dans « results ». Après décodage d'un signal IR, exécutez la fonction resume() et recevez le signal suivant.

**Pratique d'extension**

Nous avons décodé la valeur de touche de la télécommande IR. Que diriez-vous de contrôler la LED par la valeur mesurée ? Nous pourrions effectuer une expérience pour confirmer. Attachez une LED à D10, puis appuyez sur les touches de la télécommande pour allumer et éteindre la LED.

![](media/image-20250908160749345.png)

```c
/* keyestudio Mini Tank Robot V2.1
lesson 6.2
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>
int RECV_PIN = A0;// Définir la broche du récepteur IR comme A0
int LED_PIN=10;// Définir la broche de la LED
int a=0;
IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
  Serial.begin(9600);
  irrecv.enableIRIn(); // Initialiser le récepteur IR
  pinMode(LED_PIN,OUTPUT);// Définir la broche de la LED à 4
}

void loop() 
{
  if (irrecv.decode(&results)) 
  {
	Serial.println(results.value, HEX);// Envelopper le mot en HEX 16 pour afficher et recevoir le code
	if(results.value==0xFF02FD &a==0) // Selon la valeur de touche ci-dessus, appuyez sur « OK » sur la télécommande, la LED sera contrôlée
	{
		digitalWrite(LED_PIN,HIGH);// La LED s'allumera
		a=1;
	}
	else if(results.value==0xFF02FD &a==1) // Appuyez à nouveau
	{
        digitalWrite(LED_PIN,LOW);// La LED s'éteindra
        a=0;	
	}
	irrecv.resume(); // Recevoir la valeur suivante
  }
}
```

Téléchargez le code sur la carte de développement, appuyez sur la touche « OK » de la télécommande pour allumer et éteindre la LED.
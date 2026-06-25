# Projet 8 Commande de Moteur et Contrôle de Vitesse

**Description**

![](media/image-20250908162844748.png)

Il existe de nombreuses façons de commander un moteur. Notre voiture robot utilise la solution la plus courante--L298P--qui est un excellent circuit intégré de commande de moteur haute puissance produit par STMicroelectronics. Il peut commander directement des moteurs CC, des moteurs pas à pas biphasés et quadriphasés. Le courant de commande peut atteindre 2A, et la borne de sortie du moteur utilise huit diodes Schottky haute vitesse comme protection.

Nous avons conçu un shield basé sur le circuit du L298p. La conception empilée réduit la difficulté technique d'utilisation et de commande du moteur.

**Spécifications**

Schéma de Circuit pour la Carte L298P

![](media/image-20250908163017604.png)

1. Tension d'entrée de la partie logique : CC 5V
2. Tension d'entrée de la partie commande : CC 7-12V
3. Courant de fonctionnement de la partie logique : \<36mA
4. Courant de fonctionnement de la partie commande : \<2A
5. Dissipation de puissance maximale : 25W (T=75℃)
6. Température de fonctionnement : -25℃～＋130℃
7. Niveau d'entrée du signal de commande : niveau haut 2.3V\<Vin\<5V, niveau bas\0.3V\<Vin\<1.5V

![](media/image-20250908163151925.png)

**Faire Bouger le Robot**

D'après le schéma de circuit ci-dessus, la broche de direction du moteur A est D12, et la broche de vitesse est D3 ; D13 est la broche de direction du moteur B, D11 est la broche de vitesse.

Nous savons comment contrôler les ports numériques selon le tableau suivant.

PWM décide que 2 moteurs s'activent pour faire fonctionner la voiture robot. La valeur PWM est dans la plage 0-255. Plus le nombre est grand, plus le moteur tourne vite.

| Robot Tank      | Moteur (A)           | Moteur (B)           |
| --------------- | -------------------- | -------------------- |
| Avant           | Tourner dans le sens des aiguilles d'une montre | Tourner dans le sens des aiguilles d'une montre |
| Arrière         | Tourner dans le sens inverse des aiguilles d'une montre | Tourner dans le sens inverse des aiguilles d'une montre |
| Tourner à gauche  | Tourner dans le sens inverse des aiguilles d'une montre | Tourner dans le sens des aiguilles d'une montre |
| Tourner à droite | Tourner dans le sens des aiguilles d'une montre | Tourner dans le sens inverse des aiguilles d'une montre |
| Arrêt           | Arrêt                | Arrêt                |

**Composants**

![](media/image-20250908163739200.png)

**Schéma de Connexion**

![](media/d35ffe6c0c275548f40bcafb42a93da1.jpeg)

**Remarque :** le bloc terminal 4 broches est marqué avec la sérigraphie 1234. Le fil rouge du moteur arrière droit est connecté à la borne 1, le fil noir est relié à la borne 2. Le fil rouge du moteur avant gauche est attaché à la borne 3, le fil noir est relié au port 4.

**Code de Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 8.1
 commande de moteur
 http://www.keyestudio.com
*/ 

#define ML_Ctrl 13  //définir la broche de contrôle de direction du moteur gauche
#define ML_PWM 11   //définir la broche de contrôle PWM du moteur gauche
#define MR_Ctrl 12  //définir la broche de contrôle de direction du moteur droit
#define MR_PWM 3   //définir la broche de contrôle PWM du moteur droit

void setup()
{
  pinMode(ML_Ctrl, OUTPUT);//définir la broche de contrôle de direction du moteur gauche en sortie
  pinMode(ML_PWM, OUTPUT);//définir la broche de contrôle PWM du moteur gauche en sortie
  pinMode(MR_Ctrl, OUTPUT);//définir la broche de contrôle de direction du moteur droit en sortie
  pinMode(MR_PWM, OUTPUT);//définir la broche de contrôle PWM du moteur droit en sortie
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//définir la broche de contrôle de direction du moteur gauche à LOW
  analogWrite(ML_PWM,200);//définir la vitesse de contrôle PWM du moteur gauche à 200
  digitalWrite(MR_Ctrl,LOW);//définir la broche de contrôle de direction du moteur droit à LOW
  analogWrite(MR_PWM,200);//définir la vitesse de contrôle PWM du moteur droit à 200

  //avant
  delay(2000);//délai de 2s
   digitalWrite(ML_Ctrl,HIGH);//définir la broche de contrôle de direction du moteur gauche à HIGH
  analogWrite(ML_PWM,200);//définir la vitesse de contrôle PWM du moteur gauche à 200  
digitalWrite(MR_Ctrl,HIGH);//définir la broche de contrôle de direction du moteur droit à HIGH
  analogWrite(MR_PWM,200);//définir la vitesse de contrôle PWM du moteur droit à 200

   //arrière
  delay(2000);//délai de 2s 
  digitalWrite(ML_Ctrl,HIGH);//définir la broche de contrôle de direction du moteur gauche à HIGH
  analogWrite(ML_PWM,200);//définir la vitesse de contrôle PWM du moteur gauche à 200
  digitalWrite(MR_Ctrl,LOW);//définir la broche de contrôle de direction du moteur droit à LOW
  analogWrite(MR_PWM,200);//définir la vitesse de contrôle PWM du moteur droit à 200

    //gauche
  delay(2000);//délai de 2s
   digitalWrite(ML_Ctrl,LOW);//définir la broche de contrôle de direction du moteur gauche à LOW
  analogWrite(ML_PWM,200);//définir la vitesse de contrôle PWM du moteur gauche à 200
  digitalWrite(MR_Ctrl,HIGH);//définir la broche de contrôle de direction du moteur droit à HIGH
  analogWrite(MR_PWM,200);//définir la vitesse de contrôle PWM du moteur droit à 200

   //droite
  delay(2000);//délai de 2s
  analogWrite(ML_PWM,0);//définir la vitesse de contrôle PWM du moteur gauche à 0
  analogWrite(MR_PWM,0);//définir la vitesse de contrôle PWM du moteur droit à 0

    //arrêt
  delay(2000);//délai de 2s
}//*****************************************
```

**Résultat du Test**

Connectez selon le schéma de connexion, téléchargez le code et mettez sous tension, la voiture intelligente avance et recule pendant 2s, tourne à gauche et à droite pendant 2s, s'arrête pendant 2s et alterne.

**Explication du Code**

**digitalWrite(ML_Ctrl,LOW) :** le sens de rotation du moteur est décidé par le niveau haut/bas et les broches qui décident le sens de rotation sont des broches numériques.

**analogWrite(ML_PWM,200) :** la vitesse du moteur est régulée par PWM, et les broches qui décident la vitesse du moteur doivent être des broches PWM.

**Pratique d'Extension**

Ajustez la vitesse que PWM contrôle le moteur, et connectez de la même manière.

![](media/d35ffe6c0c275548f40bcafb42a93da1-1757321401643-2.jpeg)

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 8.2
 commande de moteur pwm
 http://www.keyestudio.com
*/ 
#define ML_Ctrl 13  //définir la broche de contrôle de direction du moteur gauche
#define ML_PWM 11   //définir la broche de contrôle PWM du moteur gauche
#define MR_Ctrl 12  //définir la broche de contrôle de direction du moteur droit
#define MR_PWM 3   //définir la broche de contrôle PWM du moteur droit

void setup()
{ 
  pinMode(ML_Ctrl, OUTPUT);//définir la broche de contrôle de direction du moteur gauche en OUTPUT
  pinMode(ML_PWM, OUTPUT);//définir la broche de contrôle PWM du moteur gauche en OUTPUT
  pinMode(MR_Ctrl, OUTPUT);//définir la broche de contrôle de direction du moteur droit en OUTPUT
  pinMode(MR_PWM, OUTPUT);//définir la broche de contrôle PWM du moteur droit en OUTPUT
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//Définir la broche de contrôle de direction du moteur gauche à LOW
  analogWrite(ML_PWM,100);//Définir la vitesse de contrôle PWM du moteur gauche à 100
  digitalWrite(MR_Ctrl,LOW);//Définir la broche de contrôle de direction du moteur droit à LOW
  analogWrite(MR_PWM,100);//Définir la vitesse de contrôle PWM du moteur droit à 100
  //avant
  delay(2000);//définir 2s
  digitalWrite(ML_Ctrl,HIGH);//Définir la broche de contrôle de direction du moteur gauche au niveau HIGH
  analogWrite(ML_PWM,250);//Définir la vitesse de contrôle PWM du moteur gauche à 100
  digitalWrite(MR_Ctrl,HIGH);//Définir la broche de contrôle de direction du moteur droit au niveau HIGH
  analogWrite(MR_PWM,250);//Définir la vitesse de contrôle PWM du moteur droit à 100
   //arrière
  delay(2000);//définir 2s
  digitalWrite(ML_Ctrl,HIGH);//Définir la broche de contrôle de direction du moteur gauche au niveau HIGH
  analogWrite(ML_PWM,250);//Définir la vitesse de contrôle PWM du moteur gauche à 100
  digitalWrite(MR_Ctrl,LOW);//Définir la broche de contrôle de direction du moteur droit au niveau LOW
  analogWrite(MR_PWM,250);//Définir la vitesse de contrôle PWM du moteur droit à 100
    //gauche
  delay(2000);//définir 2s
   digitalWrite(ML_Ctrl,LOW);//définir la broche de contrôle de direction du moteur gauche à LOW
  analogWrite(ML_PWM,250);//définir la vitesse de contrôle PWM du moteur gauche à 200
  digitalWrite(MR_Ctrl,HIGH);//définir la broche de contrôle de direction du moteur droit à HIGH
  analogWrite(MR_PWM,250);//définir la vitesse de contrôle PWM du moteur droit à 100
   //droite
  delay(2000);//définir 2s
  analogWrite(ML_PWM,0);//définir la vitesse de contrôle PWM du moteur gauche à 0
  analogWrite(MR_PWM,0);//définir la vitesse de contrôle PWM du moteur droit à 0

    //arrêt
  delay(2000);//définir 2s
}//******************************************************************
```

Téléchargement du code réussi, les moteurs tournent plus vite.
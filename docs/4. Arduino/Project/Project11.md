# Projet 11 Réservoir d'évitement ultrasonique

![](media/image-20250908171729897.png)

**Description**

Dans ce programme, le capteur ultrasonique détecte la distance de l'obstacle pour envoyer des signaux qui contrôlent la voiture robot. Ensuite, nous vous montrerons comment fabriquer une voiture d'évitement d'obstacles.

**La logique spécifique du robot d'évitement ultrasonique est présentée ci-dessous :**

![](media/image-20250908171756879.png)

 **Organigramme**

![](media/image-20250908171812532.png)

**Schéma de connexion :**

![](media/image-20250908171829321.png)

Remarque : Les broches « - », « + » et « S » du servo sont respectivement connectées à G (GND), V (VCC) et D9 de la carte d'extension. Le VCC, Trig, Echo et Gnd du capteur ultrasonique sont liés à 5v (V), 5 (S), Echo et Gnd (G) de la carte d'extension.

**Code de test :**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 11
 ultrasonic_avoid_tank
 http://www.keyestudio.com
*/
int random2;
int a;
int a1;
int a2;
#define ML_Ctrl 13  // définir la broche de contrôle de direction du moteur gauche
#define ML_PWM 11   // définir la broche de contrôle PWM du moteur gauche
#define MR_Ctrl 12  // définir la broche de contrôle de direction du moteur droit
#define MR_PWM 3   // définir la broche de contrôle PWM du moteur droit

#define Trig 5  // broche Trig ultrasonique
#define Echo 4  // broche Echo ultrasonique
int distance;
#define servoPin 9  // broche servo
int pulsewidth;
/************la fonction pour faire fonctionner le moteur**************/
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

// La fonction pour contrôler le servo
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}
// La fonction pour contrôler le capteur ultrasonique
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.00;  // 58.20, c'est-à-dire 2*29.1=58.2
  delay(10);
  return distance;
}
  //****************************************************************
void setup(){
  pinMode(servoPin, OUTPUT);
  procedure(90); // définir le servo à 90°
  
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  random2 = random(1, 100);
  a = checkdistance();  // attribuer la distance avant détectée par le capteur ultrasonique à la variable a
  
  if (a < 20) // quand la distance avant détectée est inférieure à 20 
  {
      Car_Stop();  // le robot s'arrête
      delay(500); // délai de 500ms
      procedure(160);  // La plateforme ultrasonique tourne à gauche
      for (int j = 1; j <= 10; j = j + (1)) { // instruction for, les données seront plus précises si le capteur ultrasonique détecte plusieurs fois.
        a1 = checkdistance();  // attribuer la distance gauche détectée par le capteur ultrasonique à la variable a1
      }
      delay(300);
      procedure(20); // La plateforme ultrasonique tourne à droite
      for (int k = 1; k <= 10; k = k + (1)) {
        a2 = checkdistance(); // attribuer la distance droite détectée par le capteur ultrasonique à la variable a2
      }
      
      if (a1 < 50 || a2 < 50)  // le robot tournera vers le côté de distance plus longue quand la distance gauche ou droite est inférieure à 50cm. 
      {
        if (a1 > a2) // la distance gauche est supérieure au côté droit      
        {
          procedure(90);  // La plateforme ultrasonique tourne vers l'avant droit         
          Car_left();  // le robot tourne à gauche
          delay(500);  // tourner à gauche pendant 500ms
          Car_front(); // aller vers l'avant
        } 
        else 
        {
          procedure(90);
          Car_right(); // le robot tourne à droite
          delay(500);
          Car_front();  // aller vers l'avant
        }
      } 
      else  // Si les deux côtés sont supérieurs ou égaux à 50cm, tourner à gauche ou à droite aléatoirement
      {
        if ((long) (random2) % (long) (2) == 0)  // Quand le nombre aléatoire est pair
        {
          procedure(90);
          Car_left(); // le réservoir robot tourne à gauche
          delay(500);
          Car_front(); // aller vers l'avant
        } 
        else 
        {
          procedure(90);
          Car_right(); // le robot tourne à droite
          delay(500);
          Car_front(); // aller vers l'avant
       }
     }
  } 
  else  // Si la distance avant est supérieure ou égale à 20cm, la voiture robot ira vers l'avant
  {
      Car_front(); // aller vers l'avant
  }
}
```

 **Résultat du test**

Téléchargez le code avec succès, le commutateur DIP est basculé vers l'extrémité droite et l'alimentation est activée, le réservoir robot avance et évite automatiquement l'obstacle.
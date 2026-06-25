# Projet 10 Robot Suiveur de Lumière

![](media/image-20250908171131879.png)

**Description**

Nous avons présenté comment utiliser différents capteurs et modules.

Dans cette leçon, nous combinons les connaissances matérielles -- module photorésistance, commande moteur, pour construire un robot suiveur de lumière !

Il suffit d'utiliser 2 modules photorésistance pour détecter l'intensité lumineuse des deux côtés du robot. Lire la valeur analogique pour faire tourner les 2 moteurs, et ainsi faire fonctionner le robot tank.

**La logique spécifique du robot suiveur de lumière est présentée dans le tableau ci-dessous :**

![](media/image-20250908171219561.png)

Nous créons un organigramme basé sur le tableau logique ci-dessus, comme indiqué ci-dessous :

![](media/image-20250908171232654.png)

**Schéma de Connexion**

![](media/image-20250908171305946.png)

**Attention :**

Le bloc terminal 4 broches est marqué avec la sérigraphie 1234. Le fil rouge du moteur arrière droit est connecté à la borne 1, le fil noir est relié à la borne 2. Le fil rouge du moteur avant gauche est attaché à la borne 3, le fil noir est relié au port 4.

| Photorésistance gauche   |      | Capteur Shield    |
| ------------------------ | ---- | ----------------- |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A1                |
|                          |      |                   |
| **Photorésistance droite** |      | **Capteur Shield** |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A2                |

**Code de Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 leçon 10
 Tank suiveur de lumière
 http://www.keyestudio.com
*/ 
#define light_L_Pin A1   //définir la broche du photorésistance gauche
#define light_R_Pin A2   //définir la broche du photorésistance droit
#define ML_Ctrl 13  //définir la broche de contrôle de direction du moteur gauche
#define ML_PWM 11   //définir la broche de contrôle PWM du moteur gauche
#define MR_Ctrl 12  //définir la broche de contrôle de direction du moteur droit
#define MR_PWM 3   //définir la broche de contrôle PWM du moteur droit
int left_light; 
int right_light;
void setup(){
  Serial.begin(9600);
  pinMode(light_L_Pin, INPUT);
  pinMode(light_R_Pin, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  left_light = analogRead(light_L_Pin);
  right_light = analogRead(light_R_Pin);
  Serial.print("left_light_value = ");
  Serial.println(left_light);
  Serial.print("right_light_value = ");
  Serial.println(right_light);
  if (left_light > 650 && right_light > 650) //la valeur détectée par le photorésistance, avancer
  {  
    Car_front();
  } 
  else if (left_light > 650 && right_light <= 650)  //la valeur détectée par le photorésistance, tourner à gauche
  {
    Car_left();
  } 
  else if (left_light <= 650 && right_light > 650) //la valeur détectée par le photorésistance, tourner à droite
  {
    Car_right();
  } 
  else  //autres situations, arrêter
  {
    Car_Stop();
  }
}
void Car_front()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,200);
}
void Car_right()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_Stop()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,0);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,0);
}
//****************************************************************
```

**Résultat du Test**

Téléchargez le code sur la carte de développement keyestudio V4.0, le commutateur DIP est basculé à l'extrémité droite et l'alimentation est activée, le robot intelligent suit la lumière pour se déplacer.
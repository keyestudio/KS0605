# 2. Installation du produit

Après avoir vérifié toutes les pièces de ce kit, nous devons assembler le robot tank. Installons la voiture intelligente en conformité avec les instructions suivantes.

## Vidéo d'assemblage

[Télécharger la vidéo](video.7z).

> **Remarque :** La vidéo d'assemblage est fournie dans le fichier `video.7z` inclus dans ce package. Veuillez l'extraire pour visualiser `video/KS0605.mp4`.

**Remarque : Retirez le film plastique de la carte en premier lors de l'installation de la voiture intelligente.**

## Étape 1 : Installer le moteur inférieur

Préparez les pièces comme suit :

- Écrou M4 \* 2
- Moteur métallique \*2
- Support métallique \*2
- Coupleur \*2
- Pièces de support bleu \*2
- Vis à six pans creux M4\*12MM \* 2
- Clé Allen nickelée M1.5 \*1
- Clé Allen nickelée M3 \*1
- Clé Allen nickelée M2.5 \*1
- Vis à six pans creux M3\*8MM \* 4

![TK_02](media/TK_02.png)

![TK_03](media/TK_03.png)

**Conseil : assemblez le moteur de l'autre côté de la même manière.**

## Étape 2 : Installer la roue motrice

Préparez les pièces comme suit :

- Vis à six pans creux M3*8MM \* 2
- Vis à six pans creux M4\*50MM \* 2
- Roue de support du tank \* 2
- Roulement à bride \* 4
- Rondelle\*2
- Bande de chenille \*2
- Écrou autobloquant M4 \* 2
- Clé Allen nickelée M3 \*1
- Clé Allen nickelée M2.5 \*1

![TK_04](media/TK_04.png)

![TK_05](media/TK_05.png)

![TK_06](media/TK_06.png)

![TK_07](media/TK_07.png)

## Étape 3 : Installer le support de batterie

Préparez les pièces comme suit :

- Support de batterie \*1
- Écrou M3 \* 2
- Support métallique bleu \*2
- Écrou M4 \*8
- Vis à tête plate M3\*10MM \* 2
- Vis à six pans creux M4\*40MM \*4
- Clé Allen nickelée M2.5 \*1
- Clé Allen nickelée M3 \*1
- Vis à six pans creux M3\*25MM \*4
- Entretoise hexagonale en cuivre M3*45MM *4
- Tournevis

![TK_08](media/TK_08.png)

![TK_09](media/TK_09.png)

Déplacez-vous pour fixer le support métallique sur la roue motrice avec quatre vis à six pans creux M4\*40MM et quatre écrous M4 une fois le processus de montage terminé.

![TK_10](media/TK_10.png)

![TK_11](media/TK_11.png)

![TK_12](media/TK_12.png)

![TK_13](media/TK_13.png)

##  Étape 4 : Monter la carte acrylique et les capteurs

- Carte acrylique \* 2
- Support en L noir \*1
- Capteur photocellule \*2
- Module récepteur IR \*1
- Panneau LED 8X16 \*1
- Écrou M2 \*4
- Écrou M3 \*10
- Vis à six pans creux M3\*6MM \* 8
- Vis à six pans creux M3\*8MM \* 8
- Clé Allen M2.5 \*1
- Vis à tête ronde M3\*12MM \*6
- Entretoise hexagonale en cuivre M3\*10MM \*8
- Vis à tête ronde M2\*10MM \* 4
- Tournevis

![TK_14](media/TK_14.png)

![TK_15](media/TK_15.png)

![TK_16](media/TK_16.png)

![TK_17](media/TK_17.png)

![TK_18](media/TK_18.png)

![TK_19](media/TK_19.png)

![TK_20](media/TK_20.png)

![TK_21](media/TK_21.png)

![TK_22](media/TK_22.png)

![TK_23](media/TK_23.png)

## Étape 5 : Installer la plateforme servo

Préparez les pièces comme suit :

-   Servo \*1
-   Cardan noir \*1
-   Serre-câble \*2
-   Vis de tôlerie à tête ronde M2x8 \*2
-   Capteur ultrasonique \*1
-   Vis M2\*4 \*1
-   Vis M1.2\*5 \*4
-   Tournevis

**Remarque : **pour un débogage pratique, gardez le module ultrasonique droit devant et l'angle du moteur servo à 90°. Par conséquent, nous devons régler le servo à 90° avant d'installer la plateforme servo.

Définissez le code à 90 degrés, copiez le code et téléchargez-le sur la carte de développement. Le servo connecté au port D9 tournera à 90°.

> Pour télécharger le code, vous aurez besoin de l'Arduino IDE. Veuillez d'abord installer l'Arduino IDE en suivant les sections 4.2–4.4. (Téléchargement du logiciel, Configuration d'Arduino IDE et Ajout de bibliothèque)

```c
#define servoPin 9 //broche servo
int pos; //variable d'angle du servo
int pulsewidth; // variable de largeur d'impulsion du servo

void setup() 
{
    pinMode(servoPin, OUTPUT); //définir la broche servo en OUTPUT
    procedure(0); //définir l'angle du servo à 0°
}

void loop() 
{
	procedure(90); // dire au servo d'aller à la position 90°
}

// fonction pour contrôler le servo
void procedure(int myangle) 
{
    pulsewidth = myangle * 11 + 500; //calculer la valeur de la largeur d'impulsion
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth); //La durée du niveau haut est la largeur d'impulsion
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000)); // le cycle est 20ms, le niveau bas dure le reste du temps
}
```

![TK_24](media/TK_24.png)

![](media/image-20250902144145590.png)

**Remarque : **Vous pouvez trouver les vis M1.2\*5 à l'intérieur du sac de la plateforme en plastique.

![TK_25](media/TK_25.png)

![TK_26](media/TK_26.png)

## Étape 6 : Installer les capteurs et les cartes

Préparez les pièces comme suit :

- Vis à tête ronde M3\*6MM \*12
- Bouclier L298P \*1
- Carte V4.0 \*1
- Bouclier capteur V5 \*1
- Tournevis \*1
- Module Bluetooth \*1
- Clé Allen nickelée M2.5 \*1

![TK_27](media/TK_27.png)

![TK_28](media/TK_28.png)

![TK_29](media/TK_29.png)

![TK_30](media/TK_30.png)

![TK_31](media/TK_31.png)

![TK_32](media/TK_32.png)

![TK_33](media/TK_33.png)

![TK_34](media/TK_34.png)



## Étape 7 : Guide de connexion

![](media/image-20250902144534790.png)

![](media/image-20250902144551034.png)

![](media/image-20250902144559983.png)

![](media/image-20250902144849310.png)

![](media/image-20250902144902221.png)

##  Étape 8 : Câbler le panneau LED

![](media/image-20250902145026905.png)

![](media/image-20250902145112884.png)

![](media/image-20250902145129382.png)

| Panneau LED                            | Bouclier capteur V5                    |
| -------------------------------------- | -------------------------------------- |
| GND                                    | -(GND)                                 |
| VCC                                    | +(VCC)                                 |
| SDA                                    | SDA                                    |
| SCL                                    | SCL                                    |
| ![](media/image-20250902145404151.png) | ![](media/image-20250902145414755.png) |

## Étape 9 : Installer toutes les pièces de la plaque acrylique

![](media/image-20250902145506652.png)

![](media/image-20250902145615504.png)

![](media/image-20250902145822634.png)

![](media/image-20250902145854886.png)

![](media/image-20250902145934002.png)

![](media/image-20250902150004173.png)

![](media/image-20250902150032438.png)

![](media/image-20250902150052468.png)

![](media/image-20250902150217564.png)

![](media/image-20250902150508905.png)

![](media/image-20250902150522753.png)

![](media/image-20250902150532987.png)

![](media/image-20250902150711706.png)

##  Étape 10 : Robot Tank

**Remarque :** Retirez le module Bluetooth avant de télécharger le code de test. Sinon, vous échouerez à télécharger le code de test.

![](media/image-20250902151034545.png)

**Voiture robot polyvalente**

![](media/image-20250902151133169.png)

  **Description**

Dans les projets précédents, la voiture tank n'effectue qu'une seule fonction. Cependant, dans cette leçon, nous intégrons toutes ses fonctions pour contrôler la voiture intelligente via le contrôle Bluetooth.

Voici un simple organigramme de la voiture robot polyvalente pour votre référence.

![](media/image-20250902151215210.png)

  **Schéma de connexion**

![](media/image-20250902151230702.png)

**Attention ：**Confirmez que chaque composant est connecté.

Guide de câblage :

| Panneau LED 8x16 | | Carte d'extension |
| -------------- | ---- | --------------- |
| GND            | →    | -（GND）        |
| VCC            | →    | +（VCC）        |
| SDA            | →    | SDA             |
| SCL            | →    | SCL             |

![](media/image-20250902152539713.png)

| Module ultrasonique |      |        |
| ----------------- | ---- | ------ |
| VCC               | →    | 5v(V)  |
| Trig              | →    | 5(S)   |
| Echo              | →    | 4(S)   |
| Gnd               | →    | Gnd(G) |

![](media/image-20250902152857086.png)

![](media/image-20250902152906103.png)

| Moteur servo |      |        |
| ----------- | ---- | ------ |
| Moteur servo | →    | Gnd(G) |
| Fil rouge    | →    | 5v(V)  |
| Fil orange   | →    | 9      |

![](media/image-20250902154418006.png)

![](media/image-20250902154820948.png)

| Module Bluetooth                        |      |          |
| --------------------------------------- | ---- | -------- |
| RXD                                     | →    | TX       |
| TXD                                     | →    | RX       |
| GND                                     | →    | -（GND） |
| VCC                                     |      | +（VCC） |
| Pas besoin de connecter les broches STATE et BRK |      |          |

![](media/image-20250902155229663.png)

![](media/image-20250902155236836.png)

| Module récepteur IR |      | Bouclier capteur |
| ------------------ | ---- | ------------- |
| －                 | →    | G（GND）      |
| +                  | →    | V（VCC）      |
| S                  | →    | A0            |

![](media/image-20250902155444270.png)

![](media/image-20250902155452133.png)

| Photorésistance gauche  |      | Bouclier capteur |
| -------------------- | ---- | ------------- |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A1            |
|                      |      |               |
| Photorésistance droite |      | Bouclier capteur |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A2            |

![](media/image-20250902155938106.png)

![](media/image-20250902155946213.png)

 Installation terminée.
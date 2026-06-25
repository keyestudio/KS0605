# 3. Installation des pilotes

> Pour l'installation des pilotes, vous pouvez ignorer cette étape initialement, car les pilotes s'installent généralement automatiquement lorsque la carte de développement est connectée à votre ordinateur. Si la carte n'est pas reconnue après la connexion, reportez-vous à cette section pour installer les pilotes.

## 3.1 Système Windows

**Vérification du pilote**

1. Connectez la carte mère à l'ordinateur.

![](./media/1.jpg)

2. Ouvrez le Gestionnaire des périphériques. Si l'invite **"Silicon Labs CP210x USB to UART Bridge (COMx)"** apparaît, cela prouve que le pilote a été installé. Veuillez ignorer la partie **"Installation du pilote"**.

![](./media/Animation.gif)

**Installation manuelle du pilote**

1. Téléchargement du pilote

- Système Windows : [Pilote du système Windows](./Windows.7z)

2. Connectez la carte mère à l'ordinateur, ouvrez le Gestionnaire des périphériques. S'il y a un point d'exclamation jaune devant le pilote dans l'image, cela prouve que le pilote n'est pas installé. Veuillez télécharger et installer le pilote manuellement.

![](./media/Animation-1750921346712-3.gif)

## 3.2 Système MAC

**1 Vérification du pilote**

Connectez la carte de développement à l'ordinateur, selon [Outils] ---> [Port] pour sélectionner le port de la carte de développement (Remarque : Si vous ne pouvez pas confirmer quel port est la carte de développement, veuillez connecter la carte mère et prendre des photos pour enregistrer tous les ports, puis débranchez la carte de développement et reprenez des photos pour enregistrer tous les ports, puis comparez pour trouver les ports disparus. Le port disparu est le port de la carte. Sélectionnez ensuite le port). Si vous ne pouvez pas reconnaître le port, veuillez remplacer le port USB de l'ordinateur ou essayer un autre câble pour reconnaître le port. Si cela ne fonctionne toujours pas, reportez-vous aux étapes suivantes pour installer le pilote.

![](./media/20250626154343.png)

**2 Installation manuelle du pilote**

1. Téléchargement du pilote

​       Système Mac : [Pilote du système Mac](./Mac.7z)

2. Double-cliquez pour décompresser le package zip du pilote téléchargé

![](./media/image-20250417083615847-1749262759458-8.png)

![](./media/image-20250417083758947-1749262759458-7.png)

![](./media/image-20250417083918581-1749262759458-5.png)

3. Après cela, continuez à cliquer sur **"Suivant"** jusqu'à ce que l'installation soit terminée

![](./media/7cca827fe946096f228797dadce10661.png)

À ce stade, le port peut être reconnu en branchant à nouveau la carte.

4. Ensuite, allez dans l'Arduino IDE, cliquez sur "Outils", sélectionnez la carte Arduino Uno et le port de la carte de développement reconnu.

![](./media/2.png)

5. Cliquez sur ![image-20250417085312966](./media/image-20250417085312966-1749262759459-18.png) pour télécharger le code et afficher "Téléchargement terminé".

![](./media/3.png)
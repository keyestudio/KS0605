# 3. Installazione del driver

> Per l'installazione del driver, è possibile saltare questo passaggio inizialmente, poiché i driver in genere si installano automaticamente quando la scheda di sviluppo è collegata al computer. Se la scheda non viene riconosciuta dopo il collegamento, fare riferimento a questa sezione per installare i driver.

## 3.1 Sistema Windows

**Verifica del driver**

1. Collegare la scheda madre al computer.

![](./media/1.jpg)

2. Aprire Gestione dispositivi. Se viene visualizzato il messaggio **"Silicon Labs CP210x USB to UART Bridge (COMx)"**, ciò prova che il driver è stato installato; saltare la parte **"Installazione del driver"**.

![](./media/Animation.gif)

**Installazione manuale del driver**

1. Download del driver

- Sistema Windows: [Driver per sistema Windows](./Windows.7z)

2. Collegare la scheda madre al computer e aprire Gestione dispositivi. Se è presente un punto esclamativo giallo davanti al driver nell'immagine, ciò prova che il driver non è installato; scaricare il driver e installarlo manualmente.

![](./media/Animation-1750921346712-3.gif)

## 3.2 Sistema MAC

**1 Verifica del driver**

Collegare la scheda di sviluppo al computer e, secondo [Strumenti] ---> [Porta], selezionare la porta della scheda di sviluppo (Nota: Se non è possibile confermare quale porta è la scheda di sviluppo, collegare la scheda madre e scattare foto per registrare tutte le porte, quindi scollegare la scheda di sviluppo e scattare di nuovo foto per registrare tutte le porte, quindi confrontare per trovare le porte scomparse. La porta scomparsa è la porta della scheda; selezionare la porta di conseguenza). Se la porta non viene riconosciuta, provare a sostituire la porta USB del computer o il cavo intorno al telefono per riconoscere di nuovo la porta. Se ancora non funziona, fare riferimento ai seguenti passaggi per installare il driver.

![](./media/20250626154343.png)

**2 Installazione manuale del driver**

1. Download del driver

​       Sistema Mac: [Driver per sistema Mac](./Mac.7z)

2. Fare doppio clic per decomprimere il pacchetto zip del driver scaricato

![](./media/image-20250417083615847-1749262759458-8.png)

![](./media/image-20250417083758947-1749262759458-7.png)

![](./media/image-20250417083918581-1749262759458-5.png)

3. Successivamente, continuare a fare clic su **"Avanti"** fino al completamento dell'installazione

![](./media/7cca827fe946096f228797dadce10661.png)

A questo punto, la porta può essere riconosciuta collegando di nuovo la scheda.

4. Quindi andare all'IDE Arduino, fare clic su "Strumenti", selezionare la scheda Arduino Uno e la porta della scheda di sviluppo riconosciuta.

![](./media/2.png)

5. Fare clic su ![image-20250417085312966](./media/image-20250417085312966-1749262759459-18.png) per caricare il codice e visualizzare "Caricamento completato".

![](./media/3.png)
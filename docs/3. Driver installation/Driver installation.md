# 3. Stuurprogramma-installatie

> Voor stuurprogramma-installatie kunt u deze stap in eerste instantie overslaan, omdat stuurprogramma's doorgaans automatisch worden geïnstalleerd wanneer het ontwikkelingsbord met uw computer wordt verbonden. Als het bord na verbinding niet wordt herkend, raadpleegt u deze sectie om de stuurprogramma's te installeren.

## 3.1 Windows-systeem

**Het stuurprogramma controleren**

1. Verbind het moederbord met de computer.

![](./media/1.jpg)

2. Open Apparaatbeheer. Als de prompt **"Silicon Labs CP210x USB to UART Bridge (COMx)"** verschijnt, bewijst dit dat het stuurprogramma is geïnstalleerd. Sla in dat geval het gedeelte **"Stuurprogramma-installatie"** over.

![](./media/Animation.gif)

**Handmatige stuurprogramma-installatie**

1. Stuurprogramma downloaden

- Windows-systeem: [Windows-systeemstuurprogramma](./Windows.7z)

2. Verbind het moederbord met de computer en open Apparaatbeheer. Als er een geel uitroepteken voor het stuurprogramma in de afbeelding staat, bewijst dit dat het stuurprogramma niet is geïnstalleerd. Download het stuurprogramma en installeer het handmatig.

![](./media/Animation-1750921346712-3.gif)

## 3.2 MAC-systeem

**1 Het stuurprogramma controleren**

Verbind het ontwikkelingsbord met de computer en selecteer volgens [Tools] ---> [Port] de poort van het ontwikkelingsbord (Opmerking: Als u niet zeker weet welke poort het ontwikkelingsbord is, verbindt u het moederbord en maakt u foto's om alle poorten vast te leggen. Ontkoppel vervolgens het ontwikkelingsbord en maak opnieuw foto's om alle poorten vast te leggen. Vergelijk vervolgens om de verdwenen poort te vinden. De verdwenen poort is de poort van het bord. Selecteer vervolgens de poort). Als de poort niet wordt herkend, vervang dan de USB-poort van de computer of probeer een ander kabel rond de telefoon om de poort opnieuw te herkennen. Als dit nog steeds niet werkt, volgt u de volgende stappen om het stuurprogramma te installeren.

![](./media/20250626154343.png)

**2 Handmatige stuurprogramma-installatie**

1. Stuurprogramma downloaden

​       Mac-systeem: [Mac-systeemstuurprogramma](./Mac.7z)

2. Dubbelklik om het gedownloade stuurprogramma-zipbestand uit te pakken

![](./media/image-20250417083615847-1749262759458-8.png)

![](./media/image-20250417083758947-1749262759458-7.png)

![](./media/image-20250417083918581-1749262759458-5.png)

3. Klik daarna op **"Next"** totdat de installatie is voltooid

![](./media/7cca827fe946096f228797dadce10661.png)

Op dit moment kan de poort worden herkend door het bord opnieuw in te pluggen.

4. Ga vervolgens naar de Arduino IDE, klik op "Tools", selecteer het bord Arduino Uno en de herkende poort van het ontwikkelingsbord.

![](./media/2.png)

5. Klik ![image-20250417085312966](./media/image-20250417085312966-1749262759459-18.png) om code te uploaden en het bericht "Done uploading" wordt weergegeven.

![](./media/3.png)
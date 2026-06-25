# 3. Treiberinstallation

> Bei der Treiberinstallation können Sie diesen Schritt zunächst überspringen, da Treiber normalerweise automatisch installiert werden, wenn das Entwicklungsboard mit Ihrem Computer verbunden wird. Falls das Board nach dem Anschließen nicht erkannt wird, lesen Sie diesen Abschnitt, um die Treiber zu installieren.

## 3.1 Windows-System

**Treiber überprüfen**

1. Verbinden Sie das Motherboard mit dem Computer.

![](./media/1.jpg)

2. Öffnen Sie den Geräte-Manager. Wenn die Meldung **"Silicon Labs CP210x USB to UART Bridge (COMx)"** angezeigt wird, bedeutet dies, dass der Treiber bereits installiert ist. Überspringen Sie bitte den Abschnitt **"Treiberinstallation"**.

![](./media/Animation.gif)

**Manuelle Treiberinstallation**

1. Treiberdownload

- Windows-System: [Windows-System-Treiber](./Windows.7z)

2. Verbinden Sie das Motherboard mit dem Computer und öffnen Sie den Geräte-Manager. Wenn vor dem Treiber in der Abbildung ein gelbes Ausrufezeichen angezeigt wird, bedeutet dies, dass der Treiber nicht installiert ist. Bitte laden Sie den Treiber herunter und installieren Sie ihn manuell.

![](./media/Animation-1750921346712-3.gif)

## 3.2 MAC-System

**1 Treiber überprüfen**

Verbinden Sie das Entwicklungsboard mit dem Computer. Gehen Sie zu [Tools] ---> [Port], um den Port des Entwicklungsboards auszuwählen (Hinweis: Falls Sie nicht sicher sind, welcher Port das Entwicklungsboard ist, verbinden Sie das Motherboard und machen Sie ein Foto aller Ports. Trennen Sie dann das Entwicklungsboard ab und machen Sie erneut ein Foto aller Ports. Vergleichen Sie die Fotos, um den fehlenden Port zu finden. Dieser Port ist der Port des Boards. Wählen Sie diesen Port aus). Falls der Port nicht erkannt wird, versuchen Sie, einen anderen USB-Port des Computers zu verwenden oder ersetzen Sie das Kabel. Falls dies nicht funktioniert, führen Sie die folgenden Schritte zur Treiberinstallation durch.

![](./media/20250626154343.png)

**2 Manuelle Treiberinstallation**

1. Treiberdownload

​       Mac-System: [Mac-System-Treiber](./Mac.7z)

2. Doppelklicken Sie, um das heruntergeladene Treiber-ZIP-Paket zu entpacken

![](./media/image-20250417083615847-1749262759458-8.png)

![](./media/image-20250417083758947-1749262759458-7.png)

![](./media/image-20250417083918581-1749262759458-5.png)

3. Klicken Sie anschließend wiederholt auf **"Weiter"**, bis die Installation abgeschlossen ist

![](./media/7cca827fe946096f228797dadce10661.png)

An diesem Punkt kann der Port erkannt werden, indem das Board erneut angeschlossen wird.

4. Gehen Sie dann zur Arduino IDE, klicken Sie auf "Tools", wählen Sie das Board Arduino Uno und den erkannten Port des Entwicklungsboards aus.

![](./media/2.png)

5. Klicken Sie auf ![image-20250417085312966](./media/image-20250417085312966-1749262759459-18.png), um Code hochzuladen und "Done uploading" wird angezeigt.

![](./media/3.png)
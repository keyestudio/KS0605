# Projekt 1 LED-Blinken

![](media/image-20250908174750401.png)

**Beschreibung**

Das LED-Blinken ist ein grundlegendes Programm für Anfänger und Enthusiasten. LED ist die Abkürzung für Leuchtdioden und besteht aus chemischen Verbindungen wie Ga, As, P und N. Die LED kann in verschiedenen Farben blinken, indem man die Verzögerungszeit im Testcode ändert. Bei der Steuerung leuchtet die LED auf, wenn der S-Anschluss auf hohem Niveau liegt, wenn GND und VCC mit Strom versorgt werden; andernfalls schaltet sie sich aus.

**Spezifikation**

![](./media/image-20250902164418568.png)

- Steuerungsschnittstelle: digitaler Anschluss
- Betriebsspannung: DC 3,3-5V
- Stiftabstand: 2,54mm
- LED-Anzeigefarbe: rot

**Komponenten**

![](./media/image-20250902164804229.png)

**V5 Sensor Shield**

Es ist mühsam, Arduino-Entwicklungsboards mit zahlreichen Sensoren zu kombinieren. Das V5 Sensor Shield ist jedoch mit Arduino-Entwicklungsboards kompatibel und löst dieses Problem perfekt. Stapeln Sie einfach das V5-Board darauf.

Dieses Sensor Shield kann in 3-Pin-Sensormodule eingesteckt werden und bricht einige Kommunikationspins auf, wie serielle, IIC- und SPI-Kommunikation.

**Pins-Beschreibung**

![](./media/image-20250902165027854.png)

**Schaltschema**

![](./media/image-20250902165110913.png)

Wie aus dem obigen Diagramm ersichtlich ist, ist die LED mit D2 verbunden.

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 1.1
 Blink
 http://www.keyestudio.com
*/
void setup()
{ 
    pinMode(2, OUTPUT);// Initialisieren Sie digitalen Pin 2 als Ausgang.
}

void loop() // Die Loop-Funktion läuft immer wieder endlos ab
{
   digitalWrite(2, HIGH); // LED einschalten (HIGH ist die Spannungsebene)
   delay(1000); // eine Sekunde warten
   digitalWrite(2, LOW); // LED ausschalten, indem die Spannung auf LOW gesetzt wird
   delay(1000); // eine Sekunde warten
}
```

**Testergebnis**

(Es gibt einen Widerspruch zwischen der seriellen Kommunikation im Code und dem Bluetooth beim Hochladen des Codes. Verbinden Sie daher das Bluetooth-Modul nicht vor dem Hochladen des Codes.)

Laden Sie das Programm auf das Entwicklungsboard hoch. Die LED blinkt in einem Intervall von 1s.

![](./media/image-20250902165335641.png)

**Code-Erklärung**

**pinMode(2，OUTPUT) -** Setzt Pin 2 auf OUTPUT

**digitalWrite(2，HIGH) -** Wenn Pin 2 auf HIGH-Ebene (5V Ausgang) oder LOW-Ebene (0V Ausgang) gesetzt wird

**Erweiterungspraxis**

Wir haben es geschafft, die LED zum Blinken zu bringen. Schauen wir uns nun an, wie sich die LED ändert, wenn wir die Pins und die Verzögerungszeit ändern.

**Schaltschema**

![](./media/image-20250902165631206.png)

Wir haben die Pins geändert und die LED mit D10 verbunden.

**Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 1.2
 Verzögerung
 http://www.keyestudio.com
*/
void setup() // Initialisieren Sie digitalen Pin 10 als Ausgang.
{  
   pinMode(10, OUTPUT);
}

// Die Loop-Funktion läuft immer wieder endlos ab
void loop() 
{
   digitalWrite(10, HIGH); // LED einschalten (HIGH ist die Spannungsebene)
   delay(100); // 0,1 Sekunde warten
   digitalWrite(10, LOW); // LED ausschalten, indem die Spannung auf LOW gesetzt wird
   delay(100); // 0,1 Sekunde warten
}
```

Das Testergebnis zeigt, dass die LED schneller blinkt. Daher können wir zu dem Ergebnis kommen, dass Pins und Verzögerungszeit die Blinkfrequenz beeinflussen.
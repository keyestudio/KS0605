# Projekt 2 LED-Helligkeit anpassen

**(1) Beschreibung**

In der vorherigen Lektion haben wir die LED ein- und ausgeschaltet und zum Blinken gebracht.

In diesem Projekt werden wir die Helligkeit der LED durch PWM steuern, um Atemeffekte zu simulieren. Auf ähnliche Weise können Sie die Schrittlänge und Verzögerungszeit im Code ändern, um verschiedene Atemeffekte zu demonstrieren.

PWM ist eine Methode zur Steuerung der analogen Ausgabe durch digitale Mittel. Die digitale Steuerung wird verwendet, um Rechteckwellen mit unterschiedlichen Tastverhältnissen (ein Signal, das ständig zwischen hohen und niedrigen Pegeln wechselt) zu erzeugen, um die analoge Ausgabe zu steuern. Im Allgemeinen betragen die Eingangsspannungen der Anschlüsse 0V und 5V. Was ist, wenn 3V erforderlich sind? Oder ein Wechsel zwischen 1V, 3V und 3,5V? Wir können nicht ständig Widerstände ändern. Aus diesem Grund greifen wir auf PWM zurück.

![](./media/bbcfcb9ae56abb7e80ee587246fc4be9.GIF)

Bei der Arduino-Digitalanschlussausgabe gibt es nur LOW und HIGH, die den Spannungsausgaben von 0V und 5V entsprechen. Sie können LOW als 0 und HIGH als 1 definieren und das Arduino innerhalb von 1 Sekunde fünfhundert 0- oder 1-Signale ausgeben lassen.

Wenn fünfhundert 1er ausgegeben werden, das heißt 5V; wenn alle davon 1 sind, das heißt 0V. Wenn auf diese Weise 010101010101 ausgegeben wird, beträgt die Ausgangsspannung des Anschlusses 2,5V, was wie das Zeigen eines Films ist. Die Filme, die wir sehen, sind nicht vollständig kontinuierlich. Es werden tatsächlich 25 Bilder pro Sekunde ausgegeben. In diesem Fall kann der Mensch es nicht erkennen, und PWM auch nicht. Wenn Sie eine andere Spannung wünschen, müssen Sie das Verhältnis von 0 und 1 steuern. Je mehr 0- und 1-Signale pro Zeiteinheit ausgegeben werden, desto genauer ist die Steuerung.

**(2) Spezifikation**

- Steueroberfläche: Digitalanschluss
- Betriebsspannung: DC 3,3-5V
- Pin-Abstand: 2,54mm
- Anzeigefarbe: rot

**(3) Komponenten**

![](./media/image-20250902170952089.png)

 **(4) Schaltplan**

![](./media/image-20250902171013917.png)

 **(5) Testcode**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 2.2
 pwm-langsam
 http://www.keyestudio.com
*/
int ledPin = 10; // Definieren Sie den LED-Pin an D10
int value;

void setup () 
{
	pinMode (ledPin, OUTPUT); // Initialisieren Sie ledpin als Ausgang.
}

void loop () 
{
    for (value = 0; value <255; value = value + 1)
    {
        analogWrite (ledPin, value); // LED leuchtet allmählich auf
        delay (30); // Verzögerung 30ms
    }
    for (value = 255; value> 0; value = value-1)
    {
        analogWrite (ledPin, value); // LED erlischt allmählich
        delay (30); // Verzögerung 30ms
	}
}
```

**Testergebnis**

Nach dem erfolgreichen Hochladen des Testcodes ändert sich die LED allmählich von hell zu dunkel, wie der Atem eines Menschen, anstatt sich sofort ein- oder auszuschalten.

**Code-Erklärung**

Wenn wir einige Anweisungen wiederholen müssen, können wir die FOR-Anweisung verwenden.

Das FOR-Anweisungsformat ist unten dargestellt:

![](./media/image-20250902171421873.png)

ODER Schleifenfolge:

Runde 1：1 → 2 → 3 → 4

Runde 2：2 → 3 → 4

…

Bis Nummer 2 nicht erfüllt ist, endet die „for"-Schleife.

Nach Kenntnis dieser Reihenfolge zurück zum Code:

**for (int value = 0; value < 255; value=value+1){**

**...**

**}**

**for (int value = 255; value >0; value=value-1){**

**...**

**}**

Die beiden „for"-Anweisungen lassen den Wert von 0 auf 255 ansteigen, dann von 255 auf 0 sinken, dann wieder auf 255 ansteigen, .... endlose Schleife.

Es gibt eine neue Funktion im Folgenden ----- analogWrite()

Wir wissen, dass der Digitalanschluss nur zwei Zustände von 0 und 1 hat. Wie sendet man also einen analogen Wert an einen digitalen Wert? Hier wird diese Funktion benötigt. Schauen wir uns das Arduino-Board an und finden Sie 6 Pins mit der Markierung „\~", die PWM-Signale ausgeben können.

**Funktionsformat wie folgt:**

**analogWrite(pin,value)**

analogWrite() wird verwendet, um einen analogen Wert von 0\~255 für den PWM-Anschluss zu schreiben, daher liegt der Wert im Bereich von 0\~255. Beachten Sie, dass Sie nur die Digitalstifte mit PWM-Funktion schreiben, wie z. B. Pin 3, 5, 6, 9, 10, 11.

PWM ist eine Technologie, um eine analoge Größe durch digitale Methoden zu erhalten. Die digitale Steuerung bildet eine Rechteckwelle, und das Rechteckwellensignal hat nur zwei Zustände des Ein- und Ausschaltens (d. h. hohe oder niedrige Pegel). Durch Steuerung des Verhältnisses der Dauer des Ein- und Ausschaltens kann eine Spannung von 0 bis 5V simuliert werden. Die Zeit des Einschaltens (akademisch als hoher Pegel bezeichnet) wird als Impulsbreite bezeichnet, daher wird PWM auch als Pulsweitenmodulation bezeichnet.

Durch die folgenden fünf Rechteckwellen können wir mehr über PWM erfahren.

![](./media/image-20250902172304373.png)

In der obigen Abbildung stellt die grüne Linie eine Periode dar, und der Wert von analogWrite() entspricht einem Prozentsatz, der auch als Tastverhältnis bezeichnet wird.

Das Tastverhältnis bedeutet, dass die Hochpegelsdauer durch die Tiefpegelsdauer in einem Zyklus geteilt wird. Von oben nach unten beträgt das Tastverhältnis der ersten Rechteckwelle 0% und der entsprechende Wert ist 0. Die LED-Helligkeit ist am niedrigsten, das heißt, sie ist ausgeschaltet. Je länger der hohe Pegel anhält, desto heller leuchtet die LED. Daher beträgt das letzte Tastverhältnis 100%, was 255 entspricht, und die LED leuchtet am hellsten. 25% bedeutet dunkler.

PWM wird hauptsächlich zur Anpassung der LED-Helligkeit oder der Drehzahl eines Motors verwendet.

Es spielt eine wichtige Rolle bei der Steuerung des intelligenten Roboter-Autos. Ich glaube, dass Sie es kaum erwarten können, zum nächsten Projekt zu gelangen.
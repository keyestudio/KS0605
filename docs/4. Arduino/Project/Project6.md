# Projekt 6 IR-Empfang

**Beschreibung**

Es ist unbestritten, dass die Infrarot-Fernbedienung im täglichen Leben allgegenwärtig ist. Sie wird zur Steuerung verschiedener Haushaltsgeräte wie Fernseher, Stereoanlagen, Videorecorder und Satellitenempfänger verwendet. Die Infrarot-Fernbedienung besteht aus Infrarot-Sender- und Infrarot-Empfängersystemen, d. h. einer Infrarot-Fernbedienung und einem Infrarot-Empfängermodul sowie einem Mikrocontroller, der zum Dekodieren fähig ist.

![](media/image-20250908155801467.png)

Das 38-kHz-Infrarot-Trägersignal, das vom Fernbedienungssender ausgesendet wird, wird durch den Kodierungschip in der Fernbedienung kodiert. Es besteht aus einem Pilotcode, Benutzercode, Benutzer-Inversecode, Datencode und Daten-Inversecode. Das Zeitintervall des Impulses wird verwendet, um zu unterscheiden, ob es sich um ein 0- oder 1-Signal handelt, und die Kodierung besteht aus diesen 0- und 1-Signalen.

Der Benutzercode derselben Fernbedienung bleibt unverändert, während der Datencode die Taste unterscheiden kann.

Wenn die Fernbedienungstaste gedrückt wird, sendet die Fernbedienung ein Infrarot-Trägersignal aus. Wenn der IR-Empfänger das Signal empfängt, dekodiert das Programm das Trägersignal und bestimmt, welche Taste gedrückt wurde. Der MCU dekodiert das empfangene 01-Signal und bestimmt somit, welche Taste der Fernbedienung gedrückt wurde.

Der von uns verwendete Infrarot-Empfänger ist ein Infrarot-Empfängermodul. Es besteht hauptsächlich aus einem Infrarot-Empfängerkopf und ist ein Gerät, das Empfang, Verstärkung und Demodulation integriert. Der interne IC hat die Demodulation abgeschlossen und kann von der Infrarot-Empfängnis bis zur Ausgabe erfolgen und ist mit TTL-Signalen kompatibel.

Darüber hinaus eignet es sich für Infrarot-Fernbedienung und Infrarot-Datenübertragung. Das vom Empfänger hergestellte Infrarot-Empfängermodul hat nur drei Anschlüsse: Signalleitung, VCC und GND. Die Kommunikation mit Arduino und anderen Mikrocontrollern ist sehr bequem.

**Spezifikation**

![](media/image-20250908160124669.png)

![](media/image-20250908160132699.png)

- Betriebsspannung: 3,3-5V（DC）
- Schnittstelle: 3PIN
- Ausgangssignal: Digitales Signal
- Empfangswinkel: 90 Grad
- Frequenz: 38kHz
- Empfangsreichweite: 10m

**Komponenten**

![](media/image-20250908160309873.png)

**Schaltplan**

![](media/image-20250908160331260.png)

Verbinden Sie "-", "+" und S des IR-Empfängermoduls jeweils mit G(GND), V(VCC) und A0 des keyestudio-Entwicklungsboards.

**Hinweis:** Wenn digitale Anschlüsse nicht verfügbar sind, können analoge Anschlüsse als digitale Anschlüsse verwendet werden. A0 entspricht D14, A1 entspricht Digital 15.

**Test-Code**

Importieren Sie zunächst die Bibliotheksdatei des IR-Empfängermoduls (siehe Anleitung zum Importieren der Arduino-Bibliotheksdatei), bevor Sie den Code entwerfen.

```c
/*
keyestudio Mini Tank Robot V2.1
Lektion 6
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>     // IRremote-Bibliotheksdeklaration
int RECV_PIN = A0;        // Definieren Sie die Anschlüsse des IR-Empfängers als A0
IRrecv irrecv(RECV_PIN);   
decode_results results;   // Dekodierungsergebnisse befinden sich in "result" von "decode results"
void setup()  
  {
      Serial.begin(9600);  
      irrecv.enableIRIn(); // Empfänger aktivieren
  }  
 void loop() {  
    if (irrecv.decode(&results))// Erfolgreich dekodiert, empfangen Sie einen Satz von Infrarotsignalen
    {  
      Serial.println(results.value, HEX);// Wort in 16 HEX umwandeln und empfangenen Code ausgeben
      irrecv.resume(); // Nächsten Wert empfangen
    }  
    delay(100);  
  }
```

 **Test-Ergebnis**

Laden Sie den Test-Code hoch, öffnen Sie den seriellen Monitor und stellen Sie die Baudrate auf 9600 ein. Richten Sie die Fernbedienung auf den IR-Empfänger und der entsprechende Wert wird angezeigt. Wenn Sie lange drücken, erscheinen Fehlercodes.

![](media/image-20250908160550590.png)

Unten haben wir die Werte jeder Taste der keyestudio-Fernbedienung aufgelistet. Sie können diese als Referenz behalten.

![](media/image-20250908160603853.png)

**Code-Erklärung**

**irrecv.enableIRIn():** Nach der Aktivierung der IR-Dekodierung werden die IR-Signale empfangen, dann prüft die Funktion "decode()" kontinuierlich, ob die Dekodierung erfolgreich ist.

**irrecv.decode(&results):** Nach erfolgreicher Dekodierung gibt diese Funktion "true" zurück und speichert das Ergebnis in "results". Nach der Dekodierung eines IR-Signals führen Sie die Funktion resume() aus und empfangen das nächste Signal.

**Erweiterungspraxis**

Wir haben den Tastenwert der IR-Fernbedienung dekodiert. Wie wäre es mit der Steuerung einer LED durch den gemessenen Wert? Wir könnten ein Experiment durchführen, um dies zu bestätigen. Schließen Sie eine LED an D10 an und drücken Sie dann die Tasten der Fernbedienung, um die LED ein- und auszuschalten.

![](media/image-20250908160749345.png)

```c
/* keyestudio Mini Tank Robot V2.1
Lektion 6.2
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>
int RECV_PIN = A0;// Definieren Sie den Anschluss des IR-Empfängers als A0
int LED_PIN=10;// Definieren Sie den Anschluss der LED
int a=0;
IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
  Serial.begin(9600);
  irrecv.enableIRIn(); // Initialisieren Sie den IR-Empfänger
  pinMode(LED_PIN,OUTPUT);// Stellen Sie den Anschluss der LED auf 4
}

void loop() 
{
  if (irrecv.decode(&results)) 
  {
	Serial.println(results.value, HEX);// Wort in 16 HEX umwandeln und empfangenen Code ausgeben
	if(results.value==0xFF02FD &a==0) // Gemäß dem obigen Tastenwert wird die LED gesteuert, wenn Sie "OK" auf der Fernbedienung drücken
	{
		digitalWrite(LED_PIN,HIGH);// LED wird eingeschaltet
		a=1;
	}
	else if(results.value==0xFF02FD &a==1) // Erneut drücken
	{
        digitalWrite(LED_PIN,LOW);// LED wird ausgeschaltet
        a=0;	
	}
	irrecv.resume(); // Nächsten Wert empfangen
  }
}
```

Laden Sie den Code auf das Entwicklungsboard hoch und drücken Sie die Taste "OK" auf der Fernbedienung, um die LED ein- und auszuschalten.
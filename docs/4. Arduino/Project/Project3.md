# Progetto 3 Sensore Fotoresistenza

![](./media/image-20250902173047302.png)

 **Descrizione**

La fotoresistenza è una resistenza speciale realizzata con materiali semiconduttori come CdS o setto di Selenio. La superficie è inoltre rivestita con resina impermeabile, che ha un effetto fotoconducente. È sensibile alla luce ambientale. La sua resistenza varia in base alle diverse intensità luminose.

Utilizziamo le caratteristiche della fotoresistenza per progettare il circuito e generare il modulo fotoresistenza.

Collegando il pin del segnale del modulo fotocellula alla porta Analogica, scoprirai che più forte è l'intensità luminosa, maggiore è la tensione della porta analogica e maggiore è il valore analogico.

Al contrario, più debole è l'intensità luminosa, minore è la tensione della porta analogica, minore è il valore analogico.

In base a ciò, possiamo utilizzare il modulo fotocellula per leggere il valore analogico e ottenere così l'intensità della luce ambientale.

 **Specifiche**

![](./media/image-20250902173349950.png)

- Resistenza: 5K ohm-0.5Mohm
- Tipo di interfaccia: analogica
- Tensione di lavoro: 3.3V-5V
- Installazione facile: con fori di fissaggio a vite
- Spaziatura dei pin: 2.54mm

 **Componenti**

![](./media/image-20250902173528860.png)

 **Diagramma di collegamento:**

![](./media/image-20250902173558747.png)

I due sensori fotoresistenza sono collegati con A1 e A2, quindi completare l'esperimento tramite la fotoresistenza collegata ad A1. Leggiamo il suo valore analogico.

**Codice di prova**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 3.1
 fotocellula
 http://www.keyestudio.com
*/

int sensorPin = A1;    // seleziona il pin di ingresso per la fotocellula
int sensorValue = 0;  // variabile per memorizzare il valore proveniente dal sensore
void setup() 
{
	Serial.begin(9600);
}

void loop() 
{
    sensorValue = analogRead(sensorPin);  // leggi il valore dal sensore:
    Serial.println(sensorValue);  // la porta seriale stampa il valore di resistenza
    delay(500);
}
//******************************************************
```

 **Risultato della prova**

Carica il codice sulla scheda di sviluppo, apri il monitor seriale e verifica se il suo valore diminuisce quando copri la fotoresistenza. Tuttavia, il valore aumenta quando è scoperta.

![](./media/image-20250902174159923.png)

**Spiegazione del codice**

**analogRead(sensorPin):** leggi il valore analogico della fotoresistenza tramite le porte analogiche.

**Serial.begin(9600):** Inizializza la porta seriale, la velocità di trasmissione della comunicazione seriale è 9600.

**Serial.println:** La porta seriale stampa e va a capo.

**Pratica di estensione**

Abbiamo imparato come leggere il valore della fotoresistenza. Combiniamo la fotoresistenza con un LED e osserviamo lo stato del LED.

![](./media/image-20250902174256941.png)

PWM limita la luminosità, quindi il LED è collegato ai pin PWM. Collega il LED al pin 10, mantieni il pin della fotoresistenza invariato, quindi progetta il codice:

```c
/*keyestudio Mini Tank Robot V2.1
lezione 3.2
fotocellula-uscita analogica
http://www.keyestudio.com
*/
int analogInPin = A1;  // pin di ingresso analogico a cui è collegata la fotocellula
int analogOutPin = 10; // pin di uscita analogica a cui è collegato il LED
int sensorValue = 0;        // valore letto dal potenziometro
int outputValue = 0;        // valore in uscita al PWM (uscita analogica)

void setup() 
{
	Serial.begin(9600);
 }
void loop() 
{
  // leggi il valore di ingresso analogico:
  sensorValue = analogRead(analogInPin);
  // mappalo nell'intervallo dell'uscita analogica:
  outputValue = map(sensorValue, 0, 1023, 0, 255);
  // cambia il valore di uscita analogica:
  analogWrite(analogOutPin, outputValue);
  // attendi 2 millisecondi prima del prossimo ciclo affinché il convertitore
  // analogico-digitale si stabilizzi dopo l'ultima lettura:
 Serial.println(sensorValue);  // la porta seriale stampa il valore della fotoresistenza
 delay(2);
}
//***************************************************************
```

Carica il codice, premi con la mano per osservare la luminosità del LED.
# Progetto 4 Controllo del Servo

![](media/image-20250908152354194.png)

 **Descrizione**

Il servomotore è un attuatore rotativo per il controllo della posizione. È composto principalmente da un alloggiamento, una scheda circuito, un motore senza nucleo, un ingranaggio e un sensore di posizione. Il suo principio di funzionamento prevede che il servo riceva il segnale inviato dai microcontrollori o dai ricevitori e produca un segnale di riferimento con un periodo di 20ms e una larghezza di 1,5ms, quindi confronti la tensione di polarizzazione DC acquisita con la tensione del potenziometro per ottenere un'uscita della differenza di tensione.

Quando la velocità del motore è costante, il potenziometro viene fatto ruotare attraverso l'ingranaggio riduttore a cascata, il che porta la differenza di tensione a 0 e il motore si ferma. In generale, l'intervallo di rotazione del servo è compreso tra 0° e 180°.

L'angolo di rotazione del servomotore è controllato regolando il ciclo di lavoro del segnale PWM (Pulse-Width Modulation). Il ciclo standard del segnale PWM è 20ms (50Hz). Teoricamente, la larghezza è distribuita tra 1ms e 2ms, ma in pratica è compresa tra 0,5ms e 2,5ms. La larghezza corrisponde all'angolo di rotazione da 0° a 180°. Si noti tuttavia che per motori di marche diverse, lo stesso segnale può produrre angoli di rotazione differenti.

![](media/image-20250908152510007.png)

In generale, il servo ha tre fili: marrone, rosso e arancione. Il filo marrone è collegato a massa, quello rosso è il polo positivo e quello arancione è il filo del segnale.

![](media/image-20250908152525491.png)

Gli angoli corrispondenti del servo sono mostrati di seguito:

![](media/image-20250908152558682.png)

 **Specifiche tecniche**

- Tensione di lavoro: DC 4,8V \~ 6V
- Intervallo di angolo operativo: circa 180° (a 500 → 2500 μsec)
- Intervallo di larghezza di impulso: 500 → 2500 μsec
- Velocità a vuoto: 0,12 ± 0,01 sec / 60 (DC 4,8V) 0,1 ± 0,01 sec / 60 (DC 6V)
- Corrente a vuoto: 200 ± 20mA (DC 4,8V) 220 ± 20mA (DC 6V)
- Coppia di stallo: 1,3 ± 0,01kg · cm (DC 4,8V) 1,5 ± 0,1kg · cm (DC 6V)
- Corrente di stallo: ≦ 850mA (DC 4,8V) ≦ 1000mA (DC 6V)
- Corrente in standby: 3 ± 1mA (DC 4,8V) 4 ± 1mA (DC 6V)

**Componenti**

![](media/image-20250908152809268.png)

  **Schema di collegamento：**

![](media/image-20250908152824930.png)**Note sul collegamento:** il filo marrone del servo è collegato a Gnd(G), il filo rosso è collegato a 5v(V) e il filo arancione è collegato al pin digitale 9.

Il servo deve essere collegato a un'alimentazione esterna a causa dell'elevata richiesta di corrente per il suo azionamento. In generale, la corrente della scheda di sviluppo non è sufficiente. Se non viene collegata l'alimentazione esterna, la scheda di sviluppo potrebbe danneggiarsi.

**Codice di Test 1**

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 4.1
Servo
http://www.keyestudio.com
*/
#define servoPin 9  //pin del servo
int pos; //variabile dell'angolo del servo
int pulsewidth; // variabile della larghezza di impulso del servo

void setup() 
{
  pinMode(servoPin, OUTPUT);  //imposta il pin del servo come OUTPUT
  procedure(0); //imposta l'angolo del servo a 0°
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1) // va da 0 gradi a 180 gradi
  { 
    // con incrementi di 1 grado
    procedure(pos);              // dice al servo di andare alla posizione nella variabile 'pos'
    delay(15);                   //controlla la velocità di rotazione del servo
  }
  for (pos = 180; pos >= 0; pos -= 1) // va da 180 gradi a 0 gradi
  { 
    procedure(pos);              // dice al servo di andare alla posizione nella variabile 'pos'
    delay(15);                    
  }
}
// funzione per controllare il servo
void procedure(int myangle) 
{
  pulsewidth = myangle * 11 + 500;  //calcola il valore della larghezza di impulso
  digitalWrite(servoPin,HIGH);
  delayMicroseconds(pulsewidth);   //la durata del livello alto è la larghezza di impulso
  digitalWrite(servoPin,LOW);
  delay((20 - pulsewidth / 1000));  // il ciclo è 20ms, il livello basso dura per il tempo rimanente
}
```

Dopo aver caricato il codice con successo, il servo oscilla nell'intervallo da 0° a 180°.

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 4.2
 servo
 http://www.keyestudio.com
*/
#include <Servo.h>
Servo myservo;  // crea un oggetto servo per controllare un servo
// sulla maggior parte delle schede possono essere creati dodici oggetti servo
int pos = 0;    // variabile per memorizzare la posizione del servo

void setup() 
{
  myservo.attach(9);  // collega il servo sul pin 9 all'oggetto servo
}

void loop() 
{
  for (pos = 0; pos <= 180; pos += 1)  // va da 0 gradi a 180 gradi
  {
    // con incrementi di 1 grado
    myservo.write(pos);              // dice al servo di andare alla posizione nella variabile 'pos'
    delay(15);                       // attende 15ms affinché il servo raggiunga la posizione
  }
  for (pos = 180; pos >= 0; pos -= 1) { // va da 180 gradi a 0 gradi
    myservo.write(pos);              // dice al servo di andare alla posizione nella variabile 'pos'
    delay(15);                       // attende 15ms affinché il servo raggiunga la posizione
  }
}
```

**Risultato del Test**

Dopo aver caricato il codice con successo e acceso l'alimentazione, il servo oscilla nell'intervallo da 0° a 180°.

Il risultato è lo stesso. Di solito lo controlliamo tramite il file di libreria.

**Spiegazione del Codice**

Arduino include **\#include \<Servo.h\>** (funzioni e istruzioni per il servo)

Di seguito sono riportate alcune istruzioni comuni della funzione servo:

1. attach（interfaccia）——Imposta l'interfaccia del servo; sono disponibili le porte 9 e 10.
2. write（angolo）——L'istruzione per impostare l'angolo di rotazione del servo.
3. read（）——L'istruzione per leggere l'angolo del servo; legge il valore del comando di "write()".
4. **Nota:** Il formato di scrittura sopra indicato è "nome variabile servo, istruzione specifica（）", ad esempio: myservo.attach(9).
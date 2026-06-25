# Progetto 1 LED Lampeggia

![](media/image-20250908174750401.png)

**Descrizione**

Per i principianti e gli appassionati, il lampeggio LED è un programma fondamentale. LED, abbreviazione di diodi luminosi, è costituito da composti chimici come Ga, As, P, N e così via. Il LED può lampeggiare in diversi colori alterando il tempo di ritardo nel codice di prova. Quando controllato, alimentando GND e VCC, il LED si accenderà se l'estremità S è a livello alto; tuttavia, si spegnerà.

**Specifiche**

![](./media/image-20250902164418568.png)

- Interfaccia di controllo: porta digitale
- Tensione di lavoro: CC 3,3-5V
- Spaziatura dei pin: 2,54mm
- Colore visualizzazione LED: rosso

**Componenti**

![](./media/image-20250902164804229.png)

**Scheda Sensori V5**

Sarebbe complicato combinare schede di sviluppo Arduino con numerosi sensori. Tuttavia, la scheda sensori V5, compatibile con la scheda di sviluppo Arduino, risolve perfettamente questo problema. Basta impilare la scheda V5 su di essa.

Questa scheda sensori può essere inserita in moduli sensori a 3 pin e mette a disposizione alcuni pin di comunicazione, come comunicazione seriale, IIC e SPI.

**Descrizione dei Pin**

![](./media/image-20250902165027854.png)

**Diagramma di Collegamento**

![](./media/image-20250902165110913.png)

Come si vede dal diagramma sopra, il LED è collegato con D2.

**Codice di Prova**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 1.1
 Blink
 http://www.keyestudio.com
*/
void setup()
{ 
    pinMode(2, OUTPUT);// inizializza il pin digitale 2 come output.
}

void loop() // la funzione loop si esegue ripetutamente all'infinito
{
   digitalWrite(2, HIGH); // accendi il LED (HIGH è il livello di tensione)
   delay(1000); // attendi un secondo
   digitalWrite(2, LOW); // spegni il LED impostando la tensione a LOW
   delay(1000); // attendi un secondo
}
```

**Risultato della Prova**

(Ci sarà una contraddizione sulla comunicazione seriale tra il codice e il Bluetooth durante il caricamento del codice. Pertanto, non collegare il modulo Bluetooth prima di caricare il codice.)

Carica il programma sulla scheda di sviluppo, il LED lampeggia a intervalli di 1s.

![](./media/image-20250902165335641.png)

**Spiegazione del Codice**

**pinMode(2，OUTPUT) -** Imposta il pin 2 come OUTPUT

**digitalWrite(2，HIGH) -** Quando imposti il pin 2 a livello HIGH (output 5V) o a livello LOW (output 0V)

**Pratica di Estensione**

Abbiamo avuto successo nel far lampeggiare il LED. Ora, osserviamo come cambierà il LED se modifichiamo i pin e il tempo di ritardo.

**Diagramma di Collegamento**

![](./media/image-20250902165631206.png)

Abbiamo modificato i pin e collegato il LED a D10.

**Codice di Prova**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lezione 1.2
 delay
 http://www.keyestudio.com
*/
void setup() // inizializza il pin digitale 10 come output.
{  
   pinMode(10, OUTPUT);
}

// la funzione loop si esegue ripetutamente all'infinito
void loop() 
{
   digitalWrite(10, HIGH); // accendi il LED (HIGH è il livello di tensione)
   delay(100); // attendi 0,1 secondi
   digitalWrite(10, LOW); // spegni il LED impostando la tensione a LOW
   delay(100); // attendi 0,1 secondi
}
```

Il risultato della prova mostra che il LED lampeggia più velocemente. Pertanto, possiamo trarre la conclusione che i pin e il tempo di ritardo influenzano la frequenza di lampeggio.
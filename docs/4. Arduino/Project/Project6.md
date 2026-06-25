# Progetto 6 Ricezione IR

**Descrizione**

Non c'è dubbio che il telecomando a infrarossi sia onnipresente nella vita quotidiana. Viene utilizzato per controllare vari elettrodomestici, come televisori, stereo, videoregistratori e ricevitori di segnali satellitari. Il telecomando a infrarossi è composto da sistemi di trasmissione e ricezione a infrarossi, cioè un telecomando a infrarossi, un modulo ricevitore a infrarossi e un microcontrollore a chip singolo in grado di decodificare.

![](media/image-20250908155801467.png)

Il segnale portante a infrarossi da 38K emesso dal telecomando viene codificato dal chip di codifica nel telecomando. È composto da una sezione di codice pilota, codice utente, codice inverso utente, codice dati e codice inverso dati. L'intervallo di tempo dell'impulso viene utilizzato per distinguere se si tratta di un segnale 0 o 1 e la codifica è composta da questi segnali 0, 1.

Il codice utente dello stesso telecomando rimane invariato mentre il codice dati può distinguere il tasto.

Quando si preme il pulsante del telecomando, il telecomando invia un segnale portante a infrarossi. Quando il ricevitore IR riceve il segnale, il programma decodificherà il segnale portante e determinerà quale tasto è stato premuto. L'MCU decodifica il segnale 01 ricevuto, determinando così quale tasto è stato premuto dal telecomando.

Il ricevitore a infrarossi che utilizziamo è un modulo ricevitore a infrarossi. Principalmente composto da una testina ricevitore a infrarossi, è un dispositivo che integra ricezione, amplificazione e demodulazione. Il suo IC interno ha completato la demodulazione e può ottenere dalla ricezione a infrarossi all'output ed è compatibile con i segnali TTL.

Inoltre, è adatto per il telecomando a infrarossi e la trasmissione dati a infrarossi. Il modulo ricevente a infrarossi realizzato dal ricevitore ha solo tre pin: linea di segnale, VCC e GND. È molto conveniente per comunicare con Arduino e altri microcontrollori.

**Specifiche**

![](media/image-20250908160124669.png)

![](media/image-20250908160132699.png)

- Tensione di funzionamento: 3,3-5V（DC）
- Interfaccia: 3PIN
- Segnale di uscita: Segnale digitale
- Angolo di ricezione: 90 gradi
- Frequenza: 38khz
- Distanza di ricezione: 10m

**Componenti**

![](media/image-20250908160309873.png)

**Diagramma di collegamento**

![](media/image-20250908160331260.png)

Collegare rispettivamente "-", "+" e S del modulo ricevitore IR con G(GND), V(VCC) e A0 della scheda di sviluppo keyestudio.

**Attenzione:** Nel caso in cui le porte digitali non siano disponibili, le porte analogiche possono essere considerate come porte digitali. A0 equivale a D14, A1 è equivalente al digitale 15.

**Codice di test**

Innanzitutto importare il file della libreria del modulo ricevitore IR (fare riferimento a come importare il file della libreria Arduino) prima di progettare il codice.

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 6
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>     // Dichiarazione della libreria IRremote
int RECV_PIN = A0;        // Definire i pin del ricevitore IR come A0
IRrecv irrecv(RECV_PIN);   
decode_results results;   // I risultati della decodifica esistono in "result" di "decode results"
void setup()  
  {
      Serial.begin(9600);  
      irrecv.enableIRIn(); // Abilita il ricevitore
  }  
 void loop() {  
    if (irrecv.decode(&results))// Decodifica riuscita, ricevi un set di segnali a infrarossi
    {  
      Serial.println(results.value, HEX);// Avvolgi la parola in 16 HEX per output e ricevi il codice
      irrecv.resume(); // Ricevi il valore successivo
    }  
    delay(100);  
  }
```

**Risultato del test**

Carica il codice di test, apri il monitor seriale e imposta la velocità in baud a 9600, punta il telecomando al ricevitore IR e verrà visualizzato il valore corrispondente. Se premi a lungo, appariranno codici di errore.

![](media/image-20250908160550590.png)

Di seguito abbiamo elencato il valore di ogni pulsante del telecomando keyestudio. Puoi conservarlo come riferimento.

![](media/image-20250908160603853.png)

**Spiegazione del codice**

**irrecv.enableIRIn():** dopo aver abilitato la decodifica IR, i segnali IR verranno ricevuti, quindi la funzione "decode()" controllerà continuamente se la decodifica è riuscita.

**irrecv.decode(&results):** dopo aver decodificato con successo, questa funzione tornerà a "true" e manterrà il risultato in "results". Dopo aver decodificato un segnale IR, esegui la funzione resume() e ricevi il segnale successivo.

**Pratica di estensione**

Abbiamo decodificato il valore del tasto del telecomando IR. Che ne dici di controllare il LED dal valore misurato? Potremmo eseguire un esperimento per confermarlo. Collega un LED a D10, quindi premi i tasti del telecomando per accendere e spegnere il LED.

![](media/image-20250908160749345.png)

```c
/* keyestudio Mini Tank Robot V2.1
lesson 6.2
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>
int RECV_PIN = A0;// Definire il pin del ricevitore IR come A0
int LED_PIN=10;// Definire il pin del LED
int a=0;
IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
  Serial.begin(9600);
  irrecv.enableIRIn(); // Inizializza il ricevitore IR
  pinMode(LED_PIN,OUTPUT);// Imposta il pin del LED a 4
}

void loop() 
{
  if (irrecv.decode(&results)) 
  {
	Serial.println(results.value, HEX);// Avvolgi la parola in 16 HEX per output e ricevi il codice
	if(results.value==0xFF02FD &a==0) // Secondo il valore del tasto sopra, premi "OK" sul telecomando, il LED sarà controllato
	{
		digitalWrite(LED_PIN,HIGH);// Il LED si accenderà
		a=1;
	}
	else if(results.value==0xFF02FD &a==1) // Premi di nuovo
	{
        digitalWrite(LED_PIN,LOW);// Il LED si spegnerà
        a=0;	
	}
	irrecv.resume(); // Ricevi il valore successivo
  }
}
```

Carica il codice sulla scheda di sviluppo, premi il tasto "OK" sul telecomando per accendere e spegnere il LED.
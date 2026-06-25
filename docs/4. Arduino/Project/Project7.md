# Progetto 7 Controllo Remoto Bluetooth

**Descrizione**

Il Bluetooth, un semplice modulo di comunicazione wireless, è diventato virale negli ultimi decenni ed è stato utilizzato nella maggior parte dei dispositivi alimentati a batteria grazie alla sua funzione facile da usare.

![](media/image-20250908161056017.png)

Nel corso degli anni, ci sono stati molti aggiornamenti dello standard Bluetooth per soddisfare le esigenze dei clienti e lo sviluppo della tecnologia, nonché per seguire le tendenze dei tempi.

Negli ultimi anni, molte cose sono cambiate, inclusa la velocità di trasmissione dei dati, il consumo energetico con dispositivi indossabili e IoT e il sistema di sicurezza.

Qui impareremo a conoscere HM-10 BLE 4.0 con Arduino Board. L'HM-10 è un modulo Bluetooth 4.0 facilmente disponibile. Questo modulo viene utilizzato per stabilire la comunicazione dati wireless. Il modulo è progettato utilizzando il System on Chip (SoC) Bluetooth low energy (BLE) Texas Instruments CC2540 o CC2541.

**Specifiche**

- Protocollo Bluetooth: Bluetooth Specification V4.0 BLE.
- Nessun limite di byte nella trasmissione della porta seriale.
- In ambiente aperto, realizza comunicazione a ultra-distanza di 100m con iPhone4s.
- Frequenza di lavoro: banda ISM 2.4GHz.
- Metodo di modulazione: GFSK (Gaussian Frequency Shift Keying).
- Potenza di trasmissione: -23dbm, -6dbm, 0dbm, 6dbm, modificabile tramite comando AT.
- Sensibilità: ≤-84dBm a 0.1% BER.
- Velocità di trasmissione: Asincrona: 6K byte; Sincrona: 6k byte.
- Funzione di sicurezza: Autenticazione e crittografia.
- Servizio supportato: Central & Peripheral UUID FFE0, FFE1.
- Consumo energetico: Modalità sleep automatica, corrente di standby 400uA~800uA, 8.5mA durante la trasmissione.
- Alimentazione: 5V DC.
- Temperatura di lavoro: –5 a +65 gradi Celsius.

**Componenti**

![](media/image-20250908161515087.png)

**Diagramma di Connessione**

**1. STATE:** *pin di test dello stato, collegato al LED interno, generalmente mantenerlo scollegato.*

**2. RXD:** *interfaccia seriale, terminale di ricezione.*

**3. TXD:** *interfaccia seriale, terminale di trasmissione.*

**4. GND:** *Terra.*

**5. VCC:** *polo positivo della fonte di alimentazione.*

**6. EN/BRK:** *interruzione della connessione, significa interrompere la connessione Bluetooth, generalmente mantenerlo scollegato.*

![](media/image-20250908161703926.png)

**Codice di Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 7.1
 bluetooth 
http://www.keyestudio.com
*/

char ble_val; //variabile carattere: salva il valore della ricezione Bluetooth

void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  //assicurati se ci sono dati nel buffer seriale
  {
    ble_val = Serial.read();  //Leggi i dati dal buffer seriale
    Serial.println(ble_val);  //Stampa
  }
}
//*******************************************
```

(Ci sarà una contraddizione tra la comunicazione seriale del codice e la comunicazione Bluetooth durante il caricamento del codice. Pertanto, non collegare il modulo Bluetooth prima di caricare il codice.)

Dopo aver caricato il codice sulla scheda di sviluppo, inserire il modulo Bluetooth e attendere il comando dal cellulare.

**Scarica l'APP**

Il codice serve per leggere il segnale ricevuto, e abbiamo anche bisogno di qualcosa per inviare il segnale. In questo progetto, inviamo il segnale per controllare l'auto robot tramite cellulare.

Allora abbiamo bisogno di scaricare l'APP.

**Sistema iOS**

**Nota: Consenti all'APP di accedere alla "posizione" nelle impostazioni del tuo cellulare quando ti connetti al modulo Bluetooth. Altrimenti, il Bluetooth potrebbe non connettersi.**

Accedi all'APP STORE e cerca **BLE Scanner 4.0, quindi scaricalo.**

![](media/image-20250908162043691.png)

**Sistema Android**

Scarica l'APP da qui.

**E consenti all'APP di accedere alla "posizione", puoi abilitare la "posizione" nelle impostazioni del tuo cellulare.**

![](media/image-20250909115039773.png)

![](media/image-20250908162115901.png)

1. Dopo l'installazione, apri l'App e abilita il permesso "Posizione e Bluetooth".
2. Prendiamo la versione iOS come esempio. Il metodo operativo della versione Android è quasi lo stesso.
3. Scansiona il modulo Bluetooth per ottenere Bluetooth BLE 4.0. Il suo nome è HMSoft. Quindi fai clic su "connetti" per collegare il Bluetooth e utilizzarlo.

![](media/image-20250908162157692.png)

4. Dopo la connessione a HMSoft, fai clic su di esso per ottenere più opzioni, come informazioni sul dispositivo, permesso di accesso, generale e servizio personalizzato. Scegli "CUSTOM SERVICE".

![](media/image-20250908162224719.png)

5. Quindi appare la seguente pagina.

![](media/image-20250908162314786.png)

6. Fai clic su (Read, Notify, WriteWithoutResponse) per accedere alla seguente pagina.

![](media/image-20250908162335862.png)

7. Fai clic su **Write Value, appare l'interfaccia per inserire HEX o Testo.**

![](media/image-20250908162354140.png)

8. Apri il monitor seriale su Arduino, inserisci uno 0 o un altro carattere nell'interfaccia Testo.

   ![](media/image-20250908162413278.png)

9. Quindi fai clic su "Write", apri il monitor seriale per verificare se c'è un segnale "0".

   ![](media/image-20250908162441251.png)

**Spiegazione del Codice**

**Serial.available()** : I caratteri rimanenti attuali quando ritornano all'area buffer. Generalmente, questa funzione viene utilizzata per giudicare se ci sono dati nel buffer. Quando Serial.available()>0, significa che la seriale ha ricevuto i dati e può essere letta.

**Serial.read()：**Leggi un dato di un Byte nel buffer della porta seriale, ad esempio, il dispositivo invia dati ad Arduino tramite la porta seriale, quindi potremmo leggere i dati tramite "Serial.read()".

**Pratica di Estensione**

Potremmo inviare un comando tramite cellulare per accendere e spegnere un LED.

D10 è collegato a un LED, come mostrato di seguito:

![](media/image-20250908162550263.png)

**Spiegazione del Codice**

**Serial.available()** : I caratteri rimanenti attuali quando ritornano all'area buffer. Generalmente, questa funzione viene utilizzata per giudicare se ci sono dati nel buffer. Quando Serial.available()>0, significa che la seriale ha ricevuto i dati e può essere letta.

**Serial.read()：**Leggi un dato di un Byte nel buffer della porta seriale, ad esempio, il dispositivo invia dati ad Arduino tramite la porta seriale, quindi potremmo leggere i dati tramite "Serial.read()".

**Pratica di Estensione**

Potremmo inviare un comando tramite cellulare per accendere e spegnere un LED.

D10 è collegato a un LED, come mostrato di seguito:

![](media/image-20250908162720671.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 7.2
 Bluetooth 
 http://www.keyestudio.com
*/ 
int ledpin=11;
void setup()
{Serial.begin(9600);
 pinMode(ledpin,OUTPUT);
}
void loop()
{ int i;
  if (Serial.available())
  {i=Serial.read();
    Serial.println("DATI RICEVUTI:");
    if(i=='1')
    { digitalWrite(ledpin,1);
      Serial.println("led acceso");
    }
    if(i=='0')
    { digitalWrite(ledpin,0);
      Serial.println("led spento");
    }
  }
}//*******************************************
```

![](media/image-20250908162739769.png)

![](media/image-20250908162747210.png)

Fai clic su "Write" sull'APP, quando inserisci 1, il LED si accenderà; quando inserisci 0, il LED si spegnerà. (Ricorda di rimuovere il modulo Bluetooth dopo aver terminato l'esperimento, altrimenti il caricamento del codice sarà influenzato).
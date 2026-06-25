# 2. Installazione del prodotto

Dopo aver verificato tutti i componenti in questo kit, è necessario montare il robot carro armato. Installiamo l'auto intelligente seguendo le seguenti istruzioni.

## Video di assemblaggio

[Scarica video](video.7z).

> **Nota:** Il video di assemblaggio è fornito nel file `video.7z` incluso in questo pacchetto. Estrarlo per visualizzare `video/KS0605.mp4`.

**Nota: Rimuovere la pellicola di plastica dalla scheda prima di installare l'auto intelligente.**

## Passaggio 1: Installare il motore inferiore

Preparare i seguenti componenti:

- Dado M4 \* 2
- Motore in metallo \*2
- Supporto in metallo \*2
- Accoppiatore \*2
- Parti di supporto blu \*2
- Vite a brugola interna M4\*12MM \* 2
- Chiave esagonale M1.5 nichelata \*1
- Chiave esagonale M3 nichelata \*1
- Chiave esagonale M2.5 nichelata \*1
- Vite a brugola interna M3\*8MM \* 4

![TK_02](media/TK_02.png)

![TK_03](media/TK_03.png)

**Suggerimento: assemblare il motore dell'altro lato allo stesso modo.**

## Passaggio 2: Installare la ruota motrice

Preparare i seguenti componenti:

- Vite a brugola interna M3*8MM \* 2
- Vite a brugola interna M4\*50MM \* 2
- Ruota di supporto del carro armato \* 2
- Cuscinetto a flangia \* 4
- Rondella\*2
- Banda cingolata \*2
- Dado autobloccante M4 \* 2
- Chiave esagonale M3 nichelata \*1
- Chiave esagonale M2.5 nichelata \*1

![TK_04](media/TK_04.png)

![TK_05](media/TK_05.png)

![TK_06](media/TK_06.png)

![TK_07](media/TK_07.png)

## Passaggio 3: Installare il supporto della batteria

Preparare i seguenti componenti:

- Supporto batteria \*1
- Dado M3 \* 2
- Supporto in metallo blu \*2
- Dado M4 \*8
- Vite a testa piatta M3\*10MM \* 2
- Vite a brugola interna M4\*40MM \*4
- Chiave esagonale M2.5 nichelata\*1
- Chiave esagonale M3 nichelata \*1
- Vite a brugola interna M3\*25MM \*4
- Pilastro in rame esagonale M3*45MM *4
- Cacciavite

![TK_08](media/TK_08.png)

![TK_09](media/TK_09.png)

Procedere a fissare il supporto in metallo sulla ruota motrice con quattro viti a brugola interna M4\*40MM e quattro dadi M4 al termine del processo di montaggio.

![TK_10](media/TK_10.png)

![TK_11](media/TK_11.png)

![TK_12](media/TK_12.png)

![TK_13](media/TK_13.png)

##  Passaggio 4: Montare la scheda acrilica e i sensori

- Scheda acrilica \* 2
- Staffa a forma di L nera \*1
- Sensore fotocellula \*2
- Modulo ricevitore IR \*1
- Pannello LED 8X16 \*1
- Dado M2 \*4
- Dado M3 \*10
- Vite a brugola interna M3\*6MM \* 8
- Vite a brugola interna M3\*8MM \* 8
- Chiave esagonale M2.5 \*1
- Vite a testa rotonda M3\*12MM \*6
- Boccola in rame esagonale M3\*10MM \*8
- Vite a testa rotonda M2\*10MM \* 4
- Cacciavite

![TK_14](media/TK_14.png)

![TK_15](media/TK_15.png)

![TK_16](media/TK_16.png)

![TK_17](media/TK_17.png)

![TK_18](media/TK_18.png)

![TK_19](media/TK_19.png)

![TK_20](media/TK_20.png)

![TK_21](media/TK_21.png)

![TK_22](media/TK_22.png)

![TK_23](media/TK_23.png)

## Passaggio 5: Installare la piattaforma del servo

Preparare i seguenti componenti:

-   Servo \*1
-   Gimbal nero \*1
-   Fascetta di cablaggio \*2
-   Vite di battuta a croce con testa rotonda M2x8 \*2
-   Sensore ultrasonico \*1
-   Vite M2\*4 \*1
-   Vite M1.2\*5 \*4
-   Cacciavite

**Nota: **per un debug conveniente, mantenere il modulo ultrasonico rivolto in avanti e l'angolo del motore servo a 90°. Pertanto, è necessario impostare il servo a 90° prima di installare la piattaforma del servo.

Impostare il codice a 90 gradi, copiare il codice e caricarlo sulla scheda di sviluppo. L'ingranaggio di sterzo collegato alla porta D9 ruoterà a 90°.

> Per caricare il codice, avrai bisogno dell'Arduino IDE. Installa prima l'Arduino IDE seguendo le sezioni 4.2–4.4. (Download del software, Configurazione di Arduino IDE e Aggiunta della libreria)

```c
#define servoPin 9 //pin servo
int pos; //variabile dell'angolo del servo
int pulsewidth; // variabile della larghezza dell'impulso del servo

void setup() 
{
    pinMode(servoPin, OUTPUT); //imposta il pin del servo su OUTPUT
    procedure(0); //imposta l'angolo del servo a 0°
}

void loop() 
{
	procedure(90); // ordina al servo di andare alla posizione nella variabile 90°
}

// funzione per controllare il servo
void procedure(int myangle) 
{
    pulsewidth = myangle * 11 + 500; //calcola il valore della larghezza dell'impulso
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth); //La durata del livello alto è la larghezza dell'impulso
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000)); // il ciclo è 20ms, il livello basso dura il resto del tempo
}
```

![TK_24](media/TK_24.png)

![](media/image-20250902144145590.png)

**Nota: **Puoi trovare le viti M1.2\*5 all'interno della borsa della piattaforma in plastica.

![TK_25](media/TK_25.png)

![TK_26](media/TK_26.png)

## Passaggio 6: Installare i sensori e le schede

Preparare i seguenti componenti:

- Vite a testa rotonda M3\*6MM \*12
- Scheda L298P \*1
- Scheda V4.0 \*1
- Scheda sensore V5 \*1
- Cacciavite \*1
- Modulo Bluetooth \*1
- Chiave esagonale M2.5 nichelata \*1

![TK_27](media/TK_27.png)

![TK_28](media/TK_28.png)

![TK_29](media/TK_29.png)

![TK_30](media/TK_30.png)

![TK_31](media/TK_31.png)

![TK_32](media/TK_32.png)

![TK_33](media/TK_33.png)

![TK_34](media/TK_34.png)



## Passaggio 7: Guida ai collegamenti

![](media/image-20250902144534790.png)

![](media/image-20250902144551034.png)

![](media/image-20250902144559983.png)

![](media/image-20250902144849310.png)

![](media/image-20250902144902221.png)

##  Passaggio 8: Collegare il pannello LED

![](media/image-20250902145026905.png)

![](media/image-20250902145112884.png)

![](media/image-20250902145129382.png)

| Pannello LED                           | Scheda sensore V5                      |
| -------------------------------------- | -------------------------------------- |
| GND                                    | -(GND)                                 |
| VCC                                    | +(VCC)                                 |
| SDA                                    | SDA                                    |
| SCL                                    | SCL                                    |
| ![](media/image-20250902145404151.png) | ![](media/image-20250902145414755.png) |

## Passaggio 9: Installare tutti i componenti della piastra acrilica

![](media/image-20250902145506652.png)

![](media/image-20250902145615504.png)

![](media/image-20250902145822634.png)

![](media/image-20250902145854886.png)

![](media/image-20250902145934002.png)

![](media/image-20250902150004173.png)

![](media/image-20250902150032438.png)

![](media/image-20250902150052468.png)

![](media/image-20250902150217564.png)

![](media/image-20250902150508905.png)

![](media/image-20250902150522753.png)

![](media/image-20250902150532987.png)

![](media/image-20250902150711706.png)

##  Passaggio 10: Robot carro armato

**Nota:** Rimuovere il modulo Bluetooth prima di caricare il codice di test. Altrimenti, non riuscirai a caricare il codice di test.

![](media/image-20250902151034545.png)

**Auto robot multifunzione**

![](media/image-20250902151133169.png)

  **Descrizione**

Nei progetti precedenti, l'auto carro armato esegue una sola funzione. Tuttavia, in questa lezione, integriamo tutte le sue funzioni per controllare l'auto intelligente tramite il controllo Bluetooth.

Ecco un semplice diagramma di flusso dell'auto robot multifunzione per il vostro riferimento.

![](media/image-20250902151215210.png)

  **Diagramma di collegamento**

![](media/image-20250902151230702.png)

**Attenzione：**Confermare che ogni componente sia collegato.

Guida ai collegamenti:

| Pannello LED 8x16 | | Scheda di espansione |
| -------------- | ---- | --------------- |
| GND            | →    | -（GND）        |
| VCC            | →    | +（VCC）        |
| SDA            | →    | SDA             |
| SCL            | →    | SCL             |

![](media/image-20250902152539713.png)

| Modulo ultrasonico |      |        |
| ----------------- | ---- | ------ |
| VCC               | →    | 5v(V)  |
| Trig              | →    | 5(S)   |
| Echo              | →    | 4(S)   |
| Gnd               | →    | Gnd(G) |

![](media/image-20250902152857086.png)

![](media/image-20250902152906103.png)

| Motore servo |      |        |
| ----------- | ---- | ------ |
| Motore servo | →    | Gnd(G) |
| Filo rosso    | →    | 5v(V)  |
| Filo arancione | →    | 9      |

![](media/image-20250902154418006.png)

![](media/image-20250902154820948.png)

| Modulo Bluetooth                        |      |          |
| --------------------------------------- | ---- | -------- |
| RXD                                     | →    | TX       |
| TXD                                     | →    | RX       |
| GND                                     | →    | -（GND） |
| VCC                                     |      | +（VCC） |
| Non è necessario collegare i pin STATE e BRK |      |          |

![](media/image-20250902155229663.png)

![](media/image-20250902155236836.png)

| Modulo ricevitore IR |      | Scheda sensore |
| ------------------ | ---- | ------------- |
| －                 | →    | G（GND）      |
| +                  | →    | V（VCC）      |
| S                  | →    | A0            |

![](media/image-20250902155444270.png)

![](media/image-20250902155452133.png)

| Fotoresistenza sinistra  |      | Scheda sensore |
| -------------------- | ---- | ------------- |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A1            |
|                      |      |               |
| Fotoresistenza destra |      | Scheda sensore |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A2            |

![](media/image-20250902155938106.png)

![](media/image-20250902155946213.png)

 Installazione completata.
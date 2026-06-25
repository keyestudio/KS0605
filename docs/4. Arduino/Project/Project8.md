# Progetto 8 Controllo del Motore e Regolazione della Velocità

**Descrizione**

![](media/image-20250908162844748.png)

Esistono molti modi per azionare un motore. L'auto robot utilizza la soluzione più comune: L298P, un eccellente circuito integrato driver motore ad alta potenza prodotto da STMicroelectronics. Può azionare direttamente motori CC, motori passo-passo a due fasi e quattro fasi. La corrente di azionamento è fino a 2A e il terminale di uscita del motore utilizza otto diodi Schottky ad alta velocità come protezione.

Abbiamo progettato uno shield basato sul circuito L298P. Il design impilabile riduce la difficoltà tecnica nell'utilizzo e nell'azionamento del motore.

**Specifiche**

Diagramma del circuito per la scheda L298P

![](media/image-20250908163017604.png)

1. Tensione di ingresso della parte logica: CC 5V
2. Tensione di ingresso della parte di azionamento: CC 7-12V
3. Corrente di lavoro della parte logica: <36mA
4. Corrente di lavoro della parte di azionamento: <2A
5. Dissipazione massima di potenza: 25W (T=75℃)
6. Temperatura di lavoro: -25℃～＋130℃
7. Livello di ingresso del segnale di controllo: livello alto 2.3V<Vin<5V, livello basso 0.3V<Vin<1.5V

![](media/image-20250908163151925.png)

**Azionare il Robot per il Movimento**

Attraverso il diagramma del circuito sopra riportato, il pin di direzione del motore A è D12 e il pin di velocità è D3; D13 è il pin di direzione del motore B, D11 è il pin di velocità.

Sappiamo come controllare le porte digitali secondo il seguente grafico.

PWM decide l'accensione di 2 motori per azionare l'auto robot. Il valore PWM è nell'intervallo 0-255. Più grande è il numero, più veloce ruota il motore.

| Robot Tank      | Motore (A)           | Motore (B)           |
| --------------- | -------------------- | -------------------- |
| Avanti          | Ruota in senso orario |                      |
| Indietro        | Ruota in senso antiorario |                  |
| Ruota a sinistra| Ruota in senso antiorario | Ruota in senso orario |
| Ruota a destra  | Ruota in senso orario | Ruota in senso antiorario |
| Arresto         | Arresto              | Arresto              |

**Componenti**

![](media/image-20250908163739200.png)

**Diagramma di Collegamento**

![](media/d35ffe6c0c275548f40bcafb42a93da1.jpeg)

**Nota:** il blocco terminale a 4 pin è contrassegnato con serigrafia 1234. Il filo rosso del motore posteriore destro è collegato al terminale 1, il filo nero è collegato all'estremità 2. Il filo rosso del motore anteriore sinistro è collegato al terminale 3, il filo nero è collegato alla porta 4.

**Codice di Test**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 8.1
 motor driver
 http://www.keyestudio.com
*/ 

#define ML_Ctrl 13  //definire il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definire il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definire il pin di controllo della direzione del motore destro
#define MR_PWM 3   //definire il pin di controllo PWM del motore destro

void setup()
{
  pinMode(ML_Ctrl, OUTPUT);//definire il pin di controllo della direzione del motore sinistro come output
  pinMode(ML_PWM, OUTPUT);//definire il pin di controllo PWM del motore sinistro come output
  pinMode(MR_Ctrl, OUTPUT);//definire il pin di controllo della direzione del motore destro come output
  pinMode(MR_PWM, OUTPUT);//definire il pin di controllo PWM del motore destro come output
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//impostare il pin di controllo della direzione del motore sinistro a LOW
  analogWrite(ML_PWM,200);//impostare la velocità di controllo PWM del motore sinistro a 200
  digitalWrite(MR_Ctrl,LOW);//impostare il pin di controllo della direzione del motore destro a LOW
  analogWrite(MR_PWM,200);//impostare la velocità di controllo PWM del motore destro a 200

  //avanti
  delay(2000);//ritardo di 2s
   digitalWrite(ML_Ctrl,HIGH);//impostare il pin di controllo della direzione del motore sinistro a HIGH
  analogWrite(ML_PWM,200);//impostare la velocità di controllo PWM del motore sinistro a 200  
digitalWrite(MR_Ctrl,HIGH);//impostare il pin di controllo della direzione del motore destro a HIGH
  analogWrite(MR_PWM,200);//impostare la velocità di controllo PWM del motore destro a 200

   //indietro
  delay(2000);//ritardo di 2s 
  digitalWrite(ML_Ctrl,HIGH);//impostare il pin di controllo della direzione del motore sinistro a HIGH
  analogWrite(ML_PWM,200);//impostare la velocità di controllo PWM del motore sinistro a 200
  digitalWrite(MR_Ctrl,LOW);//impostare il pin di controllo della direzione del motore destro a LOW
  analogWrite(MR_PWM,200);//impostare la velocità di controllo PWM del motore destro a 200

    //sinistra
  delay(2000);//ritardo di 2s
   digitalWrite(ML_Ctrl,LOW);//impostare il pin di controllo della direzione del motore sinistro a LOW
  analogWrite(ML_PWM,200);//impostare la velocità di controllo PWM del motore sinistro a 200
  digitalWrite(MR_Ctrl,HIGH);//impostare il pin di controllo della direzione del motore destro a HIGH
  analogWrite(MR_PWM,200);//impostare la velocità di controllo PWM del motore destro a 200

   //destra
  delay(2000);//ritardo di 2s
  analogWrite(ML_PWM,0);//impostare la velocità di controllo PWM del motore sinistro a 0
  analogWrite(MR_PWM,0);//impostare la velocità di controllo PWM del motore destro a 0

    //arresto
  delay(2000);//ritardo di 2s
}//*****************************************
```

**Risultato del Test**

Collegare secondo il diagramma di collegamento, caricare il codice e accendere l'alimentazione. L'auto intelligente avanza e indietreggia per 2s, gira a sinistra e a destra per 2s, si arresta per 2s e così via alternativamente.

**Spiegazione del Codice**

**digitalWrite(ML_Ctrl,LOW):** la direzione di rotazione del motore è determinata dal livello alto/basso e i pin che determinano la direzione di rotazione sono pin digitali.

**analogWrite(ML_PWM,200):** la velocità del motore è regolata da PWM e i pin che determinano la velocità del motore devono essere pin PWM.

**Pratica di Estensione**

Regolare la velocità controllata da PWM del motore e collegare nello stesso modo.

![](media/d35ffe6c0c275548f40bcafb42a93da1-1757321401643-2.jpeg)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 8.2
 motor driver pwm
 http://www.keyestudio.com
*/ 
#define ML_Ctrl 13  //definire il pin di controllo della direzione del motore sinistro
#define ML_PWM 11   //definire il pin di controllo PWM del motore sinistro
#define MR_Ctrl 12  //definire il pin di controllo della direzione del motore destro
#define MR_PWM 3   //definire il pin di controllo PWM del motore destro

void setup()
{ 
  pinMode(ML_Ctrl, OUTPUT);//definire il pin di controllo della direzione del motore sinistro come OUTPUT
  pinMode(ML_PWM, OUTPUT);//definire il pin di controllo PWM del motore sinistro come OUTPUT
  pinMode(MR_Ctrl, OUTPUT);//definire il pin di controllo della direzione del motore destro come OUTPUT
  pinMode(MR_PWM, OUTPUT);//definire il pin di controllo PWM del motore destro come OUTPUT
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//Impostare il pin di controllo della direzione del motore sinistro a LOW
  analogWrite(ML_PWM,100);//Impostare la velocità di controllo PWM del motore sinistro a 100
  digitalWrite(MR_Ctrl,LOW);//Impostare il pin di controllo della direzione del motore destro a LOW
  analogWrite(MR_PWM,100);//Impostare la velocità di controllo PWM del motore destro a 100
  //avanti
  delay(2000);//definire 2s
  digitalWrite(ML_Ctrl,HIGH);//Impostare il pin di controllo della direzione del motore sinistro a livello HIGH
  analogWrite(ML_PWM,250);//Impostare la velocità di controllo PWM del motore sinistro a 100
  digitalWrite(MR_Ctrl,HIGH);//Impostare il pin di controllo della direzione del motore destro a livello HIGH
  analogWrite(MR_PWM,250);//Impostare la velocità di controllo PWM del motore destro a 100
   //indietro
  delay(2000);//definire 2s
  digitalWrite(ML_Ctrl,HIGH);//Impostare il pin di controllo della direzione del motore sinistro a livello HIGH
  analogWrite(ML_PWM,250);//Impostare la velocità di controllo PWM del motore sinistro a 100
  digitalWrite(MR_Ctrl,LOW);//Impostare il pin di controllo della direzione del motore destro a livello LOW
  analogWrite(MR_PWM,250);//Impostare la velocità di controllo PWM del motore destro a 100
    //sinistra
  delay(2000);//definire 2s
   digitalWrite(ML_Ctrl,LOW);//impostare il pin di controllo della direzione del motore sinistro a LOW
  analogWrite(ML_PWM,250);//impostare la velocità di controllo PWM del motore sinistro a 200
  digitalWrite(MR_Ctrl,HIGH);//impostare il pin di controllo della direzione del motore destro a HIGH
  analogWrite(MR_PWM,250);//impostare la velocità di controllo PWM del motore destro a 100
   //destra
  delay(2000);//definire 2s
  analogWrite(ML_PWM,0);//impostare la velocità di controllo PWM del motore sinistro a 0
  analogWrite(MR_PWM,0);//impostare la velocità di controllo PWM del motore destro a 0

    //arresto
  delay(2000);//definire 2s
}//******************************************************************
```

Caricamento del codice completato con successo, i motori ruotano più velocemente.
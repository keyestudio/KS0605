# Proyecto 6 Recepción IR

**Descripción**

Sin duda, el control remoto por infrarrojo es omnipresente en la vida cotidiana. Se utiliza para controlar diversos electrodomésticos, como televisores, equipos de sonido, videograbadoras y receptores de señales de satélite. El control remoto por infrarrojo está compuesto por sistemas de transmisión y recepción infrarroja, es decir, un control remoto infrarrojo, un módulo receptor infrarrojo y un microcontrolador capaz de decodificar.

![](media/image-20250908155801467.png)

La señal portadora infrarroja de 38K emitida por el controlador remoto se codifica mediante el chip de codificación en el controlador remoto. Consiste en una sección de código piloto, código de usuario, código inverso de usuario, código de datos y código inverso de datos. El intervalo de tiempo del pulso se utiliza para distinguir si es una señal 0 o 1, y la codificación se compone de estas señales 0 y 1.

El código de usuario del mismo control remoto no cambia, mientras que el código de datos puede distinguir la tecla.

Cuando se presiona el botón del control remoto, el control remoto emite una señal portadora infrarroja. Cuando el receptor IR recibe la señal, el programa decodificará la señal portadora y determinará qué tecla se presionó. El MCU decodifica la señal 01 recibida, determinando así qué tecla fue presionada en el control remoto.

El receptor infrarrojo que utilizamos es un módulo receptor infrarrojo. Compuesto principalmente por una cabeza receptora infrarroja, es un dispositivo que integra recepción, amplificación y demodulación. Su IC interno ha completado la demodulación y puede lograr desde la recepción infrarroja hasta la salida, siendo compatible con señales TTL.

Además, es adecuado para control remoto infrarrojo y transmisión de datos infrarroja. El módulo receptor infrarrojo fabricado por el receptor tiene solo tres pines: línea de señal, VCC y GND. Es muy conveniente para comunicarse con Arduino y otros microcontroladores.

**Especificación**

![](media/image-20250908160124669.png)

![](media/image-20250908160132699.png)

- Voltaje de funcionamiento: 3.3-5V（DC）
- Interfaz: 3PIN
- Señal de salida: Señal digital
- Ángulo de recepción: 90 grados
- Frecuencia: 38khz
- Distancia de recepción: 10m

**Componentes**

![](media/image-20250908160309873.png)

**Diagrama de conexión**

![](media/image-20250908160331260.png)

Conecte respectivamente "-", "+" y S del módulo receptor IR con G(GND), V(VCC) y A0 de la placa de desarrollo keyestudio.

**Atención:** En caso de que no haya puertos digitales disponibles, los puertos analógicos pueden considerarse como puertos digitales. A0 es equivalente a D14, A1 es equivalente al puerto digital 15.

**Código de prueba**

Primero importe el archivo de biblioteca del módulo receptor IR (consulte cómo importar un archivo de biblioteca de Arduino) antes de diseñar el código.

```c
/*
keyestudio Mini Tank Robot V2.1
lección 6
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>     // Declaración de biblioteca IRremote
int RECV_PIN = A0;        // Define el pin del receptor IR como A0
IRrecv irrecv(RECV_PIN);   
decode_results results;   // Los resultados de decodificación existen en "result" de "decode results"
void setup()  
  {
      Serial.begin(9600);  
      irrecv.enableIRIn(); // Habilita el receptor
  }  
 void loop() {  
    if (irrecv.decode(&results))// Decodificación exitosa, recibe un conjunto de señales infrarroja
    {  
      Serial.println(results.value, HEX);// Envuelve la palabra en HEX 16 para salida y recibe código 
      irrecv.resume(); // Recibe el siguiente valor
    }  
    delay(100);  
  }
```

 **Resultado de la prueba**

Cargue el código de prueba, abra el monitor serie y establezca la velocidad en baudios a 9600, apunte el control remoto al receptor IR y se mostrará el valor correspondiente. Si presiona durante mucho tiempo, aparecerán códigos de error.

![](media/image-20250908160550590.png)

A continuación, hemos enumerado el valor de cada botón del control remoto keyestudio. Puede conservarlo como referencia.

![](media/image-20250908160603853.png)

**Explicación del código**

**irrecv.enableIRIn():** después de habilitar la decodificación IR, se recibirán las señales IR, luego la función "decode()" verificará continuamente si la decodificación es exitosa.

**irrecv.decode(&results):** después de decodificar exitosamente, esta función devolverá "true" y mantendrá el resultado en "results". Después de decodificar una señal IR, ejecute la función resume() y reciba la siguiente señal.

**Práctica de extensión**

Hemos decodificado el valor de tecla del control remoto IR. ¿Qué tal controlar el LED por el valor medido? Podríamos realizar un experimento para confirmarlo. Conecte un LED a D10, luego presione las teclas del control remoto para encender y apagar el LED.

![](media/image-20250908160749345.png)

```c
/* keyestudio Mini Tank Robot V2.1
lección 6.2
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>
int RECV_PIN = A0;// Define el pin del receptor IR como A0
int LED_PIN=10;// Define el pin del LED
int a=0;
IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
  Serial.begin(9600);
  irrecv.enableIRIn(); // Inicializa el receptor IR 
  pinMode(LED_PIN,OUTPUT);// Establece el pin del LED a 4
}

void loop() 
{
  if (irrecv.decode(&results)) 
  {
	Serial.println(results.value, HEX);// Envuelve la palabra en HEX 16 para salida y recibe código
	if(results.value==0xFF02FD &a==0) // De acuerdo con el valor de tecla anterior, presione "OK" en el control remoto, el LED será controlado
	{
		digitalWrite(LED_PIN,HIGH);// El LED se encenderá
		a=1;
	}
	else if(results.value==0xFF02FD &a==1) // Presione nuevamente
	{
        digitalWrite(LED_PIN,LOW);// El LED se apagará
        a=0;	
	}
	irrecv.resume(); // Recibe el siguiente valor
  }
}
```

Cargue el código en la placa de desarrollo, presione la tecla "OK" en el control remoto para encender y apagar el LED.
# Proyecto 5 Sensor Ultrasónico

**Descripción**

![](media/image-20250908154003868.png)

El sensor ultrasónico HC-SR04 utiliza sonar para determinar la distancia a un objeto, como lo hacen los murciélagos. Ofrece una detección de rango sin contacto excelente con alta precisión y lecturas estables en un paquete fácil de usar. Viene completo con módulos transmisor y receptor ultrasónicos.

El HC-SR04 o sensor ultrasónico se está utilizando en una amplia gama de proyectos electrónicos para crear aplicaciones de detección de obstáculos y medición de distancia, así como varias otras aplicaciones. Aquí hemos traído el método simple para medir la distancia con Arduino y sensor ultrasónico y cómo usar el sensor ultrasónico con Arduino.

**Especificación**

![](media/image-20250908154036832.png)

- Suministro de energía: +5V DC
- Corriente en reposo: <2mA
- Corriente de funcionamiento: 15mA
- Ángulo efectivo: <15°
- Distancia de medición: 2cm – 400 cm
- Resolución: 0.3 cm
- Ángulo de medición: 30 grados
- Ancho de pulso de entrada de disparo: 10uS

**Componentes**

![](media/image-20250908154147825.png)

**El principio del sensor ultrasónico**

Como se muestra en la imagen anterior, es como dos ojos. Uno es el extremo transmisor, el otro es el extremo receptor.

El módulo ultrasónico emitirá ondas ultrasónicas después de recibir una señal de disparo. Cuando las ondas ultrasónicas encuentran el objeto y se reflejan, el módulo emite una señal de eco, por lo que puede determinar la distancia del objeto a partir de la diferencia de tiempo entre la señal de disparo y la señal de eco.

La t es el tiempo que la señal emitida tarda en encontrar el obstáculo y regresar. La velocidad de propagación del sonido en el aire es aproximadamente 343m/s, y distancia = velocidad × tiempo. Sin embargo, la onda ultrasónica se emite y regresa, lo que es 2 veces la distancia. Por lo tanto, es necesario dividir por 2, la distancia medida por la onda ultrasónica = (velocidad × tiempo)/2.

1. Método de uso y gráfico de temporización del módulo ultrasónico:

2. Establecer el tiempo de retardo del pin Trig del SR04 a 10μs como mínimo, lo que puede activarlo para detectar la distancia.
3. Después del disparo, el módulo enviará automáticamente ocho pulsos ultrasónicos de 40KHz y detectará si hay una señal de retorno. El módulo completará este paso automáticamente.
4. Si la señal regresa, el pin Echo emitirá un nivel alto, y la duración del nivel alto es el tiempo desde la transmisión de la onda ultrasónica hasta el retorno.

![](media/image-20250908154407063.png)

Diagrama de circuito del sensor ultrasónico:

![](media/image-20250908154422828.png)

**Diagrama de conexión**

![](media/image-20250908154455132.png)

Guía de cableado:

- Sensor ultrasónico escudo de sensor keyestudio V5
- VCC → 5v(V)
- Trig → 5(S)
- Echo → 4(S)
- Gnd → Gnd(G)

**Código de prueba**

```c
/*
keyestudio Mini Tank Robot V2.1
lección 5
Sensor ultrasónico
http://www.keyestudio.com
*/
int trigPin = 5; // Disparo
int echoPin = 4; // Eco
long duration, cm, inches;

void setup() 
{
    // Inicio del puerto serie
    Serial.begin (9600);
    // Definir entradas y salidas
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);
}
void loop() 
{
    // El sensor se activa mediante un pulso HIGH de 10 o más microsegundos.
    // Dar un pulso LOW corto de antemano para asegurar un pulso HIGH limpio:
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    // Leer la señal del sensor: un pulso HIGH cuya duración es el tiempo (en microsegundos) desde el envío del ping hasta la recepción de su eco en un objeto.
    duration = pulseIn(echoPin, HIGH);
    // Convertir el tiempo en una distancia
    cm = (duration/2) / 29.1; // Dividir por 29.1 o multiplicar por 0.0343
    inches = (duration/2) / 74; // Dividir por 74 o multiplicar por 0.0135
    Serial.print(inches);
    Serial.print("in, ");
    Serial.print(cm);
    Serial.print("cm");
    Serial.println();
    delay(250);
}
```

**Resultado de la prueba**

Cargue el código de prueba en la placa de desarrollo, abra el monitor serie y establezca la velocidad en baudios a 9600. Se mostrará la distancia detectada, y la unidad es cm e pulgadas. Obstruya el sensor ultrasónico con la mano, el valor de distancia mostrado se hace más pequeño.

![](media/image-20250908154718663.png)

**Explicación del código**

**int trigPin = 5;** este pin se define para transmitir ondas ultrasónicas, generalmente salida.

**int echoPin = 4;** esto se define como el pin de recepción, generalmente entrada.

**cm = (duration/2) / 29.1; inches = (duration/2) / 74; por 0.0135**

Podemos calcular la distancia utilizando la siguiente fórmula:

distancia = (tiempo de viaje/2) × velocidad del sonido

La velocidad del sonido es: 343m/s = 0.0343 cm/uS = 1/29.1 cm/uS

O en pulgadas: 13503.9in/s = 0.0135in/uS = 1/74in/uS

Necesitamos dividir el tiempo de viaje por 2 porque debemos tener en cuenta que la onda se envió, golpeó el objeto y luego regresó al sensor.

**Práctica de extensión:**

Hemos medido la distancia mostrada por el ultrasónico. ¿Qué tal controlar el LED con la distancia medida? Intentémoslo y conectemos un módulo de luz LED al pin D10.

![](media/image-20250908154848028.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 5
 LED ultrasónico
 http://www.keyestudio.com
*/ 
int trigPin = 5;    // Disparo
int echoPin = 4;    // Eco
long duration, cm, inches;

void setup() 
{
  // Inicio del puerto serie
  Serial.begin (9600);
  // Definir entradas y salidas
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() 
{
  // El sensor se activa mediante un pulso HIGH de 10 o más microsegundos.
  // Dar un pulso LOW corto de antemano para asegurar un pulso HIGH limpio:
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Leer la señal del sensor: un pulso HIGH cuya duración es el tiempo (en microsegundos) desde el envío del ping hasta la recepción de su eco en un objeto.
  duration = pulseIn(echoPin, HIGH);
  // Convertir el tiempo en una distancia
  cm = (duration/2) / 29.1;     // Dividir por 29.1 o multiplicar por 0.0343
  inches = (duration/2) / 74;   // Dividir por 74 o multiplicar por 0.0135
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  Serial.print("cm");
  Serial.println();
  delay(250);
  if (cm>=2 && cm<=10)
  	digitalWrite(10, HIGH);
  delay(1000);
  digitalWrite(10, LOW);
  delay(1000);
}
```

Cargue el código de prueba en la placa de desarrollo y bloquee el sensor ultrasónico con la mano, luego verifique si el LED está encendido.
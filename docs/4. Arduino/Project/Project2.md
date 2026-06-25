# Proyecto 2 Ajustar el Brillo del LED

**(1) Descripción**

En la lección anterior, controlamos el encendido y apagado del LED e hicimos que parpadeara.

En este proyecto, controlaremos el brillo del LED a través de PWM para simular efectos de respiración. De manera similar, puede cambiar la longitud del paso y el tiempo de retardo en el código para demostrar diferentes efectos de respiración.

PWM es un medio para controlar la salida analógica por medios digitales. El control digital se utiliza para generar ondas cuadradas con diferentes ciclos de trabajo (una señal que cambia constantemente entre niveles altos y bajos) para controlar la salida analógica. En general, los voltajes de entrada de los puertos son 0V y 5V. ¿Qué pasa si se requieren 3V? ¿O cambiar entre 1V, 3V y 3.5V? No podemos cambiar resistencias constantemente. Por esta razón, recurrimos a PWM.

![](./media/bbcfcb9ae56abb7e80ee587246fc4be9.GIF)

Para la salida de voltaje del puerto digital de Arduino, solo hay LOW y HIGH, que corresponden a la salida de voltaje de 0V y 5V. Puede definir LOW como 0 y HIGH como 1, y dejar que Arduino genere quinientas señales 0 o 1 dentro de 1 segundo.

Si genera quinientas señales 1, eso es 5V; si todas son 1, eso es 0V. Si genera 010101010101 de esta manera, entonces el puerto de salida es 2.5V, que es como ver una película. Las películas que vemos no son completamente continuas. En realidad, genera 25 imágenes por segundo. En este caso, el ser humano no puede notarlo, ni tampoco PWM. Si desea un voltaje diferente, necesita controlar la proporción de señales 0 y 1. Cuantas más señales 0 y 1 genere por unidad de tiempo, más precisamente puede controlar.

**(2) Especificación**

- Interfaz de control: puerto digital
- Voltaje de funcionamiento: DC 3.3-5V
- Espaciado de pines: 2.54mm
- Color de visualización: rojo

**(3) Componentes**

![](./media/image-20250902170952089.png)

 **(4) Diagrama de Conexión**

![](./media/image-20250902171013917.png)

 **(5) Código de Prueba**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 2.2
 pwm-lento
 http://www.keyestudio.com
*/
int ledPin = 10; // Define el pin del LED en D10
int value;

void setup () 
{
	pinMode (ledPin, OUTPUT); // inicializa ledpin como salida.
}

void loop () 
{
    for (value = 0; value <255; value = value + 1)
    {
        analogWrite (ledPin, value); // El LED se ilumina gradualmente
        delay (30); // retardo de 30MS
    }
    for (value = 255; value> 0; value = value-1)
    {
        analogWrite (ledPin, value); // El LED se apaga gradualmente
        delay (30); // retardo de 30MS
	}
}
```

**Resultado de la Prueba**

Después de cargar el código de prueba exitosamente, el LED cambia gradualmente de brillante a oscuro, como la respiración humana, en lugar de encenderse y apagarse inmediatamente.

**Explicación del Código**

Cuando necesitamos repetir algunas sentencias, podemos usar la sentencia FOR.

El formato de la sentencia FOR se muestra a continuación:

![](./media/image-20250902171421873.png)

O secuencia cíclica:

Ronda 1：1 → 2 → 3 → 4

Ronda 2：2 → 3 → 4

…

Hasta que el número 2 no se cumpla, el bucle "for" termina.

Después de conocer este orden, volvamos al código:

**for (int value = 0; value < 255; value=value+1){**

**...**

**}**

**for (int value = 255; value >0; value=value-1){**

**...**

**}**

Las dos sentencias "for" hacen que value aumente de 0 a 255, luego disminuya de 255 a 0, luego aumente a 255, .... bucle infinito.

Hay una nueva función en lo siguiente ----- analogWrite()

Sabemos que el puerto digital solo tiene dos estados: 0 y 1. ¿Entonces cómo enviar un valor analógico a un valor digital? Aquí, se necesita esta función. Observemos la placa Arduino y encontremos 6 pines marcados con "\~" que pueden generar señales PWM.

**El formato de la función es el siguiente:**

**analogWrite(pin,value)**

analogWrite() se utiliza para escribir un valor analógico de 0\~255 para el puerto PWM, por lo que el valor está en el rango de 0\~255. Tenga en cuenta que solo debe escribir en los pines digitales con función PWM, como los pines 3, 5, 6, 9, 10, 11.

PWM es una tecnología para obtener cantidad analógica a través del método digital. El control digital forma una onda cuadrada, y la señal de onda cuadrada solo tiene dos estados: encendido y apagado (es decir, niveles altos o bajos). Al controlar la proporción de la duración del encendido y apagado, se puede simular un voltaje que varía de 0 a 5V. El tiempo de encendido (académicamente denominado nivel alto) se llama ancho de pulso, por lo que PWM también se llama modulación por ancho de pulso.

A través de las siguientes cinco ondas cuadradas, conozcamos más sobre PWM.

![](./media/image-20250902172304373.png)

En la figura anterior, la línea verde representa un período, y el valor de analogWrite() corresponde a un porcentaje que también se llama Ciclo de Trabajo.

El ciclo de trabajo implica que la duración del nivel alto se divide por la duración del nivel bajo en un ciclo. De arriba a abajo, el ciclo de trabajo de la primera onda cuadrada es 0% y su valor correspondiente es 0. El brillo del LED es el más bajo, es decir, apagado. Cuanto más tiempo dure el nivel alto, más brillante será el LED. Por lo tanto, el último ciclo de trabajo es 100%, que corresponde a 255, el LED es más brillante. 25% significa más oscuro.

PWM se utiliza principalmente para ajustar el brillo del LED o la velocidad de rotación del motor.

Juega un papel vital en el control del coche robot inteligente. Creo que no puede esperar para entrar en el próximo proyecto.
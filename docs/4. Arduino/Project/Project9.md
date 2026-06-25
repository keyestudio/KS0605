# Proyecto 9 Tablero LED 8*16

**Descripción**

![](media/image-20250908165452592.png)

Si añades un tablero LED 8\*16 al robot, será increíble. La matriz de puntos 8\*16 de Keyestudio puede cumplir este requisito. Con ella, puedes crear emoticones faciales, patrones u otras pantallas interesantes por ti mismo. Este tablero de luz LED 8\*16 viene con 128 LEDs. Los datos del microprocesador (Arduino) se comunican con el AiP1640 a través de la interfaz de bus de dos hilos, para controlar los 128 LEDs del módulo, que producen los patrones que necesitas en la matriz de puntos. Para facilitar el cableado, se proporciona un cableado HX-2.54 de 4 pines.

**Especificación**

- Voltaje de funcionamiento: CC 3.3-5V
- Pérdida de potencia: 400mW
- Frecuencia de oscilación: 450KHz
- Corriente de conducción: 200mA
- Temperatura de funcionamiento: -40\~80℃
- Método de comunicación: bus de dos hilos

**Componentes**

![](media/image-20250908165719665.png)

**Pantalla de Matriz de Puntos 8*16**

Diagrama del Circuito

![](media/image-20250908165746529.png)

**El principio de la matriz de puntos 8\*16:**

¿Cómo controlar cada luz LED de la matriz de puntos 8\*16? Sabemos que un byte tiene 8 bits, y cada bit es 0 o 1. Cuando un bit es 0, se apaga el LED y cuando un bit es 1, se enciende el LED. De esta manera, un byte puede controlar el LED en una fila de la matriz de puntos, por lo que 16 bytes pueden controlar 16 columnas de luces LED, es decir, una matriz de puntos 8\*16.

**Descripción de la Interfaz y Protocolo de Comunicación:**

Los datos del microprocesador (Arduino) se comunican con el AiP1640 a través de la interfaz de bus de dos hilos.

El diagrama del protocolo de comunicación se muestra a continuación:

(SCLK) es SCL, (DIN) es SDA:

![](media/image-20250908165823763.png)

①La condición de inicio para la entrada de datos: SCL está en nivel alto y SDA cambia de alto a bajo.

②Para la configuración del comando de datos, hay métodos como se muestra en la figura a continuación:

En nuestro programa de ejemplo, selecciona la forma de **añadir 1 a la dirección automáticamente**, el valor binario es 0100 0000 y el valor hexadecimal correspondiente es 0x40.

![](media/image-20250908165925549.png)

③Para la configuración del comando de dirección, la dirección se puede seleccionar como se muestra a continuación.

El primer 00H se selecciona en nuestro programa de ejemplo, y el número binario 1100 0000 corresponde al hexadecimal 0xc0.

![](media/image-20250908165938702.png)

④El requisito para la entrada de datos es que SCL esté en nivel alto al introducir datos, y la señal en SDA debe permanecer sin cambios. Solo cuando la señal de reloj en SCL está en nivel bajo, la señal en SDA puede ser alterada. La entrada de datos es de orden bajo primero, orden alto después.

⑤ La condición para terminar la transmisión de datos es que cuando SCL está bajo, SDA está bajo, y cuando SCL está alto, el nivel de SDA también se vuelve alto.

⑥ Control de pantalla, establece diferentes anchos de pulso, el ancho de pulso se puede seleccionar como se muestra a continuación.

En este ejemplo, elegimos ancho de pulso 4/16, y el hexadecimal correspondiente a 1000 1010 es 0x8A.

![](media/image-20250908170005446.png)

4\. Introducción a la Herramienta de Módulo

La versión en línea de la herramienta de módulo de matriz de puntos:

[http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)

①Abre el enlace para entrar en la siguiente página.

![](media/image-20250908170027144.png)

②La matriz de puntos es 8\*16 en este proyecto, así que establece la altura a 8, ancho a 16, como se muestra a continuación.

![](media/image-20250908170040925.png)

③ Genera datos hexadecimales a partir del patrón

Como se muestra a continuación, presiona el botón izquierdo del ratón para seleccionar, el botón derecho para cancelar, dibuja el patrón que deseas, haz clic en **Generate**, y se producirán los datos hexadecimales que necesitamos.

![](media/image-20250908170144895.png)

**Diagrama de Conexión**

![](media/image-20250908170158402.png)

Nota de cableado: El GND, VCC, SDA y SCL del panel LED 8x16 están conectados respectivamente a -(GND), + (VCC), A4 y A5 de la placa de expansión de sensores keyestudio para comunicación serial de dos hilos. (Nota: Este pin está conectado a Arduino IIC, pero este módulo no es comunicación IIC. Puede vincularse con cualquier dos pines.)

**Código de Prueba**

El código que muestra una cara sonriente.

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 9.1
 Cara de matriz
 http://www.keyestudio.com
*/ 
// Los datos de la cara sonriente de la herramienta de módulo
unsigned char smile[] = {0x00, 0x00, 0x1c, 0x02, 0x02, 0x02, 0x5c, 0x40, 0x40, 0x5c, 0x02, 0x02, 0x02, 0x1c, 0x00, 0x00};

#define SCL_Pin  A5  // Establece el pin de reloj a A5
#define SDA_Pin  A4  // Establece el pin de datos a A4

void setup()
{
  // Establece el pin como salida
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // Limpia la pantalla
  // matrix_display(clear);
}

void loop()
{
  matrix_display(smile);  // Muestra cara sonriente
}
// La función para la visualización de matriz de puntos

void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // Usa la función de la condición de inicio de transmisión de datos
  IIC_send(0xc0);  // Selecciona dirección
  
  for(int i = 0;i < 16;i++) // Los datos del patrón tienen 16 bits
  {
     IIC_send(matrix_value[i]); // Transmite los datos del patrón
  }

  IIC_end();   // Termina la transmisión de datos del patrón
  IIC_start();
  IIC_send(0x8A);  // Control de pantalla, establece ancho de pulso a 4/16 s
  IIC_end();
}

// La condición para comenzar a transmitir datos
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Transmite datos
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Cada byte tiene 8 bits, 8 bits para cada carácter
  {
      digitalWrite(SCL_Pin,LOW);  // Baja el pin de reloj SCL_Pin para cambiar la señal de SDA
      delayMicroseconds(3);
      if(send_data & 0x01)  // Establece el nivel alto y bajo de SDA_Pin según 1 o 0 de cada bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // Sube el pin de reloj SCL_Pin para detener la transmisión
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Detecta bit a bit, desplaza los datos a la derecha por uno
  }
}

// El signo de terminar la transmisión de datos
void IIC_end()
{
  digitalWrite(SCL_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
}
//******************************************************
```

**Resultado de la Prueba**

Cableado según el diagrama de conexión. El interruptor DIP se gira hacia el extremo derecho y se enciende, aparece una cara sonriente en la matriz de puntos.

![](media/image-20250908170421220.png)

**Práctica de Extensión**

Usamos la herramienta de módulo ([http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)) para hacer que la matriz de puntos muestre alternativamente patrones de avance, retroceso y parada, luego limpie los patrones, y el intervalo de tiempo es de 2000 milisegundos.

![](media/image-20250908170445861.png)

![](media/image-20250908170452897.png)

![](media/image-20250908170459213.png)

Obtén el código gráfico a mostrar a través de la herramienta de módulo.

**Inicio：** 0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01

**Avanzar：** 0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Retroceder：** 0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**Girar a la izquierda：** 0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00

**Girar a la derecha：** 0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00

**Parar：** 0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00

**Código para limpiar la pantalla**：0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00

El código para múltiples patrones en desplazamiento:

```c
/* keyestudio Mini Tank Robot V2.1
 lección 9.2
 Bucle de matriz
 http://www.keyestudio.com
*/ 
// Matriz, utilizada para almacenar los datos del patrón, puede ser calculada por ti mismo u obtenida de la herramienta de módulo
unsigned char start01[] = 
{0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = 
{0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = 
{0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // Establece el pin de reloj a A5
#define SDA_Pin  A4  // Establece el pin de datos a A4
void setup(){
  // Establece los pines como salida
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // Limpia la pantalla
  matrix_display(clear);
}
void loop(){
  matrix_display(start01);  // Muestra patrón de inicio
  delay(2000);
  matrix_display(front);    // Patrón de avance
  delay(2000);
  matrix_display(STOP01);   // Patrón de parada
  delay(2000);
  matrix_display(clear);    // Limpia la pantalla
  delay(2000);
}
// Esta función se utiliza para la visualización de matriz de puntos
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // Llama a la función de inicio de transmisión de datos
  IIC_send(0xc0);  // Elige dirección
  
  for(int i = 0;i < 16;i++) // Los datos del patrón tienen 16 bits
  {
     IIC_send(matrix_value[i]); // Datos para transmitir patrones
  }
  IIC_end();   // Termina la transmisión de datos del patrón
  IIC_start();
  IIC_send(0x8A);  // Control de pantalla, establece ancho de pulso a 4/16
  IIC_end();
}
// La condición para comenzar a transmitir datos
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
// Transmite datos
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // Cada byte tiene 8 bits
  {
      digitalWrite(SCL_Pin,LOW);  // Baja el pin de reloj SCL para cambiar las señales de SDA
      delayMicroseconds(3);
      if(send_data & 0x01)  // Establece el nivel alto y bajo de SDA_Pin según 1 o 0 de cada bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // Sube el pin de reloj SCL para detener la transmisión de datos
      delayMicroseconds(3);
      send_data = send_data >> 1;  // Detecta bit a bit, desplaza los datos a la derecha por uno
  }}
// El signo de que la transmisión de datos termina
void IIC_end()
{
  digitalWrite(SCL_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);} 
//*****************************************************
```

Carga el código en la placa de desarrollo, la matriz de puntos 8\*16 muestra patrones de avance, retroceso y parada, alternativamente.

![](media/image-20250908170902116.png)

![](media/image-20250908170913925.png)

![](media/image-20250908170921862.png)
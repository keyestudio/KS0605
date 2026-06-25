# 2. Instalación del producto

Después de verificar todas las piezas en este kit, necesitamos montar el robot tanque. Instalemos el coche inteligente siguiendo las siguientes instrucciones.

## Vídeo de montaje

[Descargar vídeo](video.7z).

> **Nota:** El vídeo de montaje se proporciona en el archivo `video.7z` incluido en este paquete. Por favor, extráigalo para ver `video/KS0605.mp4`.

**Nota: Retire la película de plástico de la placa primero al instalar el coche inteligente.**

## Paso 1: Instalar el motor inferior

Prepare las piezas de la siguiente manera:

- Tuerca M4 \* 2
- Motor de metal \*2
- Soporte de metal \*2
- Acoplador \*2
- Piezas de soporte azul \*2
- Tornillo hexagonal interior M4\*12MM \* 2
- Llave Allen hexagonal niquelada M1.5 \*1
- Llave Allen hexagonal niquelada M3 \*1
- Llave Allen hexagonal niquelada M2.5 \*1
- Tornillo hexagonal interior M3\*8MM \* 4

![TK_02](media/TK_02.png)

![TK_03](media/TK_03.png)

**Indicación: Ensamble el motor del otro lado de la misma manera.**

## Paso 2: Instalar la rueda motriz

Prepare las piezas de la siguiente manera:

- Tornillo hexagonal interior M3*8MM \* 2
- Tornillo hexagonal interior M4\*50MM \* 2
- Rueda de carga del tanque \* 2
- Rodamiento de brida \* 4
- Arandela\*2
- Banda de oruga \*2
- Tuerca autoblocante M4 \* 2
- Llave Allen hexagonal niquelada M3 \*1
- Llave Allen hexagonal niquelada M2.5 \*1

![TK_04](media/TK_04.png)

![TK_05](media/TK_05.png)

![TK_06](media/TK_06.png)

![TK_07](media/TK_07.png)

## Paso 3: Instalar el soporte de batería

Prepare las piezas de la siguiente manera:

- Soporte de batería \*1
- Tuerca M3 \* 2
- Soporte de metal azul \*2
- Tuerca M4 \*8
- Tornillo de cabeza plana M3\*10MM \* 2
- Tornillo hexagonal interior M4\*40MM \*4
- Llave Allen hexagonal niquelada M2.5 \*1
- Llave Allen hexagonal niquelada M3 \*1
- Tornillo hexagonal interior M3\*25MM \*4
- Pilar de cobre hexagonal M3*45MM *4
- Destornillador

![TK_08](media/TK_08.png)

![TK_09](media/TK_09.png)

Mueva para fijar el soporte de metal en la rueda del motor con cuatro tornillos hexagonales interiores M4\*40MM y cuatro tuercas M4 cuando se complete el proceso de montaje.

![TK_10](media/TK_10.png)

![TK_11](media/TK_11.png)

![TK_12](media/TK_12.png)

![TK_13](media/TK_13.png)

##  Paso 4: Montar la placa acrílica y los sensores

- Placa acrílica \* 2
- Soporte negro tipo L \*1
- Sensor fotoeléctrico \*2
- Módulo receptor IR \*1
- Panel LED 8X16 \*1
- Tuerca M2 \*4
- Tuerca M3 \*10
- Tornillo hexagonal interior M3\*6MM \* 8
- Tornillo hexagonal interior M3\*8MM \* 8
- Llave Allen hexagonal M2.5 \*1
- Tornillo de cabeza redonda M3\*12MM \*6
- Casquillo de cobre hexagonal M3\*10MM \*8
- Tornillo de cabeza redonda M2\*10MM \* 4
- Destornillador

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

## Paso 5: Instalar la plataforma del servo

Prepare las piezas de la siguiente manera:

-   Servo \*1
-   Cardán negro \*1
-   Brida de cable \*2
-   Tornillo de rosca de cruz de cabeza redonda M2x8 \*2
-   Sensor ultrasónico \*1
-   Tornillo M2\*4 \*1
-   Tornillo M1.2\*5 \*4
-   Destornillador

**Nota: **Para una depuración conveniente, mantenga el módulo ultrasónico recto hacia adelante y el ángulo del motor servo a 90°. Por lo tanto, necesitamos establecer el servo a 90° antes de instalar la plataforma del servo.

Establezca el código de 90 grados, copie el código y cárguelo en la placa de desarrollo. El engranaje de dirección conectado al puerto D9 girará a 90°.

> Para cargar código, necesitará el Arduino IDE. Por favor, instale primero el Arduino IDE siguiendo las secciones 4.2–4.4. (Descarga de software, Configuración de Arduino IDE y Agregar biblioteca)

```c
#define servoPin 9 //pin del servo
int pos; //variable del ángulo del servo
int pulsewidth; //variable del ancho de pulso del servo

void setup() 
{
    pinMode(servoPin, OUTPUT); //establecer el pin del servo como OUTPUT
    procedure(0); //establecer el ángulo del servo a 0°
}

void loop() 
{
	procedure(90); //indicar al servo que vaya a la posición de 90°
}

//función para controlar el servo
void procedure(int myangle) 
{
    pulsewidth = myangle * 11 + 500; //calcular el valor del ancho de pulso
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth); //la duración del nivel alto es el ancho de pulso
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000)); //el ciclo es 20ms, el nivel bajo dura el resto del tiempo
}
```

![TK_24](media/TK_24.png)

![](media/image-20250902144145590.png)

**Nota: **Puede encontrar tornillos M1.2\*5 dentro de la bolsa de la plataforma de plástico.

![TK_25](media/TK_25.png)

![TK_26](media/TK_26.png)

## Paso 6: Instalar sensores y placas

Prepare las piezas de la siguiente manera:

- Tornillo de cabeza redonda M3\*6MM \*12
- Escudo L298P \*1
- Placa V4.0 \*1
- Escudo de sensor V5 \*1
- Destornillador \*1
- Módulo Bluetooth \*1
- Llave Allen hexagonal niquelada M2.5 \*1

![TK_27](media/TK_27.png)

![TK_28](media/TK_28.png)

![TK_29](media/TK_29.png)

![TK_30](media/TK_30.png)

![TK_31](media/TK_31.png)

![TK_32](media/TK_32.png)

![TK_33](media/TK_33.png)

![TK_34](media/TK_34.png)



## Paso 7: Guía de conexión

![](media/image-20250902144534790.png)

![](media/image-20250902144551034.png)

![](media/image-20250902144559983.png)

![](media/image-20250902144849310.png)

![](media/image-20250902144902221.png)

##  Paso 8: Cableado del panel LED

![](media/image-20250902145026905.png)

![](media/image-20250902145112884.png)

![](media/image-20250902145129382.png)

| Panel LED                              | Escudo de sensor V5                    |
| -------------------------------------- | -------------------------------------- |
| GND                                    | -(GND)                                 |
| VCC                                    | +(VCC)                                 |
| SDA                                    | SDA                                    |
| SCL                                    | SCL                                    |
| ![](media/image-20250902145404151.png) | ![](media/image-20250902145414755.png) |

## Paso 9: Instalar todas las piezas de la placa acrílica

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

##  Paso 10: Robot tanque

**Nota:** Retire el módulo Bluetooth antes de cargar el código de prueba. De lo contrario, no podrá cargar el código de prueba.

![](media/image-20250902151034545.png)

**Coche robot multiusos**

![](media/image-20250902151133169.png)

  **Descripción**

En los proyectos anteriores, el coche tanque solo realiza una función única. Sin embargo, en esta lección, integramos todas sus funciones para controlar el coche inteligente a través del control Bluetooth.

Aquí hay un diagrama de flujo simple del coche robot multiusos para su referencia.

![](media/image-20250902151215210.png)

  **Diagrama de conexión**

![](media/image-20250902151230702.png)

**Atención：**Confirme que cada componente está conectado.

Guía de cableado:

| Panel LED 8x16 | | Placa de expansión |
| -------------- | ---- | --------------- |
| GND            | →    | -（GND）        |
| VCC            | →    | +（VCC）        |
| SDA            | →    | SDA             |
| SCL            | →    | SCL             |

![](media/image-20250902152539713.png)

| Módulo ultrasónico |      |        |
| ----------------- | ---- | ------ |
| VCC               | →    | 5v(V)  |
| Trig              | →    | 5(S)   |
| Echo              | →    | 4(S)   |
| Gnd               | →    | Gnd(G) |

![](media/image-20250902152857086.png)

![](media/image-20250902152906103.png)

| Motor servo |      |        |
| ----------- | ---- | ------ |
| Motor servo | →    | Gnd(G) |
| Cable rojo    | →    | 5v(V)  |
| Cable naranja | →    | 9      |

![](media/image-20250902154418006.png)

![](media/image-20250902154820948.png)

| Módulo Bluetooth                        |      |          |
| --------------------------------------- | ---- | -------- |
| RXD                                     | →    | TX       |
| TXD                                     | →    | RX       |
| GND                                     | →    | -（GND） |
| VCC                                     |      | +（VCC） |
| No es necesario conectar los pines STATE y BRK |      |          |

![](media/image-20250902155229663.png)

![](media/image-20250902155236836.png)

| Módulo receptor IR |      | Escudo de sensor |
| ------------------ | ---- | ------------- |
| －                 | →    | G（GND）      |
| +                  | →    | V（VCC）      |
| S                  | →    | A0            |

![](media/image-20250902155444270.png)

![](media/image-20250902155452133.png)

| Fotorresistencia izquierda  |      | Escudo de sensor |
| -------------------- | ---- | ------------- |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A1            |
|                      |      |               |
| Fotorresistencia derecha |      | Escudo de sensor |
| －                   | →    | G（GND）      |
| ＋                   | →    | V（VCC）      |
| S                    | →    | A2            |

![](media/image-20250902155938106.png)

![](media/image-20250902155946213.png)

 Instalación completada.
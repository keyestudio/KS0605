# Proyecto 7 Control Remoto por Bluetooth

**Descripción**

Bluetooth, un módulo de comunicación inalámbrica simple, se ha popularizado durante las últimas décadas y se utiliza en la mayoría de dispositivos alimentados por batería por su función fácil de usar.

![](media/image-20250908161056017.png)

En los últimos años, ha habido muchas actualizaciones del estándar Bluetooth para satisfacer las demandas de los clientes y el desarrollo de la tecnología, así como para seguir la tendencia de los tiempos.

En los últimos años, muchas cosas han cambiado, incluyendo la velocidad de transmisión de datos, el consumo de energía en dispositivos portátiles e IoT y el sistema de seguridad.

Aquí vamos a aprender sobre HM-10 BLE 4.0 con Arduino Board. El HM-10 es un módulo Bluetooth 4.0 fácilmente disponible. Este módulo se utiliza para establecer comunicación de datos inalámbrica. El módulo está diseñado utilizando el Sistema en Chip (SoC) Bluetooth de bajo consumo CC2540 o CC2541 de Texas Instruments.

**Especificaciones**

- Protocolo Bluetooth: Especificación Bluetooth V4.0 BLE.
- Sin límite de bytes en la transcepcción del puerto serie.
- En ambiente abierto, realiza comunicación de ultra-distancia de 100m con iphone4s.
- Frecuencia de funcionamiento: banda ISM 2.4GHz.
- Método de modulación: GFSK (Gaussian Frequency Shift Keying).
- Potencia de transmisión: -23dbm, -6dbm, 0dbm, 6dbm, puede ser modificada por comando AT.
- Sensibilidad: ≤-84dBm a 0.1% BER.
- Velocidad de transmisión: Asincrónica: 6K bytes; Sincrónica: 6k Bytes.
- Característica de seguridad: Autenticación y encriptación.
- Servicio de soporte: Central & Peripheral UUID FFE0, FFE1.
- Consumo de energía: Modo de sueño automático, corriente de espera 400uA\~800uA, 8.5mA durante la transmisión.
- Suministro de energía: 5V DC.
- Temperatura de funcionamiento: –5 a +65 Centígrados.

**Componentes**

![](media/image-20250908161515087.png)

**Diagrama de Conexión**

**1. STATE:** *pines de prueba de estado, conectados a LED interno, generalmente manténgase desconectado.*

**2. RXD:** *interfaz serie, terminal receptora.*

**3. TXD:** *interfaz serie, terminal transmisora.*

**4. GND:** *Tierra.*

**5. VCC:** *polo positivo de la fuente de alimentación.*

**6. EN/BRK:** *romper conexión, significa romper la conexión Bluetooth, generalmente, manténgase desconectado.*

![](media/image-20250908161703926.png)

**Código de Prueba**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 7.1
 bluetooth 
http://www.keyestudio.com
*/

char ble_val; //variable de carácter: guarda el valor de la recepción Bluetooth

void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  //asegúrate de que hay datos en el búfer serie
  {
    ble_val = Serial.read();  //Lee datos del búfer serie
    Serial.println(ble_val);  //Imprime
  }
}
//*******************************************
```

(Habrá contradicción entre la comunicación serie del código y la comunicación Bluetooth al cargar el código. Por lo tanto, no conectes el módulo Bluetooth antes de cargar el código.)

Después de cargar el código en la placa de desarrollo, inserta el módulo Bluetooth y espera el comando del teléfono celular.

**Descargar APP**

El código es para leer la señal recibida, y también necesitamos algo para enviar la señal. En este proyecto, enviamos señal para controlar el robot mediante el teléfono celular.

Entonces necesitamos descargar la APP.

**Sistema iOS**

**Nota: Permite que la APP acceda a "ubicación" en la configuración de tu teléfono celular al conectarse al módulo Bluetooth. De lo contrario, Bluetooth puede no conectarse.**

Entra en APP STORE para buscar **BLE Scanner 4.0, luego descárgalo.**

![](media/image-20250908162043691.png)

**Sistema Android**

Por favor descarga la APP aquí.

**Y permite que la APP acceda a "ubicación", puedes habilitar "ubicación" en la configuración de tu teléfono celular.**

![](media/image-20250909115039773.png)

![](media/image-20250908162115901.png)

1. Después de la instalación, abre la App y habilita el permiso de "Ubicación y Bluetooth".
2. Tomamos la versión iOS como ejemplo. El método de operación de la versión Android es casi igual.
3. Escanea el módulo Bluetooth para obtener Bluetooth BLE 4.0. Su nombre es HMSoft. Luego haz clic en "conectar" para vincularlo con Bluetooth y úsalo.

![](media/image-20250908162157692.png)

4. Después de conectarse a HMSoft, haz clic en él para obtener múltiples opciones, como información del dispositivo, permiso de acceso, general y servicio personalizado. Elige "CUSTOM SERVICE".

![](media/image-20250908162224719.png)

5. Luego aparece la siguiente página.

![](media/image-20250908162314786.png)

6. Haz clic en (Read, Notify, WriteWithoutResponse) para entrar en la siguiente página.

![](media/image-20250908162335862.png)

7. Haz clic en **Write Value, aparece la interfaz para ingresar HEX o Texto.**

![](media/image-20250908162354140.png)

8. Abre el monitor serie en Arduino, ingresa un 0 u otro carácter en la interfaz de Texto.

   ![](media/image-20250908162413278.png)

9. Luego haz clic en "Write", abre el monitor serie para ver si hay una señal "0".

   ![](media/image-20250908162441251.png)

**Explicación del Código**

**Serial.available()** : Los caracteres restantes actuales cuando se devuelven al área del búfer. Generalmente, esta función se utiliza para determinar si hay datos en el búfer. Cuando Serial.available()\>0, significa que el puerto serie ha recibido los datos y pueden ser leídos.

**Serial.read()：**Lee un dato de un Byte en el búfer del puerto serie, por ejemplo, el dispositivo envía datos a Arduino a través del puerto serie, entonces podemos leer los datos mediante "Serial.read()".

**Práctica de Extensión**

Podemos enviar un comando a través del teléfono celular para encender y apagar un LED.

D10 está conectado a un LED, como se muestra a continuación:

![](media/image-20250908162550263.png)

**Explicación del Código**

**Serial.available()** : Los caracteres restantes actuales cuando se devuelven al área del búfer. Generalmente, esta función se utiliza para determinar si hay datos en el búfer. Cuando Serial.available()\>0, significa que el puerto serie ha recibido los datos y pueden ser leídos.

**Serial.read()：**Lee un dato de un Byte en el búfer del puerto serie, por ejemplo, el dispositivo envía datos a Arduino a través del puerto serie, entonces podemos leer los datos mediante "Serial.read()".

**Práctica de Extensión**

Podemos enviar un comando a través del teléfono celular para encender y apagar un LED.

D10 está conectado a un LED, como se muestra a continuación:

![](media/image-20250908162720671.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 7.2
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
    Serial.println("DATOS RECIBIDOS:");
    if(i=='1')
    { digitalWrite(ledpin,1);
      Serial.println("led encendido");
    }
    if(i=='0')
    { digitalWrite(ledpin,0);
      Serial.println("led apagado");
    }
  }
}//*******************************************
```

![](media/image-20250908162739769.png)

![](media/image-20250908162747210.png)

Haz clic en "Write" en la APP, cuando ingreses 1, el LED se encenderá; cuando ingreses 0, el LED se apagará. (Recuerda remover el módulo Bluetooth después de terminar el experimento, de lo contrario, la carga de código se verá afectada).
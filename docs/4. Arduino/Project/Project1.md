# Proyecto 1 LED Parpadea

![](media/image-20250908174750401.png)

**Descripción**

Para principiantes y entusiastas, el parpadeo de LED es un programa fundamental. LED, la abreviatura de diodos emisores de luz, está compuesto por compuestos químicos como Ga, As, P, N, entre otros. El LED puede parpadear en diversos colores alterando el tiempo de retardo en el código de prueba. Cuando está bajo control, con la alimentación en GND y VCC, el LED se encenderá si el extremo S está en nivel alto; sin embargo, se apagará.

**Especificación**

![](./media/image-20250902164418568.png)

- Interfaz de control: puerto digital
- Voltaje de funcionamiento: DC 3.3-5V
- Espaciado de pines: 2.54mm
- Color de visualización LED: rojo

**Componentes**

![](./media/image-20250902164804229.png)

**Escudo de Sensor V5**

Sería problemático cuando combinamos placas de desarrollo Arduino con numerosos sensores. Sin embargo, el escudo de sensor V5, compatible con la placa de desarrollo Arduino, resuelve este problema perfectamente. Solo apile la placa V5 en él.

Este escudo de sensor puede insertarse en módulos de sensor de 3 pines y expone algunos pines de comunicación, como comunicación serial, IIC y SPI también.

**Descripción de Pines**

![](./media/image-20250902165027854.png)

**Diagrama de Conexión**

![](./media/image-20250902165110913.png)

Como se ve en el diagrama anterior, el LED está conectado con D2.

**Código de Prueba**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 1.1
 Parpadeo
 http://www.keyestudio.com
*/
void setup()
{ 
    pinMode(2, OUTPUT);// inicializar el pin digital 2 como salida.
}

void loop() // la función loop se ejecuta una y otra vez indefinidamente
{
   digitalWrite(2, HIGH); // encender el LED (HIGH es el nivel de voltaje)
   delay(1000); // esperar un segundo
   digitalWrite(2, LOW); // apagar el LED haciendo el voltaje LOW
   delay(1000); // esperar un segundo
}
```

**Resultado de la Prueba**

(Habrá contradicción sobre comunicación serial entre el código y Bluetooth al cargar el código. Por lo tanto, no conecte con el módulo Bluetooth antes de cargar el código.)

Cargue el programa en la placa de desarrollo, el LED parpadea a intervalos de 1s.

![](./media/image-20250902165335641.png)

**Explicación del Código**

**pinMode(2，OUTPUT) -** Establecer el pin2 como OUTPUT

**digitalWrite(2，HIGH) -** Cuando se establece el pin2 en nivel HIGH (salida 5V) o en nivel LOW (salida 0V)

**Práctica de Extensión**

Hemos logrado hacer parpadear el LED. A continuación, observemos qué cambiará en el LED si modificamos los pines y el tiempo de retardo.

**Diagrama de Conexión**

![](./media/image-20250902165631206.png)

Hemos alterado los pines y conectado el LED a D10.

**Código de Prueba**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 1.2
 retardo
 http://www.keyestudio.com
*/
void setup() // inicializar el pin digital 10 como salida.
{  
   pinMode(10, OUTPUT);
}

// la función loop se ejecuta una y otra vez indefinidamente
void loop() 
{
   digitalWrite(10, HIGH); // encender el LED (HIGH es el nivel de voltaje)
   delay(100); // esperar 0.1 segundo
   digitalWrite(10, LOW); // apagar el LED haciendo el voltaje LOW
   delay(100); // esperar 0.1 segundo
}
```

El resultado de la prueba muestra que el LED parpadea más rápido. Por lo tanto, podemos llegar a la conclusión de que los pines y el tiempo de retardo afectan la frecuencia de parpadeo.
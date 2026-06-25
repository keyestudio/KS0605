# Proyecto 8 Control de Motor y Velocidad

**Descripción**

![](media/image-20250908162844748.png)

Hay muchas formas de controlar un motor. Nuestro robot coche utiliza la solución más común: L298P, que es un excelente controlador de motor IC de alta potencia producido por STMicroelectronics. Puede controlar directamente motores DC, motores paso a paso de dos fases y cuatro fases. La corriente de conducción es de hasta 2A, y el terminal de salida del motor utiliza ocho diodos Schottky de alta velocidad como protección.

Diseñamos una placa base en función del circuito L298p. El diseño apilado reduce la dificultad técnica del uso y control del motor.

**Especificación**

Diagrama de Circuito de la Placa L298P

![](media/image-20250908163017604.png)

1. Voltaje de entrada de la parte lógica: DC5V
2. Voltaje de entrada de la parte de conducción: DC 7-12V
3. Corriente de trabajo de la parte lógica: \<36mA
4. Corriente de trabajo de la parte de conducción: \<2A
5. Disipación de potencia máxima: 25W (T=75℃)
6. Temperatura de trabajo: -25℃～＋130℃
7. Nivel de entrada de señal de control: nivel alto 2.3V\<Vin\<5V, nivel bajo\0.3V\<Vin\<1.5V

![](media/image-20250908163151925.png)

**Conducir el Robot para Moverse**

A través del diagrama de circuito anterior, el pin de dirección del motor A es D12, y el pin de velocidad es D3; D13 es el pin de dirección del motor B, D11 es el pin de velocidad.

Sabemos cómo controlar los puertos digitales según el siguiente gráfico.

PWM decide que 2 motores se enciendan para conducir el robot coche. El valor PWM está en el rango de 0-255. Cuanto mayor sea el número, más rápido gira el motor.

| Robot Tanque    | Motor (A)           | Motor (B)           |
| --------------- | ------------------- | ------------------- |
| Adelante        | Girar en sentido horario | Girar en sentido horario |
| Atrás           | Girar en sentido antihorario | Girar en sentido antihorario |
| Girar a la izquierda | Girar en sentido antihorario | Girar en sentido horario |
| Girar a la derecha | Girar en sentido horario | Girar en sentido antihorario |
| Parar           | Parar               | Parar               |

**Componentes**

![](media/image-20250908163739200.png)

**Diagrama de Conexión**

![](media/d35ffe6c0c275548f40bcafb42a93da1.jpeg)

**Nota:** el bloque terminal de 4 pines está marcado con serigrafía 1234. La línea roja del motor trasero derecho está conectada al terminal 1, la línea negra está vinculada al extremo 2. La línea roja del motor delantero izquierdo está conectada al terminal 3, la línea negra está vinculada al puerto 4.

**Código de Prueba**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 8.1
 controlador de motor
 http://www.keyestudio.com
*/ 

#define ML_Ctrl 13  //define el pin de control de dirección del motor izquierdo
#define ML_PWM 11   //define el pin de control PWM del motor izquierdo
#define MR_Ctrl 12  //define el pin de control de dirección del motor derecho
#define MR_PWM 3   //define el pin de control PWM del motor derecho

void setup()
{
  pinMode(ML_Ctrl, OUTPUT);//define el pin de control de dirección del motor izquierdo como salida
  pinMode(ML_PWM, OUTPUT);//define el pin de control PWM del motor izquierdo como salida
  pinMode(MR_Ctrl, OUTPUT);//define el pin de control de dirección del motor derecho como salida
  pinMode(MR_PWM, OUTPUT);//define el pin de control PWM del motor derecho como salida
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//establece el pin de control de dirección del motor izquierdo en BAJO
  analogWrite(ML_PWM,200);//establece la velocidad de control PWM del motor izquierdo en 200
  digitalWrite(MR_Ctrl,LOW);//establece el pin de control de dirección del motor derecho en BAJO
  analogWrite(MR_PWM,200);//establece la velocidad de control PWM del motor derecho en 200

  //adelante
  delay(2000);//retraso de 2s
   digitalWrite(ML_Ctrl,HIGH);//establece el pin de control de dirección del motor izquierdo en ALTO
  analogWrite(ML_PWM,200);//establece la velocidad de control PWM del motor izquierdo en 200  
digitalWrite(MR_Ctrl,HIGH);//establece el pin de control de dirección del motor derecho en ALTO
  analogWrite(MR_PWM,200);//establece la velocidad de control PWM del motor derecho en 200

   //atrás
  delay(2000);//retraso de 2s 
  digitalWrite(ML_Ctrl,HIGH);//establece el pin de control de dirección del motor izquierdo en ALTO
  analogWrite(ML_PWM,200);//establece la velocidad de control PWM del motor izquierdo en 200
  digitalWrite(MR_Ctrl,LOW);//establece el pin de control de dirección del motor derecho en BAJO
  analogWrite(MR_PWM,200);//establece la velocidad de control PWM del motor derecho en 200

    //izquierda
  delay(2000);//retraso de 2s
   digitalWrite(ML_Ctrl,LOW);//establece el pin de control de dirección del motor izquierdo en BAJO
  analogWrite(ML_PWM,200);//establece la velocidad de control PWM del motor izquierdo en 200
  digitalWrite(MR_Ctrl,HIGH);//establece el pin de control de dirección del motor derecho en ALTO
  analogWrite(MR_PWM,200);//establece la velocidad de control PWM del motor derecho en 200

   //derecha
  delay(2000);//retraso de 2s
  analogWrite(ML_PWM,0);//establece la velocidad de control PWM del motor izquierdo en 0
  analogWrite(MR_PWM,0);//establece la velocidad de control PWM del motor derecho en 0

    //parar
  delay(2000);//retraso de 2s
}//*****************************************
```

**Resultado de la Prueba**

Conecte según el diagrama de conexión, cargue el código y encienda, el coche inteligente avanza y retrocede durante 2s, gira a la izquierda y derecha durante 2s, se detiene durante 2s y alterna.

**Explicación del Código**

**digitalWrite(ML_Ctrl,LOW):** la dirección de rotación del motor se decide por el nivel alto/bajo y los pines que deciden la dirección de rotación son pines digitales.

**analogWrite(ML_PWM,200):** la velocidad del motor se regula por PWM, y los pines que deciden la velocidad del motor deben ser pines PWM.

**Práctica de Extensión**

Ajuste la velocidad que PWM controla del motor, y conecte de la misma manera.

![](media/d35ffe6c0c275548f40bcafb42a93da1-1757321401643-2.jpeg)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 8.2
 pwm del controlador de motor
 http://www.keyestudio.com
*/ 
#define ML_Ctrl 13  //define el pin de control de dirección del motor izquierdo
#define ML_PWM 11   //define el pin de control PWM del motor izquierdo
#define MR_Ctrl 12  //define el pin de control de dirección del motor derecho
#define MR_PWM 3   //define el pin de control PWM del motor derecho

void setup()
{ 
  pinMode(ML_Ctrl, OUTPUT);//define el pin de control de dirección del motor izquierdo como SALIDA
  pinMode(ML_PWM, OUTPUT);//define el pin de control PWM del motor izquierdo como SALIDA
  pinMode(MR_Ctrl, OUTPUT);//define el pin de control de dirección del motor derecho como SALIDA
  pinMode(MR_PWM, OUTPUT);//define el pin de control PWM del motor derecho como SALIDA
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//Establece el pin de control de dirección del motor izquierdo en BAJO
  analogWrite(ML_PWM,100);//Establece la velocidad de control PWM del motor izquierdo en 100
  digitalWrite(MR_Ctrl,LOW);//Establece el pin de control de dirección del motor derecho en BAJO
  analogWrite(MR_PWM,100);//Establece la velocidad de control PWM del motor derecho en 100
  //adelante
  delay(2000);//define 2s
  digitalWrite(ML_Ctrl,HIGH);//Establece el pin de control de dirección del motor izquierdo en nivel ALTO
  analogWrite(ML_PWM,250);//Establece la velocidad de control PWM del motor izquierdo en 100
  digitalWrite(MR_Ctrl,HIGH);//Establece el pin de control de dirección del motor derecho en nivel ALTO
  analogWrite(MR_PWM,250);//Establece la velocidad de control PWM del motor derecho en 100
   //atrás
  delay(2000);//define 2s
  digitalWrite(ML_Ctrl,HIGH);//Establece el pin de control de dirección del motor izquierdo en nivel ALTO
  analogWrite(ML_PWM,250);//Establece la velocidad de control PWM del motor izquierdo en 100
  digitalWrite(MR_Ctrl,LOW);//Establece el pin de control de dirección del motor derecho en nivel BAJO
  analogWrite(MR_PWM,250);//Establece la velocidad de control PWM del motor derecho en 100
    //izquierda
  delay(2000);//define 2s
   digitalWrite(ML_Ctrl,LOW);//establece el pin de control de dirección del motor izquierdo en BAJO
  analogWrite(ML_PWM,250);//establece la velocidad de control PWM del motor izquierdo en 200
  digitalWrite(MR_Ctrl,HIGH);//establece el pin de control de dirección del motor derecho en ALTO
  analogWrite(MR_PWM,250);//establece la velocidad de control PWM del motor derecho en 100
   //derecha
  delay(2000);//define 2s
  analogWrite(ML_PWM,0);//establece la velocidad de control PWM del motor izquierdo en 0
  analogWrite(MR_PWM,0);//establece la velocidad de control PWM del motor derecho en 0

    //parar
  delay(2000);//define 2s
}//******************************************************************
```

Cargue el código exitosamente, los motores giran más rápido.
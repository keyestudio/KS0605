# Proyecto 10 Robot Seguidor de Luz

![](media/image-20250908171131879.png)

**Descripción**

Hemos introducido cómo utilizar varios sensores y módulos.

En esta lección, combinamos conocimientos de hardware -- módulo fotoresistor, conducción de motores, para construir un robot seguidor de luz.

Solo necesitamos usar 2 módulos fotoresistores para detectar la intensidad de luz en ambos lados del robot. Leer el valor analógico para rotar los 2 motores, así conducir el robot tanque.

**La lógica específica del robot seguidor de luz se muestra en la tabla siguiente:**

![](media/image-20250908171219561.png)

Hacemos un diagrama de flujo basado en la tabla de lógica anterior, como se muestra a continuación:

![](media/image-20250908171232654.png)

**Diagrama de Conexión**

![](media/image-20250908171305946.png)

**Atención:**

El bloque terminal de 4 pines está marcado con serigrafía 1234. La línea roja del motor trasero derecho está conectada al terminal 1, la línea negra está vinculada al extremo 2. La línea roja del motor delantero izquierdo está conectada al terminal 3, la línea negra está vinculada al puerto 4.

| Fotoresistor izquierdo   |      | Sensor Shield     |
| ------------------------ | ---- | ----------------- |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A1                |
|                          |      |                   |
| **Fotoresistor derecho** |      | **Sensor Shield** |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A2                |

**Código de Prueba**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 10
 Tanque seguidor de luz
 http://www.keyestudio.com
*/ 
#define light_L_Pin A1   //define el pin del fotoresistor izquierdo
#define light_R_Pin A2   //define el pin del fotoresistor derecho
#define ML_Ctrl 13  //define el pin de control de dirección del motor izquierdo
#define ML_PWM 11   //define el pin de control PWM del motor izquierdo
#define MR_Ctrl 12  //define el pin de control de dirección del motor derecho
#define MR_PWM 3   //define el pin de control PWM del motor derecho
int left_light; 
int right_light;
void setup(){
  Serial.begin(9600);
  pinMode(light_L_Pin, INPUT);
  pinMode(light_R_Pin, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  left_light = analogRead(light_L_Pin);
  right_light = analogRead(light_R_Pin);
  Serial.print("left_light_value = ");
  Serial.println(left_light);
  Serial.print("right_light_value = ");
  Serial.println(right_light);
  if (left_light > 650 && right_light > 650) //el valor detectado por el fotoresistor, avanzar
  {  
    Car_front();
  } 
  else if (left_light > 650 && right_light <= 650)  //el valor detectado por el fotoresistor, girar a la izquierda
  {
    Car_left();
  } 
  else if (left_light <= 650 && right_light > 650) //el valor detectado por el fotoresistor, girar a la derecha
  {
    Car_right();
  } 
  else  //otras situaciones, detener
  {
    Car_Stop();
  }
}
void Car_front()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,200);
}
void Car_right()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_Stop()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,0);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,0);
}
//****************************************************************
```

**Resultado de la Prueba**

Cargue el código en la placa de desarrollo keyestudio V4.0, el interruptor DIP se coloca en el extremo derecho y encienda la alimentación, el robot inteligente sigue la luz para moverse.
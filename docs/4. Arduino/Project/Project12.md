# Proyecto 12 Tanque Seguidor Ultrasónico

![](media/image-20250908172315808.png)

**Descripción**

En el proyecto 11, hicimos un coche que evita obstáculos. De hecho, solo necesitamos alterar un código de prueba para transformar un coche que evita obstáculos en un coche seguidor. En esta lección, haremos un robot seguidor ultrasónico. El sensor ultrasónico detecta la distancia entre el coche inteligente y el obstáculo para conducir el tanque.

**La lógica específica del robot seguidor ultrasónico se muestra a continuación:**

| **Detección** | **Distancia medida de obstáculos frontales** | **Distancia (unidad: cm)** |
| ------------- | ---------------------------------------- | ----------------------- |
| Configuración      | Ángulo del servo 90°                          |                         |
|               | Panel LED 8X16 muestra el icono "V"        |                         |
| Si            | 20≤ distancia ≤60                         |                         |
| Estado        | Avanzar（establecer PWM a 200）               |                         |
| Si            | 10\<distancia＜20                         |                         |
|               | distancia＞60                             |                         |
| Estado        | Detener                                     |                         |
| Si            | distancia ≤10                             |
| Estado        | Retroceder（establecer PWM a 200）                   |                         |

**Diagrama de flujo**

![](media/image-20250908172442991.png)

**Diagrama de Conexión**

![](media/image-20250908172457017.png)

Nota de conexión:

| Panel LED 8x16 |      | Escudo Sensor V5 |
| ---------------- | ---- | ---------------- |
| GND              | →    | -（GND）         |
| VCC              | →    | +（VCC）         |
| SDA              | →    | SDA              |
| SCL              | →    | SCL              |

**Código de Prueba**

```c
 /*
 keyestudio Mini Tank Robot V2.1
 lección 12
 tanque seguidor ultrasónico
 http://www.keyestudio.com
*/ 
//Matriz, utilizada para almacenar los datos del patrón, puede calcularse por sí mismo u obtenerse de la herramienta de módulo
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //Establecer pin de reloj a A5
#define SDA_Pin  A4  //Establecer pin de datos a A4

#define ML_Ctrl 13  //definir el pin de control de dirección del motor izquierdo
#define ML_PWM 11   //definir pin de control PWM del motor izquierdo
#define MR_Ctrl 12  //definir el pin de control de dirección del motor derecho
#define MR_PWM 3   //definir pin de control PWM del motor derecho
#define Trig 5  //pin Trig ultrasónico
#define Echo 4  //pin Echo ultrasónico
int distance;
int pulsewidth;
#define servoPin 9  //pin del servo
void setup(){
  Serial.begin(9600);
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); //Limpiar la pantalla
  matrix_display(start01);  //mostrar patrón de inicio
  pinMode(servoPin, OUTPUT);
  procedure(90); //establecer servo a 90°
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  distance = checkdistance();  //asignar la distancia detectada por el sensor ultrasónico a distance
  if (distance >= 20 && distance <= 60) //rango para avanzar
  {
    Car_front();
  }
  else if (distance > 10 && distance < 20)  //rango para detener
  {
    Car_Stop();
  }
  else if (distance <= 10)  //rango para retroceder
  {
    Car_back();
  }
  else  //otras situaciones, detener
  {
    Car_Stop();
  }
}
/***********función para el funcionamiento del motor****************/
void Car_front()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,200);
}
void Car_back()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,200);
  digitalWrite(ML_Ctrl,HIGH);
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

/******************matriz de puntos********************/
// función para mostrar la matriz de puntos
void matrix_display(unsigned char matrix_value[])
{
  IIC_start(); // llamar a la función que inicia la transmisión de datos
  IIC_send(0xc0);  //Elegir dirección
  
  for(int i = 0;i < 16;i++) //los datos del patrón tienen 16 bits
  {
     IIC_send(matrix_value[i]); //datos para transmitir patrones
  }

  IIC_end();   //finalizar la transmisión del patrón de datos
  
  IIC_start();
  IIC_send(0x8A);  //seleccionar ancho de pulso 4/16, controlar la pantalla
  IIC_end();
}

//La condición para comenzar a transmitir datos
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

// transmitir datos
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //Cada byte tiene 8 bits
  {
      digitalWrite(SCL_Pin,LOW);  //bajar el pin de reloj SCL para cambiar las señales de SDA      
delayMicroseconds(3);
      if(send_data & 0x01)  //establecer el nivel alto y bajo de SDA_Pin según el 1 o 0 de cada bit
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //subir el pin de reloj SCL_Pin para detener la transmisión de datos
      delayMicroseconds(3);
      send_data = send_data >> 1;  // detectar bit por bit, así que desplazar los datos a la derecha por uno
  }
}
//La señal de que la transmisión de datos ha finalizado
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
/***************fin de la pantalla de matriz de puntos******************/
//La función para controlar el servo
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }}
//La función para controlar la función del sensor ultrasónico controlando ultrasónico
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.20;  //58.20, es decir, 2*29.1=58.2
  delay(10);
  return distance;
}
//****************************************************************
```

 **Resultado de la Prueba**

Código cargado exitosamente, el interruptor DIP se gira hacia el extremo derecho, el servo gira a 90°, "V" se muestra en el panel LED 8X16 y el coche inteligente se mueve según se mueve el obstáculo.
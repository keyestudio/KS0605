# Proyecto 11 Tanque Evasor Ultrasónico

![](media/image-20250908171729897.png)

**Descripción**

En este programa, el sensor ultrasónico detecta la distancia del obstáculo para enviar señales que controlan el robot coche. A continuación, te mostraremos cómo hacer un coche que evita obstáculos.

**La lógica específica del robot evasor ultrasónico se muestra a continuación:**

![](media/image-20250908171756879.png)

 **Diagrama de flujo**

![](media/image-20250908171812532.png)

**Diagrama de conexión:**

![](media/image-20250908171829321.png)

Nota: Los pines "-", "+" y "S" del servo están conectados respectivamente a G（GND）, V（VCC）y D9 de la placa de expansión. El VCC, Trig, Echo y Gnd del sensor ultrasónico están conectados con 5v(V), 5(S), Echo y Gnd(G) de la placa de expansión.

**Código de prueba:**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lección 11
 ultrasonic_avoid_tank
 http://www.keyestudio.com
*/
int random2;
int a;
int a1;
int a2;
#define ML_Ctrl 13  //define el pin de control de dirección del motor izquierdo
#define ML_PWM 11   //define el pin de control PWM del motor izquierdo
#define MR_Ctrl 12  //define el pin de control de dirección del motor derecho
#define MR_PWM 3   //define el pin de control PWM del motor derecho

#define Trig 5  //pin Trig ultrasónico
#define Echo 4  //pin Echo ultrasónico
int distance;
#define servoPin 9  //pin del servo
int pulsewidth;
/************la función para ejecutar el motor**************/
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
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,HIGH);
  analogWrite(ML_PWM,255);
}
void Car_right()
{
  digitalWrite(MR_Ctrl,HIGH);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,255);
}
void Car_Stop()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,0);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,0);
}

//La función para controlar el servo
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}
//La función para controlar el sensor ultrasónico
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.00;  //58.20, es decir, 2*29.1=58.2
  delay(10);
  return distance;
}
  //****************************************************************
void setup(){
  pinMode(servoPin, OUTPUT);
  procedure(90); //establece el servo a 90°
  
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  random2 = random(1, 100);
  a = checkdistance();  //asigna la distancia frontal detectada por el sensor ultrasónico a la variable a
  
  if (a < 20) //cuando la distancia frontal detectada es menor que 20 
  {
      Car_Stop();  //el robot se detiene
      delay(500); //retraso de 500ms
      procedure(160);  //la plataforma ultrasónica gira a la izquierda
      for (int j = 1; j <= 10; j = j + (1)) { //sentencia for, los datos serán más precisos si el sensor ultrasónico detecta varias veces.
        a1 = checkdistance();  //asigna la distancia izquierda detectada por el sensor ultrasónico a la variable a1
      }
      delay(300);
      procedure(20); //la plataforma ultrasónica gira a la derecha
      for (int k = 1; k <= 10; k = k + (1)) {
        a2 = checkdistance(); //asigna la distancia derecha detectada por el sensor ultrasónico a la variable a2
      }
      
      if (a1 < 50 || a2 < 50)  //el robot girará hacia el lado de mayor distancia cuando la distancia izquierda o derecha es menor que 50cm. 
      {
        if (a1 > a2) //la distancia izquierda es mayor que el lado derecho      
        {
          procedure(90);  //la plataforma ultrasónica gira hacia adelante a la derecha         
          Car_left();  //el robot gira a la izquierda
          delay(500);  //gira a la izquierda durante 500ms
          Car_front(); //avanza
        } 
        else 
        {
          procedure(90);
          Car_right(); //el robot gira a la derecha
          delay(500);
          Car_front();  //avanza
        }
      } 
      else  //Si ambos lados son mayores o iguales a 50cm, gira a la izquierda o derecha aleatoriamente
      {
        if ((long) (random2) % (long) (2) == 0)  //Cuando el número aleatorio es par
        {
          procedure(90);
          Car_left(); //el robot tanque gira a la izquierda
          delay(500);
          Car_front(); //avanza
        } 
        else 
        {
          procedure(90);
          Car_right(); //el robot gira a la derecha
          delay(500);
          Car_front(); //avanza
       }
     }
  } 
  else  //Si la distancia frontal es mayor o igual a 20cm, el robot coche avanzará
  {
      Car_front(); //avanza
  }
}
```

 **Resultado de la prueba**

Carga el código exitosamente, coloca el interruptor DIP hacia el extremo derecho y enciende, el robot tanque avanza y evita automáticamente el obstáculo.
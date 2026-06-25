# Projekt 11 Ultraschall-Ausweich-Panzer

![](media/image-20250908171729897.png)

**Beschreibung**

In diesem Programm erkennt der Ultraschallsensor die Entfernung von Hindernissen und sendet Signale, die den Roboterwagen steuern. Im Folgenden zeigen wir dir, wie du ein Hindernisvermeidungsfahrzeug baust.

**Die spezifische Logik des Ultraschall-Ausweichroboters ist wie folgt dargestellt:**

![](media/image-20250908171756879.png)

 **Ablaufdiagramm**

![](media/image-20250908171812532.png)

**Verbindungsdiagramm:**

![](media/image-20250908171829321.png)

Hinweis: Die Stifte „-", „+" und „S" des Servos sind jeweils mit G (GND), V (VCC) und D9 der Erweiterungsplatine verbunden. VCC, Trig, Echo und Gnd des Ultraschallsensors sind mit 5V (V), 5 (S), Echo und Gnd (G) der Erweiterungsplatine verbunden.

**Testcode:**

```c
/*
 keyestudio Mini Tank Robot V2.1
 Lektion 11
 ultrasonic_avoid_tank
 http://www.keyestudio.com
*/
int random2;
int a;
int a1;
int a2;
#define ML_Ctrl 13  // Richtungssteuerpin des linken Motors definieren
#define ML_PWM 11   // PWM-Steuerpin des linken Motors definieren
#define MR_Ctrl 12  // Richtungssteuerpin des rechten Motors definieren
#define MR_PWM 3   // PWM-Steuerpin des rechten Motors definieren

#define Trig 5  // Ultraschall-Trig-Pin
#define Echo 4  // Ultraschall-Echo-Pin
int distance;
#define servoPin 9  // Servo-Pin
int pulsewidth;
/************Funktion zum Betreiben des Motors**************/
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

// Funktion zur Steuerung des Servos
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}
// Funktion zur Steuerung des Ultraschallsensors
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.00;  // 58.20, das heißt, 2*29.1=58.2
  delay(10);
  return distance;
}
  //****************************************************************
void setup(){
  pinMode(servoPin, OUTPUT);
  procedure(90); // Servo auf 90° einstellen
  
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  random2 = random(1, 100);
  a = checkdistance();  // Die vom Ultraschallsensor erkannte Vorderentfernung der Variablen a zuweisen
  
  if (a < 20) // Wenn die erkannte Vorderentfernung kleiner als 20 ist
  {
      Car_Stop();  // Roboter stoppt
      delay(500); // Verzögerung von 500 ms
      procedure(160);  // Ultraschallplattform dreht nach links
      for (int j = 1; j <= 10; j = j + (1)) { // for-Anweisung, die Daten sind genauer, wenn der Ultraschallsensor mehrmals erkennt.
        a1 = checkdistance();  // Die vom Ultraschallsensor erkannte linke Entfernung der Variablen a1 zuweisen
      }
      delay(300);
      procedure(20); // Ultraschallplattform dreht nach rechts
      for (int k = 1; k <= 10; k = k + (1)) {
        a2 = checkdistance(); // Die vom Ultraschallsensor erkannte rechte Entfernung der Variablen a2 zuweisen
      }
      
      if (a1 < 50 || a2 < 50)  // Der Roboter dreht zur Seite mit der größeren Entfernung, wenn die linke oder rechte Entfernung kleiner als 50 cm ist.
      {
        if (a1 > a2) // Linke Entfernung ist größer als rechte Seite
        {
          procedure(90);  // Ultraschallplattform dreht zurück nach vorne rechts
Car_left();  // Roboter dreht nach links
          delay(500);  // 500 ms nach links drehen
          Car_front(); // Nach vorne fahren
        } 
        else 
        {
          procedure(90);
          Car_right(); // Roboter dreht nach rechts
          delay(500);
          Car_front();  // Nach vorne fahren
        }
      } 
      else  // Wenn beide Seiten größer oder gleich 50 cm sind, zufällig nach links oder rechts drehen
      {
        if ((long) (random2) % (long) (2) == 0)  // Wenn die Zufallszahl gerade ist
        {
          procedure(90);
          Car_left(); // Panzeroboter dreht nach links
          delay(500);
          Car_front(); // Nach vorne fahren
        } 
        else 
        {
          procedure(90);
          Car_right(); // Roboter dreht nach rechts
          delay(500);
          Car_front(); // Nach vorne fahren
       }
     }
  } 
  else  // Wenn die Vorderentfernung größer oder gleich 20 cm ist, fährt der Roboterwagen nach vorne
  {
      Car_front(); // Nach vorne fahren
  }
}
```

 **Testergebnis**

Code erfolgreich hochgeladen, DIP-Schalter auf das rechte Ende gestellt und Stromversorgung eingeschaltet. Der Panzeroboter fährt nach vorne und weicht Hindernissen automatisch aus.
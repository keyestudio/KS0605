# プロジェクト11 超音波回避タンク

![](media/image-20250908171729897.png)

**説明**

このプログラムでは、超音波センサーが障害物までの距離を検出し、ロボットカーを制御するシグナルを送信します。次に、障害物回避カーの作り方を説明します。

**超音波回避ロボットの具体的なロジックは以下の通りです：**

![](media/image-20250908171756879.png)

 **フローチャート**

![](media/image-20250908171812532.png)

**接続図：**

![](media/image-20250908171829321.png)

注：サーボの「-」、「+」、「S」ピンはそれぞれ拡張ボードのG（GND）、V（VCC）、D9に接続されています。超音波センサーのVCC、Trig、Echo、Gndは拡張ボードの5v(V)、5(S)、Echo、Gnd(G)に接続されています。

**テストコード：**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 11
 ultrasonic_avoid_tank
 http://www.keyestudio.com
*/
int random2;
int a;
int a1;
int a2;
#define ML_Ctrl 13  // 左モーターの方向制御ピンを定義
#define ML_PWM 11   // 左モーターのPWM制御ピンを定義
#define MR_Ctrl 12  // 右モーターの方向制御ピンを定義
#define MR_PWM 3   // 右モーターのPWM制御ピンを定義

#define Trig 5  // 超音波トリガーピン
#define Echo 4  // 超音波エコーピン
int distance;
#define servoPin 9  // サーボピン
int pulsewidth;
/************モーターを実行する関数**************/
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

// サーボを制御する関数
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}
// 超音波センサーを制御する関数
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.00;  // 58.20、つまり2*29.1=58.2
  delay(10);
  return distance;
}
  //****************************************************************
void setup(){
  pinMode(servoPin, OUTPUT);
  procedure(90); // サーボを90°に設定
  
  pinMode(Trig, OUTPUT);
  pinMode(Echo, INPUT);
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}
void loop(){
  random2 = random(1, 100);
  a = checkdistance();  // 超音波センサーが検出した前方の距離を変数aに割り当て
  
  if (a < 20) // 検出された前方の距離が20未満の場合
  {
      Car_Stop();  // ロボットが停止
      delay(500); // 500ms遅延
      procedure(160);  // 超音波プラットフォームが左に回転
      for (int j = 1; j <= 10; j = j + (1)) { // for文、超音波センサーが複数回検出するとデータがより正確になります
        a1 = checkdistance();  // 超音波センサーが検出した左方の距離を変数a1に割り当て
      }
      delay(300);
      procedure(20); // 超音波プラットフォームが右に回転
      for (int k = 1; k <= 10; k = k + (1)) {
        a2 = checkdistance(); // 超音波センサーが検出した右方の距離を変数a2に割り当て
      }
      
      if (a1 < 50 || a2 < 50)  // 左または右の距離が50cm未満の場合、ロボットはより長い距離の側に回転します
      {
        if (a1 > a2) // 左方の距離が右側より大きい場合
        {
          procedure(90);  // 超音波プラットフォームが右前に戻る
Car_left();  // ロボットが左に回転
          delay(500);  // 500ms左に回転
          Car_front(); // 前に進む
        } 
        else 
        {
          procedure(90);
          Car_right(); // ロボットが右に回転
          delay(500);
          Car_front();  // 前に進む
        }
      } 
      else  // 両側が50cm以上の場合、ランダムに左または右に回転
      {
        if ((long) (random2) % (long) (2) == 0)  // ランダム数が偶数の場合
        {
          procedure(90);
          Car_left(); // タンクロボットが左に回転
          delay(500);
          Car_front(); // 前に進む
        } 
        else 
        {
          procedure(90);
          Car_right(); // ロボットが右に回転
          delay(500);
          Car_front(); // 前に進む
       }
     }
  } 
  else  // 前方の距離が20cm以上の場合、ロボットカーが前に進む
  {
      Car_front(); // 前に進む
  }
}
```

 **テスト結果**

コードのアップロードが成功し、DIPスイッチを右端に切り替えて電源を入れると、タンクロボットが前に進み、自動的に障害物を回避します。
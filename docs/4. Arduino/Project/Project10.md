# プロジェクト10 光追従ロボット

![](media/image-20250908171131879.png)

**説明**

これまで、様々なセンサーやモジュールの使用方法を紹介してきました。

このレッスンでは、ハードウェアの知識（フォトレジスタモジュール、モータ駆動）と組み合わせて、光追従ロボットを構築します！

ロボットの両側に2つのフォトレジスタモジュールを使用して光の強度を検出するだけです。アナログ値を読み取って2つのモータを回転させることで、タンクロボットを走行させます。

**光追従ロボットの具体的なロジックを以下の表に示します：**

![](media/image-20250908171219561.png)

上記のロジックテーブルに基づいてフローチャートを作成しました。以下に示します：

![](media/image-20250908171232654.png)

**接続図**

![](media/image-20250908171305946.png)

**注意：**

4ピンターミナルブロックはシルクスクリーンで1234とマークされています。右後部モータの赤線はターミナル1に接続され、黒線は端子2に接続されます。左前部モータの赤線は端子3に接続され、黒線はポート4に接続されます。

| 左フォトレジスタ      |      | センサシールド    |
| ------------------------ | ---- | ----------------- |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A1                |
|                          |      |                   |
| **右フォトレジスタ** |      | **センサシールド** |
| -                        | →    | G（GND）          |
| +                        | →    | V（VCC）          |
| S                        | →    | A2                |

**テストコード**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 10
 Light-following tank
 http://www.keyestudio.com
*/ 
#define light_L_Pin A1   // 左フォトレジスタのピンを定義
#define light_R_Pin A2   // 右フォトレジスタのピンを定義
#define ML_Ctrl 13  // 左モータの方向制御ピンを定義
#define ML_PWM 11   // 左モータのPWM制御ピンを定義
#define MR_Ctrl 12  // 右モータの方向制御ピンを定義
#define MR_PWM 3   // 右モータのPWM制御ピンを定義
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
  if (left_light > 650 && right_light > 650) // フォトレジスタが検出した値、前進
  {  
    Car_front();
  } 
  else if (left_light > 650 && right_light <= 650)  // フォトレジスタが検出した値、左旋回
  {
    Car_left();
  } 
  else if (left_light <= 650 && right_light > 650) // フォトレジスタが検出した値、右旋回
  {
    Car_right();
  } 
  else  // その他の状況、停止
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

**テスト結果**

keyestudio V4.0開発ボードにコードをアップロードし、DIPスイッチを右端に設定して電源を入れると、スマートロボットが光に追従して移動します。
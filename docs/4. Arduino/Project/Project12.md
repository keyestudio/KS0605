# プロジェクト12 超音波追従タンク

![](media/image-20250908172315808.png)

**説明**

プロジェクト11では、障害物回避カーを作成しました。実は、テストコードを少し変更するだけで、障害物回避カーを追従カーに変換することができます。このレッスンでは、超音波追従ロボットを作成します。超音波センサーはスマートカーと障害物の間の距離を検出し、タンクカーを動かします。

**超音波追従ロボットの具体的なロジックは以下の通りです：**

| **検出** | **前方障害物の測定距離** | **距離（単位：cm）** |
| ------------- | ---------------------------------------- | ----------------------- |
| 設定      | サーボ角度90°                          |                         |
|               | 8X16 LEDパネルに「V」アイコンを表示        |                         |
| 条件            | 20≤ 距離 ≤60                         |                         |
| 状態        | 前進（PWMを200に設定）               |                         |
| 条件            | 10\<距離＜20                         |                         |
|               | 距離＞60                             |                         |
| 状態        | 停止                                     |                         |
| 条件            | 距離 ≤10                             |                         |
| 状態        | 後退（PWMを200に設定）                   |                         |

**フローチャート**

![](media/image-20250908172442991.png)

**接続図**

![](media/image-20250908172457017.png)

配線に関する注意：

| 1.8x16 LEDパネル |      | V5 センサーシールド |
| ---------------- | ---- | ---------------- |
| GND              | →    | -（GND）         |
| VCC              | →    | +（VCC）         |
| SDA              | →    | SDA              |
| SCL              | →    | SCL              |

**テストコード**

```c
 /*
 keyestudio Mini Tank Robot V2.1
 lesson 12
 ultrasonic follow tank
 http://www.keyestudio.com
*/ 
// 配列、パターンのデータを保存するために使用されます。自分で計算するか、モジュラスツールから取得できます
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // クロックピンをA5に設定
#define SDA_Pin  A4  // データピンをA4に設定

#define ML_Ctrl 13  // 左モーターの方向制御ピンを定義
#define ML_PWM 11   // 左モーターのPWM制御ピンを定義
#define MR_Ctrl 12  // 右モーターの方向制御ピンを定義
#define MR_PWM 3   // 右モーターのPWM制御ピンを定義
#define Trig 5  // 超音波トリガーピン
#define Echo 4  // 超音波エコーピン
int distance;
int pulsewidth;
#define servoPin 9  // サーボピン
void setup(){
  Serial.begin(9600);
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); // ディスプレイをクリア
  matrix_display(start01);  // スタートパターンを表示
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
  distance = checkdistance();  // 超音波センサーで検出した距離をdistanceに代入
  if (distance >= 20 && distance <= 60) // 前進する範囲
  {
    Car_front();
  }
  else if (distance > 10 && distance < 20)  // 停止する範囲
  {
    Car_Stop();
  }
  else if (distance <= 10)  // 後退する範囲
  {
    Car_back();
  }
  else  // その他の状況では停止
  {
    Car_Stop();
  }
}
/***********モーター動作用の関数****************/
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

/******************ドットマトリックス********************/
// ドットマトリックス表示用の関数
void matrix_display(unsigned char matrix_value[])
{
  IIC_start(); // データ送信開始の関数を呼び出し
  IIC_send(0xc0);  // アドレスを選択
  
  for(int i = 0;i < 16;i++) // パターンデータは16ビット
  {
     IIC_send(matrix_value[i]); // パターンを伝達するデータ
  }

  IIC_end();   // データパターン伝達を終了
  
  IIC_start();
  IIC_send(0x8A);  // パルス幅4/16を選択し、表示を制御
  IIC_end();
}

// データ送信開始の条件
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

// データを送信
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // 各バイトは8ビット
  {
      digitalWrite(SCL_Pin,LOW);  // クロックピンSCL_Pinを下げてSDAの信号を変更      
delayMicroseconds(3);
      if(send_data & 0x01)  // 各ビットの1または0に応じてSDA_Pinの高低レベルを設定
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // クロックピンSCL_Pinを上げてデータ送信を停止
      delayMicroseconds(3);
      send_data = send_data >> 1;  // ビットごとに検出するため、データを1ビット右にシフト
  }
}
// データ送信終了の合図
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
/***************ドットマトリックス表示終了******************/
// サーボを制御する関数
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }}
// 超音波センサー制御関数 超音波を制御する関数
float checkdistance() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(Trig, LOW);
  float distance = pulseIn(Echo, HIGH) / 58.20;  // 58.20、つまり、2*29.1=58.2
  delay(10);
  return distance;
}
// ****************************************************************
```

 **テスト結果**

コードをアップロードしました。DIPスイッチを右端に切り替え、サーボが90°に回転し、8X16 LEDパネルに「V」が表示され、スマートカーが障害物に応じて動きます。
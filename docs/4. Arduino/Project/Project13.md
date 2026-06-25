# プロジェクト 13 IR リモコンロボットタンク

![](media/image-20250908172649810.png)

**説明**

IR リモコン制御は、テレビ、扇風機、および一部の家電製品に適用される最も一般的な制御方法の1つです。このプロジェクトでは、IR リモコンスマートカーを製作します。IR リモコンの各キーの値がわかっているため、対応するキー値を通じてスマートカーを制御し、ドットマトリックスにパターンを表示することができます。

**赤外線リモコンロボットの具体的なロジックは以下の通りです：**

| 初期設定                               | サーボ角度 90°                          |                                     |
| -------------------------------------- | --------------------------------------- | ----------------------------------- |
|                                        | 8X16 LED マトリックスパネルに「V」アイコンを表示 |                                     |
| **リモコン**                           | **キー値**                              | **キー状態**                        |
| ![](media/image-20250908172904905.png) | FF629D                                  | 前進（PWM を 200 に設定）           |
|                                        |                                         | 8X16 LED パネルに前進アイコンを表示 |
| ![](media/image-20250908172927504.png) | FFA857                                  | 後進（PWM を 200 に設定）           |
|                                        |                                         | 8X16 LED パネルに後進アイコンを表示 |
| ![](media/image-20250908172954542.png) | FF22DD                                  | 左旋回                              |
|                                        |                                         | 8X16 LED パネルに左向きアイコンを表示 |
| ![](media/image-20250908173027144.png) | FFC23D                                  | 右旋回                              |
|                                        |                                         | 8X16 LED パネルに右向きアイコンを表示 |
| ![](media/image-20250908173139888.png) | FF02FD                                  | 停止                                |
|                                        |                                         | 8X16 LED パネルに「STOP」を表示     |
| ![](media/image-20250908173312378.png) | FF30CF                                  | 左回転（PWM を 200 に設定）         |
|                                        |                                         | 8X16 LED パネルに左向きアイコンを表示 |
| ![](media/image-20250908173336232.png) | FF7A85                                  | 右回転（PWM を 200 に設定）         |
|                                        |                                         | 8X16 LED パネルに右向きアイコンを表示 |

**フローチャート**

![](media/image-20250908173443316.png)

**接続図**

![](media/image-20250908173458023.png)

注意：8x16 LED パネルの GND、VCC、SDA、SCL はそれぞれ\-（GND）、+（VCC）、SDA、SCL に接続されます。IR レシーバーモジュールの「-」、「+」、S はセンサーシールドの G（GND）、V（VCC）、A0 に接続されます。デジタルポートが不足している場合、アナログポートをデジタルポートとして使用できます。A0 はデジタル 14 に相当し、A1 はデジタル 15 に相当します。

**テストコード**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 13
 IR remote tank
 http://www.keyestudio.com
*/

#include <IRremoteTank.h>
IRrecv irrecv(A0);  // IRrecv irrecv を A0 に設定
decode_results results;
long ir_rec;  // 受信した IR 値を保存

// 配列、パターンのデータを保存するために使用、自分で計算するか、モジュラスツールから取得できます
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // クロックピンを A5 に設定
#define SDA_Pin  A4  // データピンを A4 に設定

#define ML_Ctrl 13  // 左モーターの方向制御ピンを定義
#define ML_PWM 11   // 左モーターの PWM 制御ピンを定義
#define MR_Ctrl 12  // 右モーターの方向制御ピンを定義
#define MR_PWM 3    // 右モーターの PWM 制御ピンを定義

#define servoPin 9 // サーボのピン
int pulsewidth; // サーボのパルス幅値を保存

void setup(){
  Serial.begin(9600);
  irrecv.enableIRIn();  // IR 受信ライブラリを初期化
  
  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear); // 画面をクリア
  matrix_display(start01);  // スタート画像を表示
  
  pinMode(servoPin, OUTPUT);
  procedure(90);  // サーボを 90° に回転
}

void loop(){
  if (irrecv.decode(&results)) // IR リモコン値を受信
  {
    ir_rec=results.value;
    String type="UNKNOWN";
    String typelist[14]={"UNKNOWN", "NEC", "SONY", "RC5", "RC6", "DISH", "SHARP", "PANASONIC", "JVC", "SANYO", "MITSUBISHI", "SAMSUNG", "LG", "WHYNTER"};
    if(results.decode_type>=1&&results.decode_type<=13){
      type=typelist[results.decode_type];
    }
    Serial.print("IR TYPE:"+type+"  ");
    Serial.println(ir_rec,HEX);
    irrecv.resume();
  }
  
  if (ir_rec == 0xFF629D) // 前進
  {
    Car_front();
    matrix_display(front);  // 前進画像を表示
  }
  if (ir_rec == 0xFFA857)  // ロボットカーが後進
  {
    Car_back();
    matrix_display(front);  // 後進
  }
  if (ir_rec == 0xFF22DD)   // ロボットカーが左旋回
  {
    Car_T_left();
    matrix_display(left);  // 左旋回画像を表示
  }
  if (ir_rec == 0xFFC23D)   // ロボットカーが右旋回
  {
    Car_T_right();
    matrix_display(right);  // 右旋回画像を表示
  }
  if (ir_rec == 0xFF02FD)   // ロボットカーが停止
  { 
    Car_Stop();
    matrix_display(STOP01);  // 停止画像を表示
  }
  if (ir_rec == 0xFF30CF)   // ロボットカーが反時計回りに回転
  {
    Car_left();
    matrix_display(left);  // 反時計回り回転画像を表示
  }
  if (ir_rec == 0xFF7A85)  // ロボットカーが時計回りに回転
  {
    Car_right();
    matrix_display(right);  // 時計回り回転画像を表示
 }
}
/******************サーボ制御*******************/
void procedure(int myangle) {
  for (int i = 0; i <= 50; i = i + (1)) {
    pulsewidth = myangle * 11 + 500;
    digitalWrite(servoPin,HIGH);
    delayMicroseconds(pulsewidth);
    digitalWrite(servoPin,LOW);
    delay((20 - pulsewidth / 1000));
  }
}

/******************ドットマトリックス****************/
// この関数はドットマトリックス表示に使用されます
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  // アドレスを選択
   for(int i = 0;i < 16;i++) // 画像は 16 ビット
  {
     IIC_send(matrix_value[i]); // パターンを伝達するデータ
  }
  IIC_end();   // データパターン伝達を終了
  
  IIC_start();
  IIC_send(0x8A);  // 表示制御、パルス幅を 4/16 に設定
  IIC_end();
}

// データ伝達を開始する条件
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  // 各バイトは 8 ビット、各文字は 8 ビット
  {
      digitalWrite(SCL_Pin,LOW);  // クロックピン SCL_Pin を下げて SDA の信号を変更
      delayMicroseconds(3);
      if(send_data & 0x01)  // 各ビットの 1 または 0 に従って SDA_Pin の高低レベルを設定
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // クロックピン SCL_Pin を上げてデータ伝達を停止
      delayMicroseconds(3);
      send_data = send_data >> 1;  // ビットごとに検出するため、データを右に 1 ビットシフト
  }
}
// データ伝達が終了した合図
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
/***************モーターを実行する関数***************/
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
void Car_T_left()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,255);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,180);
}
void Car_T_right()
{
  digitalWrite(MR_Ctrl,LOW);
  analogWrite(MR_PWM,180);
  digitalWrite(ML_Ctrl,LOW);
  analogWrite(ML_PWM,255);
}
 //****************************************************************
```

**テスト結果**

コードを正常にアップロードして電源を入れると、スマートロボットは IR リモコンで制御でき、同時に対応するパターンが 8X16 LED パネルに表示されます。
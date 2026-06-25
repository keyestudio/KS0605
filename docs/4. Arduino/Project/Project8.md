# プロジェクト8 モーター駆動と速度制御

**説明**

![](media/image-20250908162844748.png)

モーターを駆動する方法はたくさんありますが、当社のロボットカーは最も一般的なソリューション--L298P--を使用しています。これはSTMicroelectronicsが製造した優れた高出力モータードライバーICです。DC モーター、2相および4相ステッピングモーターを直接駆動できます。駆動電流は最大2Aで、モーターの出力端子は8個の高速ショットキーダイオードで保護されています。

L298pの回路に基づいてシールドを設計しました。積み重ねられた設計により、モーターの使用と駆動の技術的難易度が軽減されます。

**仕様**

L298Pボード回路図

![](media/image-20250908163017604.png)

1. ロジック部入力電圧：DC5V
2. 駆動部入力電圧：DC 7-12V
3. ロジック部動作電流：\<36mA
4. 駆動部動作電流：\<2A
5. 最大消費電力：25W（T=75℃）
6. 動作温度：-25℃～＋130℃
7. 制御信号入力レベル：高レベル 2.3V\<Vin\<5V、低レベル\0.3V\<Vin\<1.5V

![](media/image-20250908163151925.png)

**ロボットを動かす**

上記の回路図を通じて、Aモーターの方向ピンはD12、速度ピンはD3です。D13はBモーターの方向ピン、D11は速度ピンです。

以下のチャートに従ってデジタルポートを制御する方法を知っています。

PWMは2つのモーターをオンにしてロボットカーを駆動します。PWM値の範囲は0～255です。数値が大きいほど、モーターの回転速度が速くなります。

| タンクロボット | モーター（A） | モーター（B） |
| --------------- | ------------------ | ------------------ |
| 前進         | 時計回りに回転     |                    |
| 後進        | 反時計回りに回転 |                    |
| 左回転  | 反時計回りに回転 | 時計回りに回転     |
| 右回転 | 時計回りに回転     | 反時計回りに回転 |
| 停止            | 停止               | 停止               |

**部品**

![](media/image-20250908163739200.png)

**接続図**

![](media/d35ffe6c0c275548f40bcafb42a93da1.jpeg)

**注意：** 4ピンターミナルブロックはシルクスクリーン1234でマークされています。右後部モーターの赤線はターミナル1に接続され、黒線は端子2に接続されます。左前部モーターの赤線は端子3に接続され、黒線はポート4に接続されます。

**テストコード**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 8.1
 motor driver
 http://www.keyestudio.com
*/ 

#define ML_Ctrl 13  //左モーターの方向制御ピンを定義
#define ML_PWM 11   //左モーターのPWM制御ピンを定義
#define MR_Ctrl 12  //右モーターの方向制御ピンを定義
#define MR_PWM 3   //右モーターのPWM制御ピンを定義

void setup()
{
  pinMode(ML_Ctrl, OUTPUT);//左モーターの方向制御ピンを出力として定義
  pinMode(ML_PWM, OUTPUT);//左モーターのPWM制御ピンを出力として定義
  pinMode(MR_Ctrl, OUTPUT);//右モーターの方向制御ピンを出力として定義
  pinMode(MR_PWM, OUTPUT);//右モーターのPWM制御ピンを出力として定義
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//左モーターの方向制御ピンをLOWに設定
  analogWrite(ML_PWM,200);//左モーターのPWM制御速度を200に設定
  digitalWrite(MR_Ctrl,LOW);//右モーターの方向制御ピンをLOWに設定
  analogWrite(MR_PWM,200);//右モーターのPWM制御速度を200に設定

  //前進
  delay(2000);//2秒遅延
   digitalWrite(ML_Ctrl,HIGH);//左モーターの方向制御ピンをHIGHに設定
  analogWrite(ML_PWM,200);//左モーターのPWM制御速度を200に設定  
digitalWrite(MR_Ctrl,HIGH);//右モーターの方向制御ピンをHIGHに設定
  analogWrite(MR_PWM,200);//右モーターのPWM制御速度を200に設定

   //後進
  delay(2000);//2秒遅延 
  digitalWrite(ML_Ctrl,HIGH);//左モーターの方向制御ピンをHIGHに設定
  analogWrite(ML_PWM,200);//左モーターのPWM制御速度を200に設定
  digitalWrite(MR_Ctrl,LOW);//右モーターの方向制御ピンをLOWに設定
  analogWrite(MR_PWM,200);//右モーターのPWM制御速度を200に設定

    //左回転
  delay(2000);//2秒遅延
   digitalWrite(ML_Ctrl,LOW);//左モーターの方向制御ピンをLOWに設定
  analogWrite(ML_PWM,200);//左モーターのPWM制御速度を200に設定
  digitalWrite(MR_Ctrl,HIGH);//右モーターの方向制御ピンをHIGHに設定
  analogWrite(MR_PWM,200);//右モーターのPWM制御速度を200に設定

   //右回転
  delay(2000);//2秒遅延
  analogWrite(ML_PWM,0);//左モーターのPWM制御速度を0に設定
  analogWrite(MR_PWM,0);//右モーターのPWM制御速度を0に設定

    //停止
  delay(2000);//2秒遅延
}//*****************************************
```

**テスト結果**

接続図に従って接続し、コードをアップロードして電源を入れると、スマートカーが2秒間前進・後進し、2秒間左右に回転し、2秒間停止して交互に繰り返します。

**コード説明**

**digitalWrite(ML_Ctrl,LOW)：** モーターの回転方向は高/低レベルによって決定され、回転方向を決定するピンはデジタルピンです。

**analogWrite(ML_PWM,200)：** モーターの速度はPWMで調整され、モーターの速度を決定するピンはPWMピンである必要があります。

**応用練習**

PWMがモーターを制御する速度を調整し、同じ方法で接続します。

![](media/d35ffe6c0c275548f40bcafb42a93da1-1757321401643-2.jpeg)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 8.2
 motor driver pwm
 http://www.keyestudio.com
*/ 
#define ML_Ctrl 13  //左モーターの方向制御ピンを定義
#define ML_PWM 11   //左モーターのPWM制御ピンを定義
#define MR_Ctrl 12  //右モーターの方向制御ピンを定義
#define MR_PWM 3   //右モーターのPWM制御ピンを定義

void setup()
{ 
  pinMode(ML_Ctrl, OUTPUT);//左モーターの方向制御ピンをOUTPUTとして定義
  pinMode(ML_PWM, OUTPUT);//左モーターのPWM制御ピンをOUTPUTとして定義
  pinMode(MR_Ctrl, OUTPUT);//右モーターの方向制御ピンをOUTPUTとして定義
  pinMode(MR_PWM, OUTPUT);//右モーターのPWM制御ピンをOUTPUTとして定義
}

void loop()
{ 
  digitalWrite(ML_Ctrl,LOW);//左モーターの方向制御ピンをLOWに設定
  analogWrite(ML_PWM,100);//左モーターのPWM制御速度を100に設定
  digitalWrite(MR_Ctrl,LOW);//右モーターの方向制御ピンをLOWに設定
  analogWrite(MR_PWM,100);//右モーターのPWM制御速度を100に設定
  //前進
  delay(2000);//2秒定義
  digitalWrite(ML_Ctrl,HIGH);//左モーターの方向制御ピンをHIGHレベルに設定
  analogWrite(ML_PWM,250);//左モーターのPWM制御速度を100に設定
  digitalWrite(MR_Ctrl,HIGH);//右モーターの方向制御ピンをHIGHレベルに設定
  analogWrite(MR_PWM,250);//右モーターのPWM制御速度を100に設定
   //後進
  delay(2000);//2秒定義
  digitalWrite(ML_Ctrl,HIGH);//左モーターの方向制御ピンをHIGHレベルに設定
  analogWrite(ML_PWM,250);//左モーターのPWM制御速度を100に設定
  digitalWrite(MR_Ctrl,LOW);//右モーターの方向制御ピンをLOWレベルに設定
  analogWrite(MR_PWM,250);//右モーターのPWM制御速度を100に設定
    //左回転
  delay(2000);//2秒定義
   digitalWrite(ML_Ctrl,LOW);//左モーターの方向制御ピンをLOWに設定
  analogWrite(ML_PWM,250);//左モーターのPWM制御速度を200に設定
  digitalWrite(MR_Ctrl,HIGH);//右モーターの方向制御ピンをHIGHに設定
  analogWrite(MR_PWM,250);//右モーターのPWM制御速度を100に設定
   //右回転
  delay(2000);//2秒定義
  analogWrite(ML_PWM,0);//左モーターのPWM制御速度を0に設定
  analogWrite(MR_PWM,0);//右モーターのPWM制御速度を0に設定

    //停止
  delay(2000);//2秒定義
}//******************************************************************
```

コードが正常にアップロードされ、モーターがより速く回転します。
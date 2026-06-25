# プロジェクト5 超音波センサー

**説明**

![](media/image-20250908154003868.png)

HC-SR04超音波センサーは、コウモリのようにソナーを使用して物体までの距離を測定します。優れた非接触範囲検出、高精度、安定した読み取り値を備えた使いやすいパッケージです。超音波送信機と受信機モジュールが完備されています。

HC-SR04または超音波センサーは、障害物検出および距離測定アプリケーション、ならびに様々なその他のアプリケーションを作成するために、幅広い電子プロジェクトで使用されています。ここでは、Arduinoと超音波センサーで距離を測定する簡単な方法と、Arduinoで超音波センサーを使用する方法をご紹介します。

**仕様**

![](media/image-20250908154036832.png)

- 電源供給：+5V DC
- 静止電流：<2mA
- 動作電流：15mA
- 有効角度：<15°
- 測定距離：2cm～400cm
- 分解能：0.3cm
- 測定角度：30度
- トリガー入力パルス幅：10μS

**部品**

![](media/image-20250908154147825.png)

**超音波センサーの原理**

上の画像に示すように、2つの目のようなものです。一つは送信端、もう一つは受信端です。

超音波モジュールはトリガー信号の後に超音波を放出します。超音波が物体に遭遇して反射されると、モジュールはエコー信号を出力するため、トリガー信号とエコー信号の時間差から物体までの距離を判定できます。

tは、放出信号が障害物に遭遇して戻る時間です。空気中の音の伝播速度は約343m/sであり、距離=速度×時間です。ただし、超音波は放出されて戻ってくるため、距離の2倍になります。したがって、2で割る必要があり、超音波で測定された距離=（速度×時間）/2です。

1. SR04超音波モジュールの使用方法とタイミングチャート：

2. SR04のTrigピンの遅延時間を最低10μsに設定して、距離検出をトリガーできます。
3. トリガー後、モジュールは自動的に8つの40KHz超音波パルスを送信し、信号の戻りがあるかどうかを検出します。このステップはモジュールによって自動的に完了します。
4. 信号が戻る場合、Echoピンは高レベルを出力し、高レベルの継続時間は超音波の送信から戻るまでの時間です。

![](media/image-20250908154407063.png)

超音波センサーの回路図：

![](media/image-20250908154422828.png)

**接続図**

![](media/image-20250908154455132.png)

配線ガイド：

- 超音波センサー keyestudio V5センサーシールド
- VCC → 5v(V)
- Trig → 5(S)
- Echo → 4(S)
- Gnd → Gnd(G)

**テストコード**

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 5
超音波センサー
http://www.keyestudio.com
*/
int trigPin = 5; // トリガー
int echoPin = 4; // エコー
long duration, cm, inches;

void setup() 
{
    // シリアルポート開始
    Serial.begin (9600);
    // 入出力を定義
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);
}
void loop() 
{
    // センサーは10マイクロ秒以上のHIGHパルスでトリガーされます。
    // 事前にLOWパルスを短く与えて、クリーンなHIGHパルスを確保します：
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    // センサーからの信号を読み取ります：ピングの送信から物体からのエコーの受信までの時間（マイクロ秒）の継続時間のHIGHパルス。
    duration = pulseIn(echoPin, HIGH);
    // 時間を距離に変換
    cm = (duration/2) / 29.1; // 29.1で割るか、0.0343を掛ける
    inches = (duration/2) / 74; // 74で割るか、0.0135を掛ける
    Serial.print(inches);
    Serial.print("in, ");
    Serial.print(cm);
    Serial.print("cm");
    Serial.println();
    delay(250);
}
```

**テスト結果**

開発ボードにテストコードをアップロードし、シリアルモニターを開いてボーレートを9600に設定します。検出された距離が表示され、単位はcmとインチです。超音波センサーを手で遮ると、表示される距離値が小さくなります。

![](media/image-20250908154718663.png)

**コード説明**

**int trigPin = 5;** このピンは超音波を送信するために定義され、通常は出力です。

**int echoPin = 4;** これは受信ピンとして定義され、通常は入力です。

**cm = (duration/2) / 29.1; inches = (duration/2) / 74; by 0.0135**

次の公式を使用して距離を計算できます：

距離=（移動時間/2）×音速

音速は：343m/s = 0.0343 cm/μS = 1/29.1 cm/μS

またはインチ：13503.9in/s = 0.0135in/μS = 1/74in/μS

波が送信され、物体に当たり、センサーに戻ってくることを考慮する必要があるため、移動時間を2で割る必要があります。

**拡張練習：**

超音波で測定された距離を表示しました。測定された距離でLEDを制御するのはどうでしょうか？試してみて、LEDライトモジュールをD10ピンに接続してください。

![](media/image-20250908154848028.png)

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 5
 超音波LED
 http://www.keyestudio.com
*/ 
int trigPin = 5;    // トリガー
int echoPin = 4;    // エコー
long duration, cm, inches;

void setup() 
{
  // シリアルポート開始
  Serial.begin (9600);
  // 入出力を定義
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() 
{
  // センサーは10マイクロ秒以上のHIGHパルスでトリガーされます。
  // 事前にLOWパルスを短く与えて、クリーンなHIGHパルスを確保します：
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // センサーからの信号を読み取ります：ピングの送信から物体からのエコーの受信までの時間（マイクロ秒）の継続時間のHIGHパルス。
  duration = pulseIn(echoPin, HIGH);
  // 時間を距離に変換
  cm = (duration/2) / 29.1;     // 29.1で割るか、0.0343を掛ける
  inches = (duration/2) / 74;   // 74で割るか、0.0135を掛ける
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  Serial.print("cm");
  Serial.println();
  delay(250);
  if (cm>=2 && cm<=10)
  	digitalWrite(10, HIGH);
  delay(1000);
  digitalWrite(10, LOW);
  delay(1000);
}
```

開発ボードにテストコードをアップロードし、超音波センサーを手で遮ってから、LEDが点灯しているかどうかを確認してください。
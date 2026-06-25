# プロジェクト14 Bluetooth制御ロボット

![](media/image-20250908173626827.png)

**説明**

Bluetoothの基本知識を学びました。このレッスンでは、Bluetooth遠隔操作スマートカーを作成します。実験では、HM-10 Bluetoothモジュールをスレーブ、携帯電話をホストとして設定します。

keyes BTカーはkeyestudioチームがリリースしたアプリです。これを使用してロボットカーを簡単に制御できます。

**アプリ**

- **Android APP**

  - こちらからアプリをダウンロードしてください。

  ![](media/image-20250909110725934.png)

  - Tank_Carアイコンをタップして、Bluetooth APPを開きます。以下のように表示されます。

    ![](media/image-20250909112000615.png)

  - UNO R3ボードにコードをアップロードし、Bluetoothモジュールを接続すると、BluetoothモジュールのLEDが点滅します。その後、アプリのCONNECTオプションをタップして、Bluetoothを検索します。

  ![](media/image-20250909112142944.png)

  - Bluetoothをクリックして接続します。HMSoftが接続され、Bluetooth LEDが通常点灯します。

  ![](media/image-20250909112205361.png)

  - ボタン![](media/image-20250909112233736.png)①をタップすると、8x16 LEDパネルに前進アイコンが表示されます。ボタンを離すと、「STOP」が表示されます。
  - 以下はタンクロボットBluetooth APPインターフェースで、各キーの機能を一覧にしました：

  ![](media/image-20250909112313634.png)

  ![](media/image-20250909111647904.png)

  ![](media/image-20250909111712783.png)

- **iOS APP**

  - APP Storeを開きます

    ![](media/wps1.jpg)

  - keyestudioを検索すると、keyes BTカーが表示されます。

    ![](media/wps2.jpg)

  - keyes BTカーをタップして開きます

  - Bluetoothを開くには、左上隅の「Connect」をクリックして、Bluetoothを検索して接続します。

  ![](media/wps3.jpg)

  - Tank_Carアイコンをタップして、制御インターフェースを開きます。

    ![](media/wps4.jpg)

    ![](media/wps5.jpg)

  - 以下はタンクロボットBluetooth APPインターフェースで、各キーの機能を一覧にしました：

![](media/wps6.jpg)

![](media/image-20250909111647904.png)

![](media/image-20250909111712783.png)

**テストコード**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 14.1
 bluetooth test
 http://www.keyestudio.com
*/
char ble_val; //文字変数、Bluetooth受信値を保存するために使用
void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
  if(Serial.available() > 0)  //バッファエリアにデータがあるかどうかを判定
  { ble_val = Serial.read();  //シリアルバッファからデータを読み込む
    Serial.println(ble_val);  //出力
  }
}//**************************************************************
```

**Bluetoothモジュールを取り外し、テストコードをアップロードし、Bluetoothモジュールを再度接続して、シリアルモニターを開き、ボーレートを9600に設定します。Bluetoothモジュールに向けてアプリのキーを押すと、対応する文字が以下のように表示されます。**

![](media/image-20250908173736780.png)

検出された文字と対応する機能：

![](media/image-20250908173843012.png)

![](media/image-20250908173856246.png)

**接続図**

![](media/image-20250908173908251.png)

**配線に関する注意：**

| 8x16 LEDパネル                                               |      | 拡張ボード |
| ------------------------------------------------------------ | ---- | --------------- |
| GND                                                          | →    | -（GND）        |
| VCC                                                          | →    | +（VCC）        |
| SDA                                                          | →    | SDA             |
| SCL                                                          | →    | SCL             |
| Bluetoothモジュールを垂直に挿入します。STATEおよびBRKピンに接続する必要はありません |      |                 |

**テストコード**

**注意：** テストコードをアップロードする前に、Bluetoothモジュールを取り外してください。そうしないと、テストコードのアップロードに失敗します。

```c
/*
 keyestudio Robot Car v2.0
 lesson 14.2
 bluetooth car
 http://www.keyestudio.com
*/

//配列、パターンのデータを保存するために使用、自分で計算するか、モジュラスツールから取得できます
unsigned char start01[] = {0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = {0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = {0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = {0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  //クロックピンをA5に設定
#define SDA_Pin  A4  //データピンをA4に設定

#define ML_Ctrl 13  //左モーターの方向制御ピンを定義
#define ML_PWM 11   //左モーターのPWM制御ピンを定義
#define MR_Ctrl 12  //右モーターの方向制御ピンを定義
#define MR_PWM 3    //右モーターのPWM制御ピンを定義

char bluetooth_val; //Bluetooth受信値を保存

void setup(){
  Serial.begin(9600);
  
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  matrix_display(clear);    //ディスプレイをクリア
  matrix_display(start01);  //スタートパターンを表示

  pinMode(ML_Ctrl, OUTPUT);
  pinMode(ML_PWM, OUTPUT);
  pinMode(MR_Ctrl, OUTPUT);
  pinMode(MR_PWM, OUTPUT);
}

void loop(){
  if (Serial.available())
  {
    bluetooth_val = Serial.read();
    Serial.println(bluetooth_val);
  }
  switch (bluetooth_val) 
  {
     case 'F':  //前進コマンド
        Car_front();
        matrix_display(front);  //前進デザインを表示
        break;
     case 'B':  //後退コマンド
        Car_back();
        matrix_display(back);  //後退パターンを表示
        break;
     case 'L':  //左折命令
        Car_left();
        matrix_display(left);  //「左折」サインを表示
        break;
     case 'R':  //右折命令
        Car_right();
        matrix_display(right);  //右折サインを表示
       break;
     case 'S':  //停止コマンド
        Car_Stop();
        matrix_display(STOP01);  //停止画像を表示
        break;
  }
}

/**************ドットマトリックスの機能****************/
//この関数はドットマトリックス表示に使用されます
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();
  IIC_send(0xc0);  //アドレスを選択
  
  for(int i = 0;i < 16;i++) //パターンデータは16ビット
  {
     IIC_send(matrix_value[i]); //パターンを伝達するデータ
  }
  IIC_end();   //データパターン伝達を終了
  
  IIC_start();
  IIC_send(0x8A);  //表示制御、パルス幅を4/16に設定
  IIC_end();
}
//データ伝達を開始する条件
void IIC_start()
{
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}
//データを伝達
void IIC_send(unsigned char send_data)
{
  for(char i = 0;i < 8;i++)  //各バイトは8ビット
  {
      digitalWrite(SCL_Pin,LOW);  //クロックピンSCL Pinを下げてSDAの信号を変更
delayMicroseconds(3);
      if(send_data & 0x01)  //各ビットの1または0に従ってSDA_Pinの高低レベルを設定
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); //クロックピンSCL_Pinを上げてデータ伝達を停止
      delayMicroseconds(3);
      send_data = send_data >> 1;  //ビットごとに検出するため、データを1ビット右にシフト
  }
}
//データ伝達が終了したサイン
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
/*************モーターを実行する機能**************/
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

コードが正常にアップロードされ、DIPスイッチが右端に設定され、電源がオンになります。Bluetoothを接続した後、Bluetooth Appでスマートカーを動かすことができます。

![](media/image-20250908174053007.png)を押すと、タンクロボットが前進します。

![](media/image-20250908174111419.png)をクリックすると、スマートカーが後退します。

![](media/image-20250908174138113.png)ボタンを押すと、タンクロボットが左に曲がります。![](media/image-20250908174152325.png)をクリックすると、ロボットが右に曲がります。

![](media/image-20250908174213256.png)を押し続けると、停止します。

![](media/image-20250908174235827.png)をクリックして重力制御を有効にし、![](media/image-20250908174249747.png)をもう一度タップして重力制御を終了します。同時に、ロボットカー上の8X16 LEDパネルに対応するパターンが表示されます。
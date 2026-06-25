# プロジェクト 9 8*16 LED ボード

**説明**

![](media/image-20250908165452592.png)

ロボットに 8\*16 LED ボードを追加すると、素晴らしい効果が得られます。Keyestudio の 8\*16 ドットマトリックスはこの要件を満たすことができます。これを使用することで、顔の絵文字、パターン、またはその他の興味深いディスプレイを自分で作成できます。この 8\*16 LED ライトボードには 128 個の LED が搭載されています。マイクロプロセッサ (Arduino) のデータは 2 線バスインターフェースを通じて AiP1640 と通信し、モジュール上の 128 個の LED を制御して、ドットマトリックスに必要なパターンを生成します。配線を簡単にするために、HX-2.54 4Pin 配線が提供されています。

**仕様**

- 動作電圧: DC 3.3-5V
- 消費電力: 400mW
- 発振周波数: 450KHz
- 駆動電流: 200mA
- 動作温度: -40\~80℃
- 通信方式: 2 線バス

**コンポーネント**

![](media/image-20250908165719665.png)

**8*16 ドットマトリックスディスプレイ**

回路図

![](media/image-20250908165746529.png)

**8\*16 ドットマトリックスの原理:**

8\*16 ドットマトリックスの各 LED ライトを制御するにはどうすればよいでしょうか？1 バイトは 8 ビットを持ち、各ビットは 0 または 1 です。ビットが 0 の場合は LED をオフにし、ビットが 1 の場合は LED をオンにします。したがって、1 バイトはドットマトリックスの 1 行の LED を制御でき、16 バイトは 16 列の LED ライトを制御できます。つまり、8\*16 ドットマトリックスです。

**インターフェース説明と通信プロトコル:**

マイクロプロセッサ (Arduino) のデータは 2 線バスインターフェースを通じて AiP1640 と通信します。

通信プロトコル図は以下の通りです:

(SCLK) は SCL、(DIN) は SDA です:

![](media/image-20250908165823763.png)

①データ入力の開始条件: SCL がハイレベルで、SDA がハイからロー に変わります。

②データコマンド設定の場合、以下の図に示すような方法があります:

サンプルプログラムでは、**アドレスに 1 を自動的に追加する**方法を選択し、バイナリ値は 0100 0000 で、対応する 16 進値は 0x40 です。

![](media/image-20250908165925549.png)

③アドレスコマンド設定の場合、以下に示すようにアドレスを選択できます。

サンプルプログラムでは最初の 00H を選択し、バイナリ数 1100 0000 は 16 進数 0xc0 に対応します。

![](media/image-20250908165938702.png)

④データ入力の要件は、データ入力時に SCL がハイレベルであり、SDA 上の信号は変わらないままである必要があります。SCL 上のクロック信号がロー レベルの場合のみ、SDA 上の信号を変更できます。データ入力は下位ビットが最初で、上位ビットが後です。

⑤データ伝送を終了する条件は、SCL がロー の場合 SDA がロー で、SCL がハイ の場合 SDA レベルもハイ になることです。

⑥ディスプレイ制御、異なるパルス幅を設定します。パルス幅は以下に示すように選択できます。

この例では、パルス幅 4/16 を選択し、16 進数は 1000 1010 で 0x8A に対応します。

![](media/image-20250908170005446.png)

4\. モジュラスツール紹介

ドットマトリックスモジュラスツールのオンライン版:

[http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)

①リンクを開いて以下のページに入ります。

![](media/image-20250908170027144.png)

②このプロジェクトのドットマトリックスは 8\*16 なので、高さを 8、幅を 16 に設定します。以下に示すとおりです。

![](media/image-20250908170040925.png)

③パターンから 16 進数データを生成します

以下に示すように、左マウスボタンを押して選択し、右ボタンでキャンセルし、必要なパターンを描き、**Generate** をクリックすると、必要な 16 進数データが生成されます。

![](media/image-20250908170144895.png)

**接続図**

![](media/image-20250908170158402.png)

配線注意: 8x16 LED パネルの GND、VCC、SDA、SCL は、それぞれ keyestudio センサー拡張ボードの -(GND)、+ (VCC)、A4、A5 に接続され、2 線シリアル通信を行います。(注: このピンは Arduino IIC に接続されていますが、このモジュールは IIC 通信ではありません。任意の 2 つのピンで接続できます。)

**テストコード**

スマイルフェイスを表示するコードです。

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 9.1
 Matrix  face
 http://www.keyestudio.com
*/ 
// モジュラスツールからのスマイリーフェイスのデータ
unsigned char smile[] = {0x00, 0x00, 0x1c, 0x02, 0x02, 0x02, 0x5c, 0x40, 0x40, 0x5c, 0x02, 0x02, 0x02, 0x1c, 0x00, 0x00};

#define SCL_Pin  A5  // クロックピンを A5 に設定
#define SDA_Pin  A4  // データピンを A4 に設定

void setup()
{
  // ピンを出力に設定
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // ディスプレイをクリア
  // matrix_display(clear);
}

void loop()
{
  matrix_display(smile);  // スマイルフェイスを表示
}
// ドットマトリックスディスプレイの関数

void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // データ伝送開始条件の関数を使用
  IIC_send(0xc0);  // アドレスを選択
  
  for(int i = 0;i < 16;i++) // パターンデータは 16 ビット
  {
     IIC_send(matrix_value[i]); // パターンデータを送信
  }

  IIC_end();   // パターンデータの伝送を終了
  IIC_start();
  IIC_send(0x8A);  // ディスプレイ制御、パルス幅を 4/16 に設定
  IIC_end();
}

// データ伝送を開始する条件
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
  for(char i = 0;i < 8;i++)  // 各バイトは 8 ビット
  {
      digitalWrite(SCL_Pin,LOW);  // クロックピン SCL_Pin をプルダウンして SDA の信号を変更
      delayMicroseconds(3);
      if(send_data & 0x01)  // 各ビットの 1 または 0 に応じて SDA_Pin のハイ・ロー レベルを設定
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // クロックピン SCL_Pin をプルアップして伝送を停止
      delayMicroseconds(3);
      send_data = send_data >> 1;  // ビットごとに検出し、データを右に 1 ビットシフト
  }
}

// データ伝送を終了する標識
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
//******************************************************
```

**テスト結果**

接続図に従って配線します。DIP スイッチを右端に切り替えて電源を入れると、ドットマトリックスにスマイルフェイスが表示されます。

![](media/image-20250908170421220.png)

**拡張練習**

モジュロツール ([http://dotmatrixtool.com/#](http://dotmatrixtool.com/#)) を使用して、ドットマトリックスが前進、停止パターンを交互に表示し、パターンをクリアし、時間間隔は 2000 ミリ秒です。

![](media/image-20250908170445861.png)

![](media/image-20250908170452897.png)

![](media/image-20250908170459213.png)

モジュラスツールを使用して表示するグラフィックコードを取得します。

**スタート:** 0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01

**前進:** 0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**後退:** 0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00

**左回転:** 0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00

**右回転:** 0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00

**停止:** 0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00

**表示をクリアするコード**: 0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00

複数のパターンがシフトするコード:

```c
/* keyestudio Mini Tank Robot V2.1
 lesson 9.2
 Matrix loop
 http://www.keyestudio.com
*/ 
// 配列、パターンのデータを保存するために使用、自分で計算するか、モジュラスツールから取得できます
unsigned char start01[] = 
{0x01,0x02,0x04,0x08,0x10,0x20,0x40,0x80,0x80,0x40,0x20,0x10,0x08,0x04,0x02,0x01};
unsigned char front[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x12,0x09,0x12,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char back[] = 
{0x00,0x00,0x00,0x00,0x00,0x24,0x48,0x90,0x48,0x24,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char left[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x44,0x28,0x10,0x44,0x28,0x10,0x44,0x28,0x10,0x00};
unsigned char right[] = 
{0x00,0x10,0x28,0x44,0x10,0x28,0x44,0x10,0x28,0x44,0x00,0x00,0x00,0x00,0x00,0x00};
unsigned char STOP01[] = 
{0x2E,0x2A,0x3A,0x00,0x02,0x3E,0x02,0x00,0x3E,0x22,0x3E,0x00,0x3E,0x0A,0x0E,0x00};
unsigned char clear[] = 
{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
#define SCL_Pin  A5  // クロックピンを A5 に設定
#define SDA_Pin  A4  // データピンを A4 に設定
void setup(){
  // ピンを出力に設定
  pinMode(SCL_Pin,OUTPUT);
  pinMode(SDA_Pin,OUTPUT);
  // ディスプレイをクリア
  matrix_display(clear);
}
void loop(){
  matrix_display(start01);  // スタートパターンを表示
  delay(2000);
  matrix_display(front);    // 前進パターン
  delay(2000);
  matrix_display(STOP01);   // 停止パターン
  delay(2000);
  matrix_display(clear);    // ディスプレイをクリア
  delay(2000);
}
// この関数はドットマトリックスの表示に使用されます
void matrix_display(unsigned char matrix_value[])
{
  IIC_start();  // データ伝送開始の関数を呼び出す
  IIC_send(0xc0);  // アドレスを選択
  
  for(int i = 0;i < 16;i++) // パターンデータは 16 ビット
  {
     IIC_send(matrix_value[i]); // パターンを送信するデータ
  }
  IIC_end();   // パターンデータの伝送を終了
  IIC_start();
  IIC_send(0x8A);  // ディスプレイ制御、パルス幅を 4/16 に設定
  IIC_end();
}
// データ伝送を開始する条件
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
  for(char i = 0;i < 8;i++)  // 各バイトは 8 ビット
  {
      digitalWrite(SCL_Pin,LOW);  // クロックピン SCL をプルダウンして SDA の信号を変更
      delayMicroseconds(3);
      if(send_data & 0x01)  // 各ビットの 1 または 0 に応じて SDA_Pin のハイ・ロー レベルを設定
      {
        digitalWrite(SDA_Pin,HIGH);
      }
      else
      {
        digitalWrite(SDA_Pin,LOW);
      }
      delayMicroseconds(3);
      digitalWrite(SCL_Pin,HIGH); // クロックピン SCL_Pin をプルアップしてデータ伝送を停止
      delayMicroseconds(3);
      send_data = send_data >> 1;  // ビットごとに検出し、データを右に 1 ビットシフト
  }}
// データ伝送が終了する標識
void IIC_end()
{
  digitalWrite(SCL_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);} 
//*****************************************************
```

開発ボードにコードをアップロードすると、8\*16 ドットマトリックスが前進、後退、停止パターンを交互に表示します。

![](media/image-20250908170902116.png)

![](media/image-20250908170913925.png)

![](media/image-20250908170921862.png)
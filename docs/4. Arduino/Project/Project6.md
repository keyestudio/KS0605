# プロジェクト6 IR受信

**説明**

赤外線リモコンが日常生活に普及していることは疑いの余地がありません。テレビ、ステレオ、ビデオレコーダー、衛星信号受信機など、様々な家電製品の制御に使用されています。赤外線リモコンは赤外線送信システムと赤外線受信システムで構成されており、つまり赤外線リモコンと赤外線受信モジュール、およびデコード機能を持つシングルチップマイクロコンピュータで構成されています。

![](media/image-20250908155801467.png)

リモコンから送出される38K赤外線キャリア信号は、リモコン内のエンコーディングチップによってエンコードされます。これはパイロットコード、ユーザーコード、ユーザー逆コード、データコード、およびデータ逆コードのセクションで構成されています。パルスの時間間隔は、0または1の信号であるかを区別するために使用され、エンコーディングはこれらの0、1信号で構成されています。

同じリモコンのユーザーコードは変わりませんが、データコードはキーを区別することができます。

リモコンのボタンが押されると、リモコンは赤外線キャリア信号を送出します。IR受信機が信号を受け取ると、プログラムはキャリア信号をデコードし、どのキーが押されたかを判定します。MCUは受け取った01信号をデコードすることで、リモコンのどのキーが押されたかを判定します。

使用する赤外線受信機は赤外線受信モジュールです。主に赤外線受信ヘッドで構成されており、受信、増幅、復調を統合したデバイスです。その内部ICは復調を完了しており、赤外線受信から出力までを実現でき、TTL信号と互換性があります。

さらに、赤外線リモコンおよび赤外線データ伝送に適しています。受信機で製造された赤外線受信モジュールはわずか3本のピン、信号線、VCC、GNDのみを持っています。Arduinoおよび他のマイクロコントローラとの通信が非常に便利です。

**仕様**

![](media/image-20250908160124669.png)

![](media/image-20250908160132699.png)

- 動作電圧: 3.3-5V（DC）
- インターフェース: 3PIN
- 出力信号: デジタル信号
- 受信角度: 90度
- 周波数: 38khz
- 受信距離: 10m

**コンポーネント**

![](media/image-20250908160309873.png)

**接続図**

![](media/image-20250908160331260.png)

IR受信モジュールの「-」、「+」、Sをそれぞれkeyestudio開発ボードのG（GND）、V（VCC）、A0に接続します。

**注意:** デジタルポートが利用できない場合、アナログポートをデジタルポートとして使用できます。A0はD14に相当し、A1はデジタル15に相当します。

**テストコード**

コードを設計する前に、まずIR受信モジュールのライブラリファイルをインポートしてください（Arduinoライブラリファイルのインポート方法を参照）。

```c
/*
keyestudio Mini Tank Robot V2.1
lesson 6
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>     // IRremoteライブラリステートメント
int RECV_PIN = A0;        // IR受信機のピンをA0として定義
IRrecv irrecv(RECV_PIN);   
decode_results results;   // デコード結果は「decode results」の「result」に存在
void setup()  
  {
      Serial.begin(9600);  
      irrecv.enableIRIn(); // 受信機を有効にする
  }  
 void loop() {  
    if (irrecv.decode(&results))// デコード成功、赤外線信号のセットを受信
    {  
      Serial.println(results.value, HEX);// 16進数でワードをラップして出力し、受信コードを表示
      irrecv.resume(); // 次の値を受信
    }  
    delay(100);  
  }
```

 **テスト結果**

テストコードをアップロードし、シリアルモニターを開いてボーレートを9600に設定し、リモコンをIR受信機に向けると、対応する値が表示されます。長く押し続けると、エラーコードが表示されます。

![](media/image-20250908160550590.png)

以下は、keyestudioリモコンの各ボタン値をリストアップしたものです。参考のために保管しておくことができます。

![](media/image-20250908160603853.png)

**コード説明**

**irrecv.enableIRIn():** IR デコーディングを有効にした後、IR信号が受信され、その後、関数「decode()」は継続的にデコードが成功したかどうかをチェックします。

**irrecv.decode(&results):** デコードが成功した後、この関数は「true」を返し、結果を「results」に保持します。IR信号をデコードした後、resume()関数を実行して次の信号を受信します。

**拡張練習**

IRリモコンのキー値をデコードしました。測定値でLEDを制御するのはどうでしょうか？実験を行って確認することができます。LEDをD10に接続し、リモコンのキーを押してLEDのオン・オフを制御します。

![](media/image-20250908160749345.png)

```c
/* keyestudio Mini Tank Robot V2.1
lesson 6.2
IRremote
http://www.keyestudio.com
*/ 
#include <IRremoteTank.h>
int RECV_PIN = A0;// IR受信機のピンをA0として定義
int LED_PIN=10;// LEDのピンを定義
int a=0;
IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
  Serial.begin(9600);
  irrecv.enableIRIn(); // IR受信機を初期化
  pinMode(LED_PIN,OUTPUT);// LEDのピンを出力に設定
}

void loop() 
{
  if (irrecv.decode(&results)) 
  {
	Serial.println(results.value, HEX);// 16進数でワードをラップして出力し、受信コードを表示
	if(results.value==0xFF02FD &a==0) // 上記のキー値に従い、リモコンの「OK」を押すと、LEDが制御されます
	{
		digitalWrite(LED_PIN,HIGH);// LEDがオンになります
		a=1;
	}
	else if(results.value==0xFF02FD &a==1) // もう一度押す
	{
        digitalWrite(LED_PIN,LOW);// LEDがオフになります
        a=0;	
	}
	irrecv.resume(); // 次の値を受信
  }
}
```

開発ボードにコードをアップロードし、リモコンの「OK」キーを押してLEDのオン・オフを制御します。
# プロジェクト15: 最終的な完全機能プロジェクト

**テストコード**

```c
/*
 keyestudio Mini Tank Robot V2.1
 lesson 15
 bluetooth tank
 http://www.keyestudio.com
*/

//配列、パターンのデータを保存するために使用されます。自分で計算するか、モジュラスツールから取得できます
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

void setup()
{
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

void loop()
{
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
     case 'B':  //後進コマンド
        Car_back();
        matrix_display(back);  //後進パターンを表示
        break;
     case 'L':  //左旋回指示
        Car_left();
        matrix_display(left);  //「左旋回」サインを表示
        break;
     case 'R':  //右旋回指示
        Car_right();
        matrix_display(right);  //右旋回サインを表示
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
      digitalWrite(SCL_Pin,LOW);  //クロックピンSCL_Pinを下げてSDAの信号を変更
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
```

**テスト結果**

**注意: **テストコードをアップロードする前にBluetoothモジュールを取り外してください。そうしないとテストコードのアップロードに失敗します。テストコードをアップロードした後、Bluetoothモジュールを再度接続してください

テストコードが正常にアップロードされたら、Bluetoothモジュールを挿入し、電源を入れてBluetoothに接続してください。タンクロボットはアプリで明確な機能を表示できます。

以上で、すべてのプロジェクトが完了しました。問題が発生した場合は、お気軽にお問い合わせください。
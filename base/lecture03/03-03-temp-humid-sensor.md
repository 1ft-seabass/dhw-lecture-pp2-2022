# Grove 温度・湿度センサー を動かす

![image](https://i.gyazo.com/8d5578f02a146a3410715b675a3620c2.jpg)

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/83d7047c299d09bae2db87215ceda1e6.jpg)

書き込むと 温度と湿度データが表示されます。

## Grove への Grove ケーブルのつなぎかた

Grove と Grove ケーブルのツメを合わせるように差し込みます。

![image](https://i.gyazo.com/8ce9947a4620d5891ca3548ff1afed3e.jpg)

このように差し込みました。

## M5Stack への Grove ケーブルのつなぎかた

![image](https://i.gyazo.com/799ed69fbe277c5ba0e4fd518fe07e5e.jpg)

裏面右側のピン番号を合わせて以下のようにつなぎます。

- 赤ケーブル
  - 5V
- 黒ケーブル
  - G
- 白ケーブル
  - つながない
- 黄ケーブル
  - 2

## ライブラリをインストール

GitHub の [Seeed\-Studio/Grove\_Temperature\_And\_Humidity\_Sensor のライブラリページ](https://github.com/Seeed-Studio/Grove_Temperature_And_Humidity_Sensor) にアクセスします。

![image](https://i.gyazo.com/6d89dd6d6e9e7cb525293399e4bdf363.png)

Code ボタンをクリックして Download ZIP ボタンからダウンロードします。

![image](https://i.gyazo.com/854ed3b2de05ebb38e187135f9e58de2.png)

スケッチ > ライブラリをインクルード > .ZIP形式のライブラリをインストール をクリックします。

![image](https://i.gyazo.com/bee83bbc530e3a6b8df8b7291212a744.png)

先ほどダウンロードした ZIP ファイルを選択してインストールします。こちらは Windows のファイル選択ですが、Mac の人は適宜読み替えてください。

これでインストール完了です。

![image](https://i.gyazo.com/0926b156ff8ee87a9a8b626d80bbeb92.png)

ツール > ライブラリを管理で、

![image](https://i.gyazo.com/0d505f636d7fa9263971c8af80daccb6.png)

タイプをインストール済みで絞り込んで、Grove Temperature And Humidity Sensor がインストールされていればインストール成功です。

## ソースコードを反映＆保存

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-10-TempHumid-sensor` というファイル名で保存します。

```c
#include <M5Stack.h>

// 今回のセンサー DHT 11 のライブラリの呼び出し
#include "DHT.h"
#define DHTTYPE DHT11   // DHT 11
// 黄色いケーブルを挿すピンは 2 番ピン
#define DHTPIN 2
// ライブラリの宣言
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  // init lcd, serial, but don't init sd card
  M5.begin(true, false, true);

  Wire.begin();
  
  M5.Power.begin();

  M5.Lcd.clear(BLACK);
  M5.Lcd.setTextSize(3);
  M5.Lcd.print("TempHumid");
  M5.Lcd.setTextColor(WHITE);

  // スピーカーぱちぱち音対策
  dacWrite(25, 0); // Speaker OFF

  dht.begin();
}

void loop() {
  M5.update();

  // 温度データを取得
  float temp_hum_val[2] = {0};
  
  if (!dht.readTempAndHumidity(temp_hum_val)) {
      Serial.print("Humid: ");
      Serial.print(temp_hum_val[0]);
      Serial.print(" %\t");
      Serial.print("Temp: ");
      Serial.print(temp_hum_val[1]);
      Serial.println(" *C");
  } else {
      Serial.println("Failed to get temprature and humidity value.");
  }
  
  M5.Lcd.clear(BLACK);
  M5.Lcd.setCursor(0, 100);
  // 1行目
  M5.Lcd.print("Humid : ");
  M5.Lcd.print(temp_hum_val[0]);
  M5.Lcd.println(" %");
  // 2行目
  M5.Lcd.print("Temp : ");
  M5.Lcd.print(temp_hum_val[1]);
  M5.Lcd.println(" C");

  delay(1500);
}
```

## M5Stack に書き込んでみる

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

### A fatal error occurred: Timed out waiting for packet header が出たら

![image](https://i.gyazo.com/c6d032401793930db1f0cf8909df7c92.png)

このエラーが出た場合は、

![image](https://i.gyazo.com/d56186fe56aedbdb20ab093d65179cda.jpg)

一度、Grove ケーブルを外してから、書き込んでみましょう。温度湿度センサーの仕組み（ Wire あたり）が、ときどき処理を邪魔することがあることが原因です。

## 動かしてみる

![image](https://i.gyazo.com/83d7047c299d09bae2db87215ceda1e6.jpg)

動かしてみると、このように温度と湿度データが表示されます。

2 ～ 3 分しばらく置いていると安定してきますが、他の温度計と比べてみると高めに推移しやすいので、実際に使う場合は、他の温度計で示す温度と見比べて、補正して対応しましょう。

## LINE BOT と連携するソースコードを試す

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-10-TempHumid-sensor-LINE-BOT` というファイル名で保存します。

```c
#include <M5Stack.h>

// 以下2つはHTTPSでデータを送るためのライブラリ
#include <WiFiClientSecure.h>
#include <ssl_client.h>

// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";

// 今回のセンサー DHT 11 のライブラリの呼び出し
#include "DHT.h"
#define DHTTYPE DHT11   // DHT 11
// 黄色いケーブルを挿すピンは 2 番ピン
#define DHTPIN 2
// ライブラリの宣言
DHT dht(DHTPIN, DHTTYPE);

// カウントダウン用変数 メッセージを送った時間
long messageSentAt = 0;

void setup() {
  // init lcd, serial, but don't init sd card
  // LCD ディスプレイとシリアルは動かして、SDカードは動かさない設定
  M5.begin(true, false, true);

  // スタート
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(10, 10);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.setTextSize(2);

  // Arduino のシリアルモニタ・M5Stack LCDディスプレイ両方にメッセージを出す
  Serial.print("START");  // Arduino のシリアルモニタにメッセージを出す
  M5.Lcd.print("START");  // M5Stack LCDディスプレイにメッセージを出す（英語のみ）
   
  // WiFi 接続開始
  WiFi.begin(ssid, password);
 
  while (WiFi.status() != WL_CONNECTED) {
      delay(500);

      // Arduino のシリアルモニタ・M5Stack LCDディスプレイ両方にメッセージを出す
      Serial.print(".");
      M5.Lcd.print(".");
  }

  // WiFi Connected
  // WiFi 接続完了
  M5.Lcd.setCursor(10, 40);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.setTextSize(2);

  // Arduino のシリアルモニタ・M5Stack LCDディスプレイ両方にメッセージを出す
  // 前のメッセージが print で改行入っていないので println で一つ入れる
  Serial.println("");  // Arduino のシリアルモニタにメッセージを出し改行が最後に入る
  M5.Lcd.println("");  // M5Stack LCDディスプレイにメッセージを出す改行が最後に入る（英語のみ） 
  
  // Arduino のシリアルモニタ・M5Stack LCDディスプレイ両方にメッセージを出す
  Serial.println("WiFi Connected.");  // Arduino のシリアルモニタにメッセージを出す
  M5.Lcd.println("WiFi Connected.");  // M5Stack LCDディスプレイにメッセージを出す（英語のみ）
  
  // 起動時に送る
  delay(1000);
  send_message("{\"message\":\"Launched!\"}");

  // スピーカーぱちぱち音対策
  dacWrite(25, 0); // Speaker OFF

  // 温度湿度センサーの開始
  dht.begin();
}

// HTTP でメッセージ送信部分
void send_message(String msg) {

  // 今回送るホスト名 GitPod のホスト名 (https://なし)を反映
  // https://3000-hoge-fuga-scnzIUgdS.gitpod.io/ の場合は 3000-hoge-fuga-scnzIUgdS.gitpod.io
  const char* hostName = "*********************.gitpod.io";

  WiFiClientSecure clientHTTPS;
  
  Serial.println("-> send_message");
  Serial.print("msg: ");
  Serial.println(msg);
  
  // ngrok の HTTPS になぜかつながらないので HTTP で対応 ポート番号変更 443 → 80
  if (!clientHTTPS.connect(hostName, 443)) {
    delay(2000);
    return;
  }
  String queryString = msg;

  // TestHTTP からの変更点 2
  // Content-Type: application/json
  // で POST 送信で /from/m5stack に送信
  String request = String("") +
   "POST /from/m5stack HTTP/1.1\r\n" +
   "Host: " + hostName + "\r\n" +
   "Content-Length: " + String(queryString.length()) +  "\r\n" + 
   "Content-Type: application/json\r\n\r\n" +
    queryString + "\r\n";
  
  clientHTTPS.print(request);
  
  Serial.println("clientHTTPS.printed");
  while (clientHTTPS.connected()) {
    String response = clientHTTPS.readStringUntil('\n');
    if (response == "\r") {
      break;
    }
  }

  // データ送信完了
  Serial.println("sended.");

  // サーバーから返答を受け取ったらデータを表示
  String response = clientHTTPS.readStringUntil('\n');

  Serial.println("response:");
  Serial.println(response);
  
  delay(2000);
}

void loop() {
  M5.update();

  // 以前の時間から現在の時間 millis() でどれだけ経過したかを計算
  long spanTime = millis() - messageSentAt;

  // 5秒 = 5000ミリ秒に1回送る
  // センサー取得時間は3秒くらいは確保する
  if (spanTime > 5000) {
    // 送った時間を更新
    messageSentAt = millis();
    // 湿度データを取得
    float humidity = dht.readHumidity();
    // 温度データを取得
    float temperature = dht.readTemperature();
  
    M5.Lcd.clear(BLACK);
    M5.Lcd.setCursor(0, 100);
    M5.Lcd.setTextSize(3);
    // 1行目
    M5.Lcd.print("Humid : ");
    M5.Lcd.print(humidity);
    M5.Lcd.println("%");
    // 2行目
    M5.Lcd.print("Temp : ");
    M5.Lcd.print(temperature);
    M5.Lcd.println("C");

    // float値を文字列で連結できるようにする
    String humidityString = String(humidity);
    String temperatureString = String(temperature);
    // JSON 形式のメッセージを送る
    send_message("{\"message\":\"humidity:" + humidityString + "% temperature:" + temperatureString + "C\"}");
  }
  
  if (M5.BtnA.wasReleased()) {
    // A ボタンを押したら JSON 形式のメッセージを飛ばす
    // \" はダブルクォーテーションで囲まれた中で JSON 内のダブルクォーテーションを表現するために \" でエスケープしてます。
    send_message("{\"message\":\"Pushed A\"}");
  } else if (M5.BtnB.wasReleased()) {
    // B ボタンを押したら JSON 形式のメッセージを飛ばす
    send_message("{\"message\":\"Pushed B\"}");
  } else if (M5.BtnC.wasReleased()) {
    // C ボタンを押したら JSON 形式のメッセージを飛ばす
    send_message("{\"message\":\"Pushed C\"}");
  }

  delay(1000);
}
```


## Wi-Fi 情報を反映

```c
// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";
```

自分のつなぎたい Wi-Fi の SSID とパスワードを反映します。

## Gitpod のホスト名を反映

```c
  // 今回送るホスト名 Gitpod のホスト名 (https://なし)を反映
  // https://3000-hoge-fuga-scnzIUgdS.gitpod.io/ の場合は 3000-hoge-fuga-scnzIUgdS.gitpod.io
  const char* hostName = "*********************.gitpod.io";
```

今回送るホスト名 Gitpod のホスト名を反映します。

## LINE BOT 連携のプログラムを M5Stack に書き込んでみる

そして、もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## LINE BOT 連携のプログラムを M5Stack に動かしてみる

起動してみると Launched! というメッセージが Gitpod 経由で LINE BOT のほうに送られます。

![image](https://i.gyazo.com/83d7047c299d09bae2db87215ceda1e6.jpg)

5秒に1回のタイミングで、センサーからデータが取得されて温度と湿度データが表示されます。

同時に、`humidity:42.00% temperature:29.00C` というメッセージが Gitpod 経由で LINE BOT のほうに送られます。

![image](https://i.gyazo.com/1a103f88247796f4e84723799560a28d.png)

LINE BOT はこのようにメッセージが送られています。

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから「M5Stack 事例を通してプロトタイプをアウトプットする世界観を学ぶ」にすすみましょう。
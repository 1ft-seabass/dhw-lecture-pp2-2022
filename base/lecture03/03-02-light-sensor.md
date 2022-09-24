# Grove 光センサー を動かす

![image](https://i.gyazo.com/f894d7925f9335f8982e33b5f56ee83d.jpg)

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/819cd1259c50711ce5521e06c0771647.jpg)

書き込みすると、光センサーが明るさを感知してディスプレイに明るさをパーセンテージで表示します。

## Grove への Grove ケーブルのつなぎかた

Grove と Grove ケーブルのツメを合わせるように差し込みます。

![image](https://i.gyazo.com/b9775c0daa5367bda28b6eb4e80dcbcc.jpg)

このように差し込みました。

## M5Stack への Grove ケーブルのつなぎかた

![image](https://i.gyazo.com/d771bba6a2e3d54e10b093c557a0acee.jpg)

裏面右側のピン番号を合わせて以下のようにつなぎます。

- 赤ケーブル
  - 5V
- 黒ケーブル
  - G
- 白ケーブル
  - つながない
- 黄ケーブル
  - 35

## ソースコードを反映＆保存

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-03-04-Light-sensor` というファイル名で保存します。

```c
#include <M5Stack.h>

// 光センサーの値
int currentLightValue = 0;

// 記録しているボタンの状態（前の状態を比較する）
int lightValue = 0;

// 黄色いケーブルを差し込む M5Stack ピン番号
int analogPin = 35;

void setup() {
  
  // LCD ディスプレイとシリアルは動かして、SDカードは動かさない設定
  M5.begin(true, false, true);

  M5.Power.begin();

  M5.Lcd.clear(BLACK);
  M5.Lcd.setTextSize(3);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.print("Light Analog");

  // スピーカーぱちぱち音対策
  dacWrite(25, 0); // Speaker OFF
}

void loop() {
  M5.update();

  // まずアナログ値の素の値として取得 0 - 4096
  int lightSensorBaseValue = analogRead(analogPin);

  // 4096 を最大値としてパーセント値にする
  currentLightValue = lightSensorBaseValue / 4096.00 * 100.00; 
  
  M5.Lcd.clear(BLACK);
  M5.Lcd.setCursor(0, 100);
  
  // 2行目
  M5.Lcd.println("[Light Sensor]");
  M5.Lcd.print("Percent : ");
  M5.Lcd.print(currentLightValue);
  M5.Lcd.println("%");
  
  delay(500);
}
```

## M5Stack に書き込んでみる

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

![image](https://i.gyazo.com/819cd1259c50711ce5521e06c0771647.jpg)

書き込みすると、光センサーが明るさを感知してディスプレイに明るさをパーセンテージで表示します。

![image](https://i.gyazo.com/ede4ddeffc7c3235e0f3439e8d0b64c3.jpg)

手で隠すと暗くなりパーセンテージが変化することも確認してみましょう。

## LINE BOT と連携するソースコードを試す

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-03-05-Light-sensor-LINEBOT` というファイル名で保存します。

```c
#include <M5Stack.h>

// HTTP 通信を行うライブラリ
#include <HTTPClient.h>

// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";

// 光センサーの値
int currentLightValue = 0;

// 明るいかくらいかの判定 1/0
int currentLightFlag = 1;

// 記録しているボタンの状態（前の状態を比較する）
int lightValue = 0;

// 黄色いケーブルを差し込む M5Stack ピン番号
int analogPin = 35;
void setup() {
  // init lcd, serial, but don't init sd card
  // LCD ディスプレイとシリアルは動かして、SDカードは動かさない設定
  M5.begin(true, false, true);

  // スピーカーぱちぱち音対策
  dacWrite(25, 0); // Speaker OFF

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
}

// HTTP でメッセージ送信部分
void send_message(String msg) {

  // 今回送るホスト名 GitPod のホスト名 (https://なし)を反映
  // https://3000-hoge-fuga-scnzIUgdS.gitpod.io/ の場合は 3000-hoge-fuga-scnzIUgdS.gitpod.io
  String hostName = "*********************.gitpod.io";

  // 今回送るURL
  String url = "https://" + hostName + "/from/m5stack";
  
  Serial.println("-> send_message");
  Serial.print("msg: ");
  Serial.println(msg);
  
  // 送るデータ
  String queryString = msg;
  // HTTPClient 準備
  HTTPClient httpClient;
  // URL 設定
  httpClient.begin(url);
  // Content-Type
  httpClient.addHeader("Content-Type", "application/json");
  
  // データ送信完了
  Serial.println("sended.");

  // ポストする
  int status_code = httpClient.POST(queryString);
  if( status_code == 200 ){
    String response = httpClient.getString();
    
    Serial.println("response:");
    Serial.println(response);
  }
  httpClient.end();
  
  delay(2000);
}

void loop() {
  M5.update();

  // まずアナログ値の素の値として取得 0 - 4096
  int lightSensorBaseValue = analogRead(analogPin);

  // 4096 を最大値としてパーセント値にする
  currentLightValue = lightSensorBaseValue / 4096.00 * 100.00; 
  
  M5.Lcd.clear(BLACK);
  M5.Lcd.setCursor(0, 100);
  
  // 2行目
  M5.Lcd.println("[Light Sensor]");
  M5.Lcd.print("Percent : ");
  M5.Lcd.print(currentLightValue);
  M5.Lcd.println("%");

  // 暗くなったらメッセージを送る
  if(currentLightFlag == 1){
    // 明るいとき 5%以上
    if( currentLightValue < 5.0 ){
      // 暗くなったらメッセージ
      send_message("{\"message\":\"Dark\"}");
      // 暗くなったフラグ
      currentLightFlag = 0;
    }
  } else {
    // 暗いとき
    if( currentLightValue > 5.0 ){
      // 明るくなったらメッセージ
      send_message("{\"message\":\"Light\"}");
      // 明るくなったフラグ
      currentLightFlag = 1;
    }
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

  delay(500);
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
  String hostName = "*********************.gitpod.io";
```

今回送るホスト名 Gitpod のホスト名を反映します。

## LINE BOT 連携のプログラムを M5Stack に書き込んでみる

そして、もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## LINE BOT 連携のプログラムを M5Stack に動かしてみる

起動してみると Launched! というメッセージが Gitpod 経由で LINE BOT のほうに送られます。

![image](https://i.gyazo.com/ede4ddeffc7c3235e0f3439e8d0b64c3.jpg)

光センサーを手で隠すと暗くなると Dark というメッセージを送ります。5 %未満。

![image](https://i.gyazo.com/819cd1259c50711ce5521e06c0771647.jpg)

暗くしてから、明るくすると Light というメッセージを送ります。

![image](https://i.gyazo.com/7ca06702a4d1771f163097131bb5d9a6.png)

LINE BOT はこのようにメッセージが送られています。

# 次にすすみましょう

左のナビゲーションから「温度・湿度センサー」にすすみましょう。
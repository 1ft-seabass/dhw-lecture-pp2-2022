# M5Stack から LINE Notify にメッセージを送ろう

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/d4f219471329ee42c7d2b291e7272873.png)

このような仕組みです。

![image](https://i.gyazo.com/e49942702a7683010b16a76d797d83e2.jpg)

Aボタン、Bボタン、Cボタンをクリックすると LINE Notify サーバーにメッセージが送られます。

![image](https://i.gyazo.com/9bc36d121afc4ac5e8b71a2b2c8fe573.png)

先ほど設定した LINE Notify のアカウントにメッセージが表示されます。

## LINE Notify のトークンを取得します

![image](https://i.gyazo.com/c45ee309aa6793bc12f67c24f3a905ce.png)

https://notify-bot.line.me/ja/ からログインします。

![image](https://i.gyazo.com/bf26cdcf093a783aee4ff510ec9ddb68.png)

ログインすると、右上にアカウント名が出るのでクリックします。

![image](https://i.gyazo.com/7cdb07c7e588bac278a5fb0b2dbc9d83.png)

マイページをクリック。

![image](https://i.gyazo.com/0c48f49af1be4e5e7c789366c0b838cf.png)

マイページの下の方に「アクセストークンの発行(開発者向け)」というエリアがあるのでスクロールします。

![image](https://i.gyazo.com/bbba9909e0437e487718479953b198ff.png)

トークンを発行するボタンをクリックします。

![image](https://i.gyazo.com/05b98371bcae556f578cdb96505ecb7c.jpg)

トークンを発行するウィンドウが表示されるので、以下のように対応します。

- トークン名を記入してください (通知の際に表示されます)
  - `M5Stack Notify` と入力
- 通知を送信するトークルームを選択してください
  - `1:1でLINE Notifyから通知を受け取る` を選択

こちらを対応すると、発行するボタンが押せるようになるのでクリックします。

![image](https://i.gyazo.com/abee7f38d2db6897c61cdb42fc29a83c.png)

このようにトークンが表示されるのでメモしましょう。

**このページから移動すると、新しく発行されたトークンは二度と表示されないので気をつけましょう**

メモしたらウィンドウを閉じるボタンをクリックして閉じます。

![image](https://i.gyazo.com/2fcf2f7f0d039737510759301a619485.png)

リストに追加されたことを確認しておきます。

![image](https://i.gyazo.com/07a33eec5562fc6fd67e52aeaa5c2bc9.png)

今回選択した LINE Notify 先のアカウントにこのようなメッセージが来ていることも確認します。

## ソースコードを反映

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。

```c
#include <M5Stack.h>
#include <WiFiClientSecure.h>
#include <ssl_client.h>

// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";

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
  
}

void send_message(String msg) {

  // LINE Notify のホスト
  const char* hostLINENotify = "notify-api.line.me";

  // LINE Notify のトークン
  const char* tokenLINENotify = "LINE Notify のトークン";
  
  WiFiClientSecure clientHTTPS;

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(10, 10);
  M5.Lcd.println("-> LINE Notify");
  Serial.println("-> LINE Notify");
  M5.Lcd.print("msg: ");
  Serial.print("msg: ");
  M5.Lcd.println(msg);
  Serial.println(msg);
  
  if (!clientHTTPS.connect(hostLINENotify, 443)) {
    delay(2000);
    return;
  }
  String queryString = String("message=") + msg;

  // LINE Notify の API に合わせて送信内容を作る
  // Content-Type は application/x-www-form-urlencoded
  // Authorization: Bearer にトークンを割り当てる
  String request = String("") +
   "POST /api/notify HTTP/1.1\r\n" +
   "Host: " + hostLINENotify + "\r\n" +
   "Authorization: Bearer " + tokenLINENotify + "\r\n" +
   "Content-Length: " + String(queryString.length()) +  "\r\n" + 
   "Content-Type: application/x-www-form-urlencoded\r\n\r\n" +
    queryString + "\r\n";
  
  clientHTTPS.print(request);
  M5.Lcd.println("clientHTTPS.printed");
  Serial.println("clientHTTPS.printed");
  while (clientHTTPS.connected()) {
    String line = clientHTTPS.readStringUntil('\n');
    if (line == "\r") {
      break;
    }
  }
  
  String response = clientHTTPS.readStringUntil('\n');
  M5.Lcd.println("sended.");
  Serial.println("sended.");

  M5.Lcd.println("response:");
  M5.Lcd.println(response);
  
  delay(2000);
}

void loop() {
  M5.update();
  
  if (M5.BtnA.wasReleased()) {
    // A ボタンを押したらテキストメッセージを LINE Notify へ送る
    send_message("Pushed A");
  } else if (M5.BtnB.wasReleased()) {
    // B ボタンを押したらテキストメッセージを LINE Notify へ送る
    send_message("Pushed B");
  } else if (M5.BtnC.wasReleased()) {
    // C ボタンを押したらテキストメッセージを LINE Notify へ送る
    send_message("Pushed C");
  }
}
```

こちらを `dhw-pp2-study-05-TestLINENotify` で保存します。

## Wi-Fi 情報を反映

```c
// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";
```

先ほどと同じように Wi-Fi 情報を反映します。

## LINE Notify のトークンを反映

```c
void send_message(String msg) {

  // LINE Notify のホスト
  const char* hostLINENotify = "notify-api.line.me";

  // LINE Notify のトークン
  const char* tokenLINENotify = "LINE Notify のトークン";
```

send_message のすぐ近くにある `const char* tokenLINENotify = "LINE Notify のトークン";` の `LINE Notify のトークン` の部分を、先ほどメモした LINE Notify のトークンに置き換えましょう。ダブルクオーテーションを消していないか気をつけましょう。

## M5Stack に書き込んでみる

そして、もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

![image](https://i.gyazo.com/e49942702a7683010b16a76d797d83e2.jpg)

Aボタン、Bボタン、Cボタンをクリックすると LINE Notify サーバーにメッセージが送られます。

![image](https://i.gyazo.com/9bc36d121afc4ac5e8b71a2b2c8fe573.png)

先ほど設定した LINE Notify のアカウントにメッセージが表示されます。

# 次にすすみましょう

左のナビゲーションから「M5Stack + LINE BOT 連携」にすすみましょう。
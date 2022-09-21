
# サーバーにメッセージを送ってみる

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/416402edf3476298c3065d0500962a92.png)

このような仕組みです。

![image](https://i.gyazo.com/e9c21a3d286467ed300b2ac6e1f48ad7.jpg)

起動時にサーバーにメッセージが送られ、その後、各ボタンをクリックしてサーバーにメッセージが送られます。

## テストサーバーを用意しました

![image](https://i.gyazo.com/b9f26a859d98bbee5dc509b2cfe3aaa2.png)

授業のためにテストサーバーを用意しました

https://dhw-pp2-test01.herokuapp.com/ui

- こういうサーバーがあるとデータがちゃんと届いているかのチェックがしやすいです
- ひとまず今回は HTTP で受信するとログが出ます
- 時間に余裕があれば軽くデモします

## ソースコードを反映

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-04-TestHTTP` で保存します。

```c
#include <M5Stack.h>

// HTTP 通信を行うライブラリ
#include <HTTPClient.h>

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
  
  // 起動時に送る
  delay(1000);
  send_message("{\"message\":\"Launched!\"}");
}

// HTTP でメッセージ送信部分
void send_message(String msg) {

  // 今回送るURL
  String url = "https://dhw-pp2-test01.herokuapp.com/dhw/pp2/http/message";

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(10, 10);
  
  M5.Lcd.println("-> send_message");
  M5.Lcd.print("msg: ");
  M5.Lcd.println(msg);

  // 送るデータ
  String queryString = msg;
  // HTTPClient 準備
  HTTPClient httpClient;
  // URL 設定
  httpClient.begin(url);
  // Content-Type
  httpClient.addHeader("Content-Type", "application/json");

  M5.Lcd.println("sended.");
  Serial.println("sended.");
  
  // ポストする
  int status_code = httpClient.POST(queryString);
  if( status_code == 200 ){
    String response = httpClient.getString();
    
    M5.Lcd.println("response:");
    M5.Lcd.println(response);
  }
  httpClient.end();
  
  delay(2000);
}

void loop() {
  M5.update();
  
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
}
```

## Wi-Fi 情報を反映して、また保存

```c
// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";
```

先ほどと同じように Wi-Fi 情報を反映します。もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

![image](https://i.gyazo.com/e9c21a3d286467ed300b2ac6e1f48ad7.jpg)

起動時に サーバーに `{"message":"Launched!"}` という JSON データが送られます。

また、3つボタンが並んでいますが、Aボタン、Bボタン、Cボタンでプログラムと対応しています。クリックしてサーバーにメッセージが送られるか確認してみましょう。

たとえば、Cボタンをクリックすると `{"message":"Pushed C"}` という JSON データが送られます。

# 送るデータを変更してみる（書き換え訓練）

これだと送った人が分からないので、英数字で自分の名前を決めて、送るデータをちょっと書き換えましょう。

## Launched メッセージをちょっと変更

```c
  // 起動時に送る
  delay(1000);
  send_message("{\"message\":\"Launched!\"}");
```

を、以下のように書き加えます。Seigo で変更した例です。

```c
  // 起動時に送る
  delay(1000);
  send_message("{\"message\":\"Seigo Launched!\"}");
```

## ボタンを押したときのメッセージをちょっと変更

```c
void loop() {
  M5.update();
  
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
}
```

を、以下のように書き加えます。Seigo で変更した例です。

```c
void loop() {
  M5.update();
  
  if (M5.BtnA.wasReleased()) {
    // A ボタンを押したら JSON 形式のメッセージを飛ばす
    // \" はダブルクォーテーションで囲まれた中で JSON 内のダブルクォーテーションを表現するために \" でエスケープしてます。
    send_message("{\"message\":\"Seigo Pushed A\"}");
  } else if (M5.BtnB.wasReleased()) {
    // B ボタンを押したら JSON 形式のメッセージを飛ばす
    send_message("{\"message\":\"Seigo Pushed B\"}");
  } else if (M5.BtnC.wasReleased()) {
    // C ボタンを押したら JSON 形式のメッセージを飛ばす
    send_message("{\"message\":\"Seigo Pushed C\"}");
  }
}
```

## 余談：英数字で顔文字表現テクニック

もし余裕があれば、

 - `(^_^)`
 - `(=_=)`
 - `:-)`
 - `;-)`
 - `(*o*)/`

のような顔文字は英数字でも表現できるので、ぜひ試してみましょう～。

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから「M5Stack から LINE BOT にメッセージを送る」にすすみましょう。
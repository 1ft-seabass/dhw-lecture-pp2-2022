# LINEBOT に M5Stack からメッセージを送る

## 事前準備：ホスト名を準備しましょう

この後の設定で、ホスト名というものが必要になるので、さきほどメモした Gitpod URL からホスト名を分離してメモしておきます。

もし、 `https://3000-hoge-fuga-scnzIUgdS.gitpod.io/` という Gitpod URL の場合は `3000-hoge-fuga-scnzIUgdS.gitpod.io` としてホスト名をメモしておきます。

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/2aac11e5dafe4f5bf9045da64dccf3e2.jpg)

書き込みと同時に Wi-Fi がつながり Launched! というメッセージが Gitpod のサーバーに送られ、 LINE BOT にメッセージが通されます。

![image](https://i.gyazo.com/9e3eb877028c7f9e4b54f1b5994ec2e6.jpg)

さらに A B C 各ボタンを押すと、

![image](https://i.gyazo.com/f1c2f6f8d5ad04acee037335b99c1bfb.png)

押したボタンの情報が LINE BOT に表示されます。

## ソースコードを反映＆保存

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-07-LINEBotMessageHTTPSToGitpod` というファイル名で保存します。

```c
#include <M5Stack.h>

// 以下2つはHTTPSでデータを送るためのライブラリ
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
  
  // 起動時に送る
  delay(1000);
  send_message("{\"message\":\"Launched!\"}");
}

// HTTP でメッセージ送信部分
void send_message(String msg) {

  // 今回送るホスト名 GitPod のホスト名 (https://なし)を反映
  // https://3000-hoge-fuga-scnzIUgdS.gitpod.io/ の場合は 3000-hoge-fuga-scnzIUgdS.gitpod.io
  const char* hostName = "*********************.gitpod.io";

  WiFiClientSecure clientHTTPS;

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(10, 10);
  
  M5.Lcd.println("-> send_message");
  M5.Lcd.print("msg: ");
  M5.Lcd.println(msg);
  
  Serial.println("-> send_messagey");
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
  M5.Lcd.println("clientHTTPS.printed");
  Serial.println("clientHTTPS.printed");
  while (clientHTTPS.connected()) {
    String response = clientHTTPS.readStringUntil('\n');
    if (response == "\r") {
      break;
    }
  }

  // データ送信完了
  M5.Lcd.println("sended.");
  Serial.println("sended.");

  // サーバーから返答を受け取ったらデータを表示
  String response = clientHTTPS.readStringUntil('\n');

  M5.Lcd.println("response:");
  M5.Lcd.println(response);
  
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

## M5Stack に書き込んでみる

そして、もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

![image](https://i.gyazo.com/2aac11e5dafe4f5bf9045da64dccf3e2.jpg)

うまく設定できていれば、書き込みと同時に Wi-Fi がつながり Launched! というメッセージが Gitpod のサーバーに送られ、 LINE BOT にメッセージが通されます。

![image](https://i.gyazo.com/9e3eb877028c7f9e4b54f1b5994ec2e6.jpg)

さらに A B C 各ボタンを押すと、

![image](https://i.gyazo.com/f1c2f6f8d5ad04acee037335b99c1bfb.png)

押したボタンの情報が LINE BOT に表示されます。

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから「ディスプレイとボタン実装」にすすみましょう。
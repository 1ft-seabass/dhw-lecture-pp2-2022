# M5Stack から LINE Notify にメッセージを送ろう

## LINE Notify とは

![image](https://i.gyazo.com/5cf50e16f926ec4f0bdc17f76b5558e5.png)

LINE Notify の大まかな仕組みを把握します。

![image](https://i.gyazo.com/5ee7b4482f1b28caaf860e72744cd2f5.png)

LINE Notify https://notify-bot.line.me/ja/

> Webサービスと連携すると、LINEが提供する公式アカウント"LINE Notify"から通知が届きます。
> 複数のサービスと連携でき、グループでも通知を受信することが可能です。

![image](https://i.gyazo.com/8dddc3621b4a77af65be36d7907cd02a.png)

つまり「LINE Notifyと連携を行うことで、LINEユーザーが簡単にサービスの通知を受信できるようになります」

## IoT における通知

LINE Notify のような IoT における通知の重要性を把握します。

![image](https://i.gyazo.com/9e70b598a6e055b9b1a7ddab800f6f8a.png)

自分の LINE に通知としてメッセージを受け取ることができる仕組みです。

- 自分から見に行かなくてもメッセージで知らせてくれる仕組み
- 複数人に一斉にメッセージを受け取る仕組み
- 何かしらの情報をまとめて知らせてくれる仕組み

といったものを作ることができます。

自分たちが設定した情報だけを手軽に LINE ユーザーに配信できるので、WEB アプリの仕組みと相性がよく「こういう通知内容は実際役に立つだろうか？」といった形で、自分たちの考えた発想を試しやすいです。

また、そういったメッセージを受け付ける部分を LINE に任せることで、LINE アプリの使い慣れた洗練されたレイアウトで、すぐにメッセージを表示できることも良い点です。

もし、このレベルに使いやすい仕組みを自分でイチからつくるとなると、WEB サーバーの知識・HTML/CSS/JavaScript といった WEB 表示の知識・セキュリティへの配慮・多くのユーザーがいる状況などなど整えるべきことが数多くあります。「こういう通知内容は実際役に立つだろうか？」というシンプルな試行錯誤をするまでに、たくさんの時間がかかります。

このあたりを、LINE Nofity および LINE アプリで、すぐに試せるというのは、自分たちの構想を試して良くしていくときには強い味方になります。

## LINE Notify 事例

LINE Notify に関する使いどころ（ユースケース）を個人面・IoT事例面で把握します

- 個人面
  - 自分の興味のありそうなカテゴリのニュースを定期的に通知
  - 家族共有の Google カレンダーが更新されると通知して共有
- IoT事例面
  - 温度・湿度センサーを設置しておき熱中症アラートを出す（冷房を促すこともセットで）
  - CO2 センサーを設置しておき一定量の濃度に到達したら換気を促す
  - 新生児の健康指標事例
    - https://www.1ft-seabass.jp/memo/2019/11/06/unko-button-getting-start/
    - https://www.1ft-seabass.jp/memo/2020/07/13/summary-of-oyako-tech-lt/
  - 事例
    - https://speakerdeck.com/1ftseabass/developers-summit-2021

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/d4f219471329ee42c7d2b291e7272873.png)

このような仕組みです。

![image](https://i.gyazo.com/e49942702a7683010b16a76d797d83e2.jpg)

Aボタン、Bボタン、Cボタンをクリックすると LINE Notify サーバーにメッセージが送られます。

![image](https://i.gyazo.com/9bc36d121afc4ac5e8b71a2b2c8fe573.png)

先ほど設定した LINE Notify のアカウントにメッセージが表示されます。

## LINE Notify のトークンを取得します

![image](https://i.gyazo.com/c45ee309aa6793bc12f67c24f3a905ce.png)

https://notify-bot.line.me/ja/ からログインします。QR コードログインで入りましょう。（ログインメールアドレス＋パスワードのログインは設定や思い出すのに苦労すると思うので、避けましょう。）

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

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-02-05-TestLINENotify` で保存します。

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
  // 勝手に Button A が押されることを回避
  WiFi.setSleep(false);
 
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
  String hostLINENotify = "notify-api.line.me";

  // LINE Notify のトークン
  String tokenLINENotify = "LINE Notify のトークン";

  // 今回送るURL
  String url = "https://" + hostLINENotify + "/api/notify";

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(10, 10);
  M5.Lcd.println("-> LINE Notify");
  Serial.println("-> LINE Notify");
  M5.Lcd.print("msg: ");
  Serial.print("msg: ");
  M5.Lcd.println(msg);
  Serial.println(msg);

  // 送るデータ
  String queryString = String("message=") + msg;
  // HTTPClient 準備
  HTTPClient httpClient;
  // URL 設定
  httpClient.begin(url);
  // Content-Type
  httpClient.addHeader("Content-Type", "application/x-www-form-urlencoded");
  // Authorization
  httpClient.addHeader("Authorization", "Bearer " + tokenLINENotify);

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
  String hostLINENotify = "notify-api.line.me";

  // LINE Notify のトークン
  String tokenLINENotify = "LINE Notify のトークン";
```

send_message のすぐ近くにある `String tokenLINENotify = "LINE Notify のトークン";` の `LINE Notify のトークン` の部分を、先ほどメモした LINE Notify のトークンに置き換えましょう。ダブルクオーテーションを消していないか気をつけましょう。

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

左のナビゲーションから「Gitpod をはじめる」にすすみましょう。
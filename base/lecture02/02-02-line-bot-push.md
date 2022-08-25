# M5Stack から LINE BOT にメッセージを送ろう

[1時間でLINE BOTを作るハンズオン \(資料\+レポート\) in Node学園祭2017 \#nodefest \- Qiita](https://qiita.com/n0bisuke/items/ceaa09ef8898bee8369d#1-bot%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B) を参考に進めます。

以下に、今回の授業のの手順として、書いてありますので、進めていきましょう。

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/d0ce73c1d8c7992d57aa01df0a323ab0.png)

このような仕組みです。

起動すると、まず `Launched` メッセージが ngrok に送られて、さらに LINE BOT にプッシュメッセージとして送られます。

![image](https://i.gyazo.com/03598f71b9402bacd545881ed0225481.jpg)

ボタンを押すと `response Hello M5Stack!(POST)` と返答が返ってきて、メッセージが ngrok に送られて、さらに LINE BOT にプッシュメッセージとして送られます。

![image](https://i.gyazo.com/9df34d328f5d0433888c75ff964c26a5.png)

うまくいけば、LINE BOT でこのように M5Stack で押した内容が表示されて連携できます。

## LINE BOT の作り方の手順まとめています

こちらにLINE BOT の作り方の手順まとめています。

[LINE BOT 作成手順](12-line-bot-create.md)

まだ、作っていない方は、ぜひ試してみてください。

## Node.js で BOT 開発

Visual Studio Code で開発していきます。

講師の環境は

- Windows 10
- Node.js 14.17.5
- npm 6.14.14

です。

## フォルダを作成して Visual Studio Code ではじめる

![image](https://i.gyazo.com/2f0b578e81dbccbedc7338d165c92bc4.png)

`dhw-pp2-linebot` というフォルダを作成します。

![image](https://i.gyazo.com/fedda70653d896c15c83be417e0c6b25.png)

こちらを Visual Studio Code で開きます。こちらを今回のプロジェクトフォルダとして使います。

## npm の初期化

プロジェクトフォルダ直下で、

```
npm init -y
```

`npm init` コマンドで `package.json` を作成します。

## ライブラリのインストール

```
npm i @line/bot-sdk express
```

コマンドで `@line/bot-sdk` と `express` のLINE BOT に関連するライブラリをインストールします。

## BOT として動かす app.js の作成

![image](https://i.gyazo.com/66bfdf2fece70796b98a935899fd943d.png)

プロジェクトフォルダ直下で Visual Studio Code の新規ファイル作成を使って `app.js` を作成します。

## app.js にソースコードを反映

`app.js` に以下のソースコードをコピーアンドペーストで反映します。

[LINE BOT 作成手順](12-line-bot-create.md) で、作成したBOTのチャンネルシークレットとチャンネルアクセストークンが必要になるので準備しておきましょう。

```js
'use strict';

const express = require('express');
const line = require('@line/bot-sdk');
const PORT = process.env.PORT || 3000;

// 作成したBOTのチャンネルシークレットとチャンネルアクセストークン
const config = {
  channelSecret: '作成したBOTのチャンネルシークレット',
  channelAccessToken: '作成したBOTのチャンネルアクセストークン'
};

// プッシュメッセージで受け取る宛先となる作成したBOTのユーザーID'
const userId = '作成したBOTのユーザーID';

const app = express();

// M5Stack からJSON データを受け取ったときに扱えるようにする設定
// https://qiita.com/kmats/items/2c2502cfa3a633e7e049
app.use('/from/m5stack', express.json()); 

app.get('/', (req, res) => res.send('Hello LINE BOT!(GET)')); //ブラウザ確認用(無くても問題ない)
app.post('/webhook', line.middleware(config), (req, res) => {
    console.log(req.body.events);

    //ここのif分はdeveloper consoleの"接続確認"用なので削除して問題ないです。
    if(req.body.events[0].replyToken === '00000000000000000000000000000000' && req.body.events[1].replyToken === 'ffffffffffffffffffffffffffffffff'){
        res.send('Hello LINE BOT!(POST)');
        console.log('疎通確認用');
        return; 
    }

    Promise
      .all(req.body.events.map(handleEvent))
      .then((result) => res.json(result));
});

const client = new line.Client(config);

async function handleEvent(event) {
  if (event.type !== 'message' || event.message.type !== 'text') {
    return Promise.resolve(null);
  }

  return client.replyMessage(event.replyToken, {
    type: 'text',
    text: event.message.text //実際に返信の言葉を入れる箇所
  });
}

// M5Stack からメッセージを受け取り LINE BOT へプッシュメッセージする部分
app.post('/from/m5stack', async function(req, res){

  console.log('M5Stack からメッセージを受け取り');
  console.log(req.body);

  const pushText = req.body.message;  // 受信した JSON データの message 値を LINE BOT へプッシュする

  client.pushMessage(userId, {
    type: 'text',
    text: pushText,
  });

  res.send('Hello M5Stack!(POST)');
});

app.listen(PORT);

console.log(`Server running at ${PORT}`);
```

## 作成したBOTのチャンネルシークレットとチャンネルアクセストークンを反映

```js
// 作成したBOTのチャンネルシークレットとチャンネルアクセストークン
const config = {
  channelSecret: '作成したBOTのチャンネルシークレット',
  channelAccessToken: '作成したBOTのチャンネルアクセストークン'
};
```

[LINE BOT 作成手順](12-line-bot-create.md) で、作成したBOTのチャンネルシークレットとチャンネルアクセストークンを、それぞれ反映します。

わりと編集中にシングルクォーテーション消してしまったり、channelSecretとchannelAccessTokenを、逆に書いたりしてハマるので気をつけましょう。

## M5Stack から来るプッシュ通知の送り先のユーザーIDを反映

まず、プッシュ通知の送り先であるこの BOT のユーザーIDを LINE Developers から取得します。

https://developers.line.biz/ja/

こちらからログインしましょう。

![image](https://i.gyazo.com/b4cff116ffa19c5ed6b6b2c98e15cedb.png)

今回使っている BOT のチャネル設定に移動します。

![image](https://i.gyazo.com/1e959b391cb50becbdff3fd3ca39b3e2.png)

下にスクロールしていくと `あなたのユーザーID` という項目があるので、これをメモしておきます。

```js
// プッシュメッセージで受け取る宛先となる作成したBOTのユーザーID'
const userId = '作成したBOTのユーザーID';
```

`作成したBOTのユーザーID` の部分を先ほどメモしたユーザーIDで書き換えます。

ここまで設定出来たらファイルを保存します。

## app.js を Node.js で実行して手元のサーバー起動

プロジェクトフォルダ直下で、

```
node app.js
```

コマンドで app.js を Node.js で実行して手元のサーバー起動します。

`Server running at 3000`

と表示されれば起動成功です。

### 動作確認してみる

ちゃんと動作しているか見てみましょう。

Chrome ブラウザで

`http://localhost:3000`

でアクセスすると、

![image](https://i.gyazo.com/4d8c47c50965ff64d49b0641670e1ddd.png)

と表示されます。

これで動作確認は完了です。

## ngrok で手元の PC に外からつながる BOT を立ち上げます

ngrok で手元の PC に外からつながる BOT を立ち上げます。 ngrok が何かは　[こちらの ngrok の説明](https://qiita.com/n0bisuke/items/ceaa09ef8898bee8369d#3-ngrok%E3%81%A7%E3%83%88%E3%83%B3%E3%83%8D%E3%83%AA%E3%83%B3%E3%82%B0) を参考にしてください。

![image](https://i.gyazo.com/67c4d65d66b6e18eb9082d1d6f6bd8fe.png)

こちらを起動している状態で、別のターミナルを立ち上げます。

```
npm i -g ngrok
```

コマンドで ngrok をインストールします。

```
ngrok http 3000
```

先ほど起動した 3000 番のポートで起動しているボットサーバーを ngrok で公開します。

  ngrok by @inconshreveable                                                                               (Ctrl+C to quit)                                                                                                                        Session Status                online                                                                                    Session Expires               1 hour, 59 minutes                                                                        Update                        update available (version 2.3.40, Ctrl-U to update)                                       Version                       2.3.35                                                                                    Region                        United States (us)                                                                        Web Interface                 http://127.0.0.1:4040                                                                     Forwarding                    http://**********.ngrok.io -> http://localhost:3000     Forwarding                    https://**********.ngrok.io -> http://localhost:3000                                                                                                                            Connections                   ttl     opn     rt1     rt5     p50     p90                                                                             0       0       0.00    0.00    0.00    0.00

といったメッセージが表示されるます。

2 つある Forwarding の項目で https が書かれているほうの `https://**********.ngrok.io` までをメモしておきます。`**********` の部分はみなさんの環境によって変わります。

## Webhook URL の更新

![image](https://i.gyazo.com/5d1457a134175c620f142f92177ab373.png)

今回使っている BOT の Messaging API 設定に移動します。

![image](https://i.gyazo.com/c85ed28c0caa79951404327636ef1a44.png)

Webhook URL の項目に移動して以下の手順を行います。（大事な設定なので、一息タイミングを置いています）

### さきほどの URL に `/webhook` をつけて

![image](https://i.gyazo.com/9f162805b70de3e064982abbf3136e3e.png)

さきほどの URL `https://**********.ngrok.io` に `/webhook` をつけて Webhook URL の項目に入力して更新ボタンをクリックします。

![image](https://i.gyazo.com/3340e1c266fdfbfda807586ea34e04b3.png)

同時に Webhook の利用がオンになっていることも確認しましょう。

## BOT にオウム返しが来るか確認しましょう

一旦ここでちゃんと動いているかオウム返しを確認してみましょう。

![image](https://i.gyazo.com/3bf69c731aca0696a7f70f2dfdd9a670.png)

## M5Stack のソースコードを反映

ここから Arduino パートです。

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。

こちら、`dhw-pp2-study-04-TestHTTP` で保存したサーバーにメッセージを送ってみるプログラムとかなり近いものです。ただ ngrok の HTTPS になぜかつながらないので HTTP で対応したコードになっていることにご注意ください。

たとえば Heroku とか別のサーバーの HTTPS は以前の TestHTTP でつながるんですが、なにか ngrok が特殊なんでしょうか。もし、今後 HTTPS のサーバーに何か送る場合は `dhw-pp2-study-04-TestHTTP` で試してみましょう。

前向きに言えば、ここまでで HTTP での送り方と HTTPS での送り方を両方体験できました。

```c
#include <M5Stack.h>

// 以下2つはHTTPSでデータを送るためのライブラリ
// #include <WiFiClientSecure.h>
// #include <ssl_client.h>

// ngrok の HTTPS になぜかつながらないので HTTP で対応
// たとえば Heroku とか別のサーバーの HTTPS は以前の TestHTTP でつながる
#include <WiFi.h>

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

  // TestHTTP からの変更点 1
  // 今回送るホスト名に ngrok のホスト名 (http://なし)を反映
  const char* hostName = "**********.ngrok.io";

  // WiFiClientSecure clientHTTPS;
  // ngrok の HTTPS になぜかつながらないので HTTP で対応
  // 以下 clientHTTPS → clientHTTP
  WiFiClient clientHTTP;

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(10, 10);
  
  M5.Lcd.println("-> send_message");
  M5.Lcd.print("msg: ");
  M5.Lcd.println(msg);
  
  Serial.println("-> send_messagey");
  Serial.print("msg: ");
  Serial.println(msg);
  
  // ngrok の HTTPS になぜかつながらないので HTTP で対応 ポート番号変更 443 → 80
  // if (!clientHTTP.connect(hostName, 443)) {
  if (!clientHTTP.connect(hostName, 80)) {
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
  
  clientHTTP.print(request);
  M5.Lcd.println("clientHTTPS.printed");
  Serial.println("clientHTTPS.printed");
  while (clientHTTP.connected()) {
    String response = clientHTTP.readStringUntil('\n');
    if (response == "\r") {
      break;
    }
  }

  // データ送信完了
  M5.Lcd.println("sended.");
  Serial.println("sended.");

  // サーバーから返答を受け取ったらデータを表示
  String response = clientHTTP.readStringUntil('\n');

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

こちらを `dhw-pp2-study-06-LINEBotMessageHTTP_ngrok` で保存します。

## Wi-Fi 情報を反映して

```c
// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";
```

先ほどと同じように Wi-Fi 情報を反映します。

## 今回送るホスト名に ngrok のホスト名 (http://なし) を反映

```c
  // 今回送るホスト名に ngrok のホスト名 (http://なし)を反映
  const char* hostName = "**********.ngrok.io";
```

こちらには、今回送るホスト名に ngrok のホスト名 (http://なし) を反映します。

## M5Stack に書き込んでみる

ここで、もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

起動すると、まず `Launched` メッセージが ngrok に送られて、さらに LINE BOT にプッシュメッセージとして送られます。

![image](https://i.gyazo.com/03598f71b9402bacd545881ed0225481.jpg)

ボタンを押すと `response Hello M5Stack!(POST)` と返答が返ってきて、メッセージが ngrok に送られて、さらに LINE BOT にプッシュメッセージとして送られます。

![image](https://i.gyazo.com/9df34d328f5d0433888c75ff964c26a5.png)

うまくいけば、LINE BOT でこのように M5Stack で押した内容が表示されて連携できているはずです！

## ngrok 再起動時にやること

ngrok は 2 時間までしか起動しているアドレスのサーバーが使えません。なので、何か実験をするタイミングでサッと使うプロトタイピングに向いています。

もし、ngrok を再起動した場合は、以下の点を対応すれば、作業を再開できます。

- 起動した ngrok のURLを確認して再度メモ
- LINE BOT 側の Webhook URL を今回の ngrok で再度更新
- Arduino のソースコードの今回送るホスト名に ngrok のホスト名 (http://なし) を再度反映

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから「SNS からのアウトプットの事例や世界観を把握する」にすすみましょう。

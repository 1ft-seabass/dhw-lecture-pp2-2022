# M5Stack 側の仕組みをつくる

## 大事なこと

- ここからは M5Stack と Arduino IDE 側の作業です

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/caea033f69762e2626a7bfc1a1425f7c.png)

半角英数字で LINE BOT 側からメッセージを送ってみましょう。Gitpod の LINE BOT サーバー側から MQTT ブローカーへメッセージが送信されます。

![image](https://i.gyazo.com/70ecb50cfaee17985b012d880f16bafb.jpg)

M5Stack 側で MQTT ブローカーからこのメッセージを受け取るので M5Stack にメッセージが届きます。この例は `DHGS Hello!` を送った例です。

![image](https://i.gyazo.com/baa7f80cba18501b3e5518823eae8594.png)

実際の動きはこのようなイメージです。

## ライブラリを Arduino IDE にインストール

### MQTT のやり取りできるライブラリ PubSubClient をインストールする

この作業はインストールなので、一度だけ対応すれば OK です。

以前の温度湿度センサーは GitHub に行かないとライブラリがなかったですが、これは、とてもやりやすく、Arduino のライブラリ管理から検索してインストールできます。

![image](https://i.gyazo.com/0926b156ff8ee87a9a8b626d80bbeb92.png)

ツール > ライブラリを管理 でライブラリマネージャを起動します。

![image](https://i.gyazo.com/6a82b0efb54bc4a78c3faf643f8b8494.png)

`PubSubClient` で検索して、`完全同名` のライブラリを探します。

![image](https://i.gyazo.com/1e50c2b9a016c7a733822ae9d40bc020.png)

マウスを乗せると、バージョンとインストールのボタンが右下に出るので `2.8` のバージョンを指定してインストールボタンをクリックします。

![image](https://i.gyazo.com/5d4ab17b5a0bf7dc323f20ff5efedf3b.png)

インストールできたら、ひょっとすると、リストが一番上に戻ってしまうかもしれませんが、根気よく `PubSubClient` に移動して INSTALLED になっていたら成功です。

### JSON を扱いやすくする ArduinoJson をインストール

この作業はインストールなので、一度だけ対応すれば OK です。

![image](https://i.gyazo.com/0926b156ff8ee87a9a8b626d80bbeb92.png)

ツール > ライブラリを管理 でライブラリマネージャを起動します。

![image](https://i.gyazo.com/b8b223beedfe7b134fe5380e1920e584.png)

`ArduinoJson` で検索して、`完全同名` のライブラリを探します。

マウスを乗せると、バージョンとインストールのボタンが右下に出るので `6.18.5` のバージョンを指定してインストールボタンをクリックします。

![image](https://i.gyazo.com/ec1d0688667c161c941eced03a1bade9.png)

インストールできたら、ひょっとすると、リストが一番上に戻ってしまうかもしれませんが、根気よく `ArduinoJson` に移動して INSTALLED になっていたら成功です。

### ArduinoJson の情報は充実

![image](https://i.gyazo.com/ce0d85128ef8b9dd91734f1176607439.png)

https://arduinojson.org/ というウェブサイトを持っていて情報が充実しています。

![image](https://i.gyazo.com/35ae79ee668a291a65845fb495bb1f32.png)

ソースコードのサンプルもあり、すぐに使いやすくなっています。

## ソースコードを反映＆保存

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-12-TestMQTT` というファイル名で保存します。

```c
#include <M5Stack.h>

// Wi-Fi をつなぐためのライブラリ
// 今回は MQTT のため
#include <WiFiClient.h>
#include <WiFi.h>

// MQTT をつなぎためのライブラリ
// 今回追加インストールする
#include <PubSubClient.h>  // インストールすれば色がつく
// JSON を扱いやすくするライブラリ
#include <ArduinoJson.h> // こちらは色がついてなくてOK

// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";

// 今回使いたい CloudMQTT のブローカーのアドレス
const char *mqttEndpoint = "今回使いたい CloudMQTT のブローカーのアドレス";
// 今回使いたい CloudMQTT のポート
const int mqttPort = 1883;
// 今回使いたい CloudMQTT のユーザー名
const char *mqttUsername = "今回使いたい CloudMQTT のユーザー名";
// 今回使いたい CloudMQTT のパスワード
const char *mqttPassword = "今回使いたい CloudMQTT のパスワード";

// デバイスID
// デバイスIDは機器ごとにユニークにします
// YOURNAME を自分の名前の英数字に変更します
// デバイスIDは同じMQTTブローカー内で重複すると大変なので、後の処理でさらにランダム値を付与してますが、名前を変えるのが確実なので、ちゃんと変更しましょう。
char *deviceID = "M5Stack-YOURNAME";

// MQTT メッセージを LINE BOT に知らせるトピック
// YOURNAME を自分の名前の英数字に変更します
char *pubTopic = "/dhw/pp2/mqtt/YOURNAME/publish";

// MQTT メッセージを LINE BOT から待つトピック
// YOURNAME を自分の名前の英数字に変更します
char *subTopic = "/dhw/pp2/mqtt/YOURNAME/subscribe";

// JSON 送信時に使う buffer
char pubJson[255];

// PubSubClient まわりの準備
WiFiClient httpClient;
PubSubClient mqttClient(httpClient);

void setup() {
  // init lcd, serial, but don't init sd card
  // LCD ディスプレイとシリアルは動かして、SDカードは動かさない設定
  M5.begin(true, false, true);

  // スタート
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(0, 0);
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

  // ちゃんとつながったと分かるために 2 秒待ってから MQTT の処理に行く
  delay(2000);

  // MQTT の接続先設定
  mqttClient.setServer(mqttEndpoint, mqttPort);
  // MQTT のデータを受け取った時（購読時）の動作を設定
  mqttClient.setCallback(mqttCallback);
  // MQTT の接続
  mqttConnect();
}



void mqttConnect() {

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(0, 0);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.setTextSize(2);
  
  // MQTT clientID のランダム化（名称重複対策）
  char clientID[40] = "clientID";
  String rndNum = String(random(0xffffff), HEX);
  String deviceIDRandStr = String(deviceID);
  deviceIDRandStr.concat("-");
  deviceIDRandStr.concat(rndNum);
  deviceIDRandStr.toCharArray(clientID, 40);
  M5.Lcd.println("[MQTT]");
  M5.Lcd.println("");
  M5.Lcd.printf("- clientID ");
  M5.Lcd.println("");
  M5.Lcd.println(clientID);

  // 接続されるまで待ちます
  while (!mqttClient.connected()) {
    if (mqttClient.connect(clientID,mqttUsername,mqttPassword)) {
      Serial.println("Connected.");
      M5.Lcd.println("");
      M5.Lcd.println("- MQTT Connected.");
      
      // subTopic 変数で指定されたトピックに向けてデータを送ります
      int qos = 0;
      mqttClient.subscribe(subTopic, qos);
      Serial.println("Subscribe start.");
      M5.Lcd.println("");
      M5.Lcd.println("- MQTT Subscribe start.");
      M5.Lcd.println(subTopic);

      // 初回データ送信 publish ///////////
      // データ送信のための JSON をつくる
      DynamicJsonDocument doc(1024);
      doc["message"] = "Connected";
      // pubJson という変数に JSON 文字列化されたものが入る
      serializeJson(doc, pubJson);
      // pubTopic 変数で指定されたトピックに向けてデータを送ります
      mqttClient.publish(pubTopic, pubJson);
    } else {
      // MQTT 接続エラーの場合はつながるまで 5 秒ごとに繰り返します
      Serial.print("Failed. Error state=");
      Serial.println(mqttClient.state());
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

// JSON を格納する StaticJsonDocument を準備
StaticJsonDocument<2048> jsonData;

// MQTT のデータを受け取った時（購読時）の動作を設定
void mqttCallback (char* topic, byte* payload, unsigned int length) {
  
  // データ取得
  String str = "";
  Serial.print("Received. topic=");
  Serial.println(topic);
  for (int i = 0; i < length; i++) {
      Serial.print((char)payload[i]);
      str += (char)payload[i];
  }
  Serial.print("\n");

  // 来た文字列を JSON 化して扱いやすくする
  // 変換する対象は jsonData　という変数
  DeserializationError error = deserializeJson(jsonData, str);

  // JSON パースのテスト
  if (error) {
    Serial.print(F("deserializeJson() failed: "));
    Serial.println(error.f_str());
    return;
  }

  // 以下 jsonData 内が JSON として呼び出せる
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(0, 0);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.setTextSize(2);
  M5.Lcd.println("MQTT Subscribed data");

  // データの取り出し
  // https://arduinojson.org/v6/example/parser/
  const char* message = jsonData["message"];

  // データの表示
  M5.Lcd.setCursor(0, 100);
  M5.Lcd.setTextSize(4);
  M5.Lcd.println(message);
  
}

// 常にチェックして切断されたら復帰できるようにする対応
void mqttLoop() {
  if (!mqttClient.connected()) {
      mqttConnect();
  }
  mqttClient.loop();
}
 
void loop() {

  M5.update();

  // 常にチェックして切断されたら復帰できるようにする対応
  mqttLoop();

  if (M5.BtnA.wasReleased()) {
    // A ボタンを押したら JSON 形式のメッセージを飛ばす
    // データ送信のための JSON をつくる
    DynamicJsonDocument doc(1024);
    doc["message"] = "Pushed A";
    // pubJson という変数に JSON 文字列化されたものが入る
    serializeJson(doc, pubJson);
    // pubTopic 変数で指定されたトピックに向けてデータを送ります
    mqttClient.publish(pubTopic, pubJson);
  } else if (M5.BtnB.wasReleased()) {
    // B ボタンを押したら JSON 形式のメッセージを飛ばす
    // データ送信のための JSON をつくる
    DynamicJsonDocument doc(1024);
    doc["message"] = "Pushed B";
    // pubJson という変数に JSON 文字列化されたものが入る
    serializeJson(doc, pubJson);
    // pubTopic 変数で指定されたトピックに向けてデータを送ります
    mqttClient.publish(pubTopic, pubJson);
  } else if (M5.BtnC.wasReleased()) {
    // C ボタンを押したら JSON 形式のメッセージを飛ばす
    // データ送信のための JSON をつくる
    DynamicJsonDocument doc(1024);
    doc["message"] = "Pushed C";
    // pubJson という変数に JSON 文字列化されたものが入る
    serializeJson(doc, pubJson);
    // pubTopic 変数で指定されたトピックに向けてデータを送ります
    mqttClient.publish(pubTopic, pubJson);
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

## MQTT の接続設定を反映

LINE BOT と同じように 今回は私（講師）の方が、CloudMQTT というサービスで、ひとつブローカーを立ち上げているので、そのまま使いましょう。

```c
// 今回使いたい CloudMQTT のブローカーのアドレス
const char *mqttEndpoint = "今回使いたい CloudMQTT のブローカーのアドレス";
// 今回使いたい CloudMQTT のポート
const int mqttPort = 1883;
// 今回使いたい CloudMQTT のユーザー名
const char *mqttUsername = "今回使いたい CloudMQTT のユーザー名";
// 今回使いたい CloudMQTT のパスワード
const char *mqttPassword = "今回使いたい CloudMQTT のパスワード";
```

ここの設定を Slack でお知らせする設定で置き換えましょう。

## LINE BOT でも設定した自分の名前を思い出しましょう

このあと、MQTT の送受信トピックとクライアントIDに自分の名前を反映します。

LINE BOT でも設定した自分の名前を思い出して、`全く同じもの` を使いましょう。

## MQTT の送受信トピックとクライアントIDに自分の名前を反映します

MQTT では「自分がどんな名前の機器か」「どこからデータを待ち」「どこへデータを知らせる」という情報を MQTT ブローカーに知らせてあげると、いろいろなデバイスでデータが飛び交っても、目的のところにちゃんと届くようにしてくれます。しかも双方向。

まるで、住所と表札のようなものです。

とにもかくにも、これが重複してしまうと、違うところにデータが送られてしまったり、自分の名前がほかの人と同じになって混乱してしまいます。

```c
// デバイスID
// デバイスIDは機器ごとにユニークにします
// YOURNAME を自分の名前の英数字に変更します
// デバイスIDは同じMQTTブローカー内で重複すると大変なので、後の処理でさらにランダム値を付与してますが、名前を変えるのが確実なので、ちゃんと変更しましょう。
char *deviceID = "M5Stack-YOURNAME";

// MQTT メッセージを LINE BOT に知らせるトピック
// YOURNAME を自分の名前の英数字に変更します
char *pubTopic = "/dhw/pp2/mqtt/YOURNAME/publish";

// MQTT メッセージを LINE BOT から待つトピック
// YOURNAME を自分の名前の英数字に変更します
char *subTopic = "/dhw/pp2/mqtt/YOURNAME/subscribe";
```

たとえば、hogehoge さんなら YOURNAME を hogehoge に変更します。

```c
// デバイスID
// デバイスIDは機器ごとにユニークにします
// YOURNAME を自分の名前の英数字に変更します
// デバイスIDは同じMQTTブローカー内で重複すると大変なので、後の処理でさらにランダム値を付与してますが、名前を変えるのが確実なので、ちゃんと変更しましょう。
char *deviceID = "M5Stack-hogehoge";

// MQTT メッセージを LINE BOT に知らせるトピック
// YOURNAME を自分の名前の英数字に変更します
char *pubTopic = "/dhw/pp2/mqtt/hogehoge/publish";

// MQTT メッセージを LINE BOT から待つトピック
// YOURNAME を自分の名前の英数字に変更します
char *subTopic = "/dhw/pp2/mqtt/hogehoge/subscribe";
```

ということで、こちらの以下の説明を見つつ変更していきましょう。

- デバイスID ≒ 表札
  - hogehoge さんなら YOURNAME を hogehoge に変更します。- hogehoge さんなら YOURNAME を hogehoge に変更します。
    - つまり、 `"M5Stack-YOURNAME"` を `"M5Stack-hogehoge"` に変更します。
  - MQTTの設定ではクライアントIDという扱いになっていて、ソースコードを追っていくと最終的にクライアントIDとして割り当てられます
    - ここではこのデバイスIDの名称に、後の処理でさらにランダム値を付与してます。`M5Stack-hogehoge` であれば `M5Stack-hogehoge-43ff13` のようなランダム値が加わります。
    - デバイスIDは同じMQTTブローカー内で重複すると大変なので、後の処理でさらにランダム値を付与してますが名前を変えるのが確実なので、ちゃんと変更しましょう。
- MQTT メッセージを知らせるトピック ≒ 手紙を送る住所
  - hogehoge さんなら YOURNAME を hogehoge に変更します。
    - つまり、 `"/dhw/pp2/mqtt/YOURNAME/publish"` を `"/dhw/pp2/mqtt/hogehoge/publish"` に変更します。
- MQTT メッセージを待つトピック ≒ 手紙を受け付ける住所
  - hogehoge さんなら YOURNAME を hogehoge に変更します。
    - つまり、 `"/dhw/pp2/mqtt/YOURNAME/subscribe"` を `"/dhw/pp2/mqtt/hogehoge/subscribe"` に変更します。


## M5Stack に書き込んでみる

そして、もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## LINE BOT の Gitpod がスリープしてたら起こす

![image](https://i.gyazo.com/9bf4ba6a54072e0d6a08ae66a7c9ceca.png)

ここまで作業で時間が経過していると、LINE BOT の Gitpod がスリープしているかもしれません。

そのときは、Open Workspace で起こしてあげましょう。

## 動かしてみる

![image](https://i.gyazo.com/2c6b403d3913cf02138e19cb25a68193.jpg)

書き込みと同時に Connected というメッセージが MQTT ブローカーに送信されます。

実際に M5Stack から接続されているデバイスID（clientID）やデータを待ち受けるトピック subscribe が表示されているので YOURNAME になっていないかや、設定した名前が Gitpod 側で設定した名前と一致しているかを確認しましょう。

![image](https://i.gyazo.com/956797f426bacaf2caa91a2023a3cadb.png)

同時に、Gitpod の LINE BOT サーバー側で MQTT ブローカーから Connected のデータを受け取るので LINE BOT にメッセージが届きます。

同様に ABC のボタンを押すと同じ流れでメッセージが送られます。これが、まず、 MQTT ブローカーが中継した M5Stack と LINE BOT のまずは動作確認です。

ABC のボタンを押す段階では M5Stack の画面に変化はなくて大丈夫です。

![image](https://i.gyazo.com/caea033f69762e2626a7bfc1a1425f7c.png)

つづいて、半角英数字で LINE BOT 側からメッセージを送ってみましょう。Gitpod の LINE BOT サーバー側から MQTT ブローカーへメッセージが送信されます。

![image](https://i.gyazo.com/70ecb50cfaee17985b012d880f16bafb.jpg)

M5Stack 側で MQTT ブローカーからこのメッセージを受け取るので M5Stack にメッセージが届きます。この例は `DHGS Hello!` を送った例です。

### うまく動かない場合は MQTT のトピックを確認してみましょう

うまくいかない場合の一つの原因として MQTT のトピックが M5Stack と Gitpod の LINE BOT サーバとでうまく紐づいていないことがあります。

いま一度、ちゃんと `自分の名前が半角英数字で M5Stack と Gitpod の LINE BOT サーバ両方に反映されているか確認` してみましょう。

これは seigo として設定した例です。

![image](https://i.gyazo.com/66ed32a7bcb98c2037261034bb472b7f.png)

M5Stack でのソースコードではこちらで設定しています。 YOURNAME のままになってませんか？

![image](https://i.gyazo.com/1ef830abebef80d3d8da634704ee6058.png)

Gitpod では clientMQTT.publish に同じ名前でしているか確認しましょう。

![image](https://i.gyazo.com/7e83f720b7781937f8b231acc863da69.png)

同じく clientMQTT.subscribe に同じ名前でしているか確認しましょう。

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから「センサー連携を学ぶ その2」にすすみましょう。
# M5Stack スピーカーを動かす

内蔵されているスピーカーを鳴らします。

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/1dfc9a4a72a623e02dc6a3d008299de8.jpg)

以前のボタンとディスプレイのサンプルに、スピーカーで音を鳴らすのも加えています。より押したときの楽しさが伝わるのと、何が起こったかがメロディによって分かるようにいています。

![image](https://i.gyazo.com/67ee6368fad0b12028ec549849f752a1.jpg)

動画での様子はツイート済みです。→ https://twitter.com/1ft_seabass/status/1448820901340876838

## スピーカーの関数

![image](https://i.gyazo.com/9b87e36a19925728592abbf3822c88fe.png)

スピーカーを動かす関数は以下に説明があります。

- 本家の英語のドキュメント
  - [m5\-docs](https://docs.m5stack.com/en/api/speaker)
- ちょっと以前の情報だけど日本語訳のドキュメント
  - [m5\-docs/speaker\.md at master · m5stack/m5\-docs](https://github.com/m5stack/m5-docs/blob/master/docs/ja/api/speaker.md)

音を止める、ボリュームを設定する、音階で指定の期間鳴らるといったシンプルな構成です。

### 音階で鳴らすときはトーンという値で周波数を指定する

[圧電ブザーを鳴らせてみよう · Arduino docs](https://fabkura.gitbooks.io/arduino-docs/content/chapter7.html)

音階で鳴らすときはトーンという値で周波数を指定しますが、こちらのページ具体的な値の指定の仕方が載っています。Arduinoでの鳴らし方ですが、M5Stackでの tone 値にも同じように適用できます。

![image](https://i.gyazo.com/4d7b1697599d83b73e510bcded042a29.png)

より、ひろーい音階が気になる人は [Play a Melody using the tone\(\) function \| Arduino](https://www.arduino.cc/en/Tutorial/BuiltInExamples/toneMelody) こちらも参考になります。

## ソースコードを反映＆保存

ということで動かしていきましょう。

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-04-02-Speaker` というファイル名で保存します。

```c
#include <M5Stack.h>


void setup() {
  // M5Stack 自体の初期化
  M5.begin(true, false, true);

  // 電源まわりの初期化
  // こうしないとバッテリー状態が更新されない模様
  M5.Power.begin();
  
  M5.Lcd.clear(TFT_BLACK);

  // 線を引いてみた目を区切る ちなみに画面サイズは 320 x 240
  M5.Lcd.drawLine(0,200,320,200,WHITE);
  
  // 下部にボタンナビゲーションをつける
  // 描画順は背景から書いて文字を載せる

  // A ボタンの背景
  M5.Lcd.fillRect(28, 204, 80, 26, BLUE);
  M5.Lcd.drawRect(28, 231, 80, 7, BLUE);

  // B ボタンの背景
  M5.Lcd.fillRect(28 + 80 + 15 , 204, 80, 26, RED);
  M5.Lcd.drawRect(28 + 80 + 15, 231, 80, 7, RED);

  // C ボタンの背景
  M5.Lcd.fillRect(28 + 80 + 15 + 80 + 15 , 204, 75, 26, TFT_GREEN);
  M5.Lcd.drawRect(28 + 80 + 15 + 80 + 15, 231, 75, 7, TFT_GREEN);
  
  // A ボタンの説明
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.setTextSize(2);
  M5.Lcd.setCursor(55, 210);  // 結構合ってるけど何度も書き出して目視で合わせている
  M5.Lcd.print("OK");

  // B ボタンの説明
  M5.Lcd.setCursor(127, 210);
  M5.Lcd.print("CANCEL");

  // C ボタンの説明
  M5.Lcd.setTextColor(BLACK);
  M5.Lcd.setCursor(227, 210);
  M5.Lcd.print("RESET");

  // バッテリーレベル
  M5.Lcd.setTextSize(2);
  M5.Lcd.setCursor(0, 0);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.print("Battery:");
  M5.Lcd.print(M5.Power.getBatteryLevel());

  // テキストを真ん中あたりに出す
  M5.Lcd.setTextSize(4);
  M5.Lcd.setCursor(20, 85);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.print("PUSH BUTTON!");

  // スピーカー音量は 3
  M5.Speaker.setVolume(2);

  // 音を鳴らす ドミソ
  // https://fabkura.gitbooks.io/arduino-docs/content/chapter7.html
  M5.Speaker.tone(523, 200);
  delay(200);
  M5.Speaker.tone(659, 200);
  delay(200);
  M5.Speaker.tone(784, 200);

}

void loop() {
  M5.update();
  
  if (M5.BtnA.wasReleased()) {
    // 音を鳴らす
    M5.Speaker.tone(440, 200);
    delay(200);
    M5.Speaker.tone(440, 200);
    // 上部だけ背景色で塗りつぶす
    M5.Lcd.fillRect(0, 0, 320, 199, BLUE);
    // テキストを真ん中あたりに出す
    M5.Lcd.setTextSize(5);
    M5.Lcd.setCursor(60, 80);
    M5.Lcd.setTextColor(BLACK);
    M5.Lcd.print("OK ^_^/");
    // 下部の選択状態
    M5.Lcd.fillRect(0, 231, 320, 7, BLACK); // 下部を細く黒で塗りつぶし
    M5.Lcd.fillRect(28, 231, 80, 7, WHITE);  // 選択ホワイト
    M5.Lcd.drawRect(28 + 80 + 15, 231, 80, 7, RED);
    M5.Lcd.drawRect(28 + 80 + 15 + 80 + 15, 231, 75, 7, TFT_GREEN);
  } else if (M5.BtnB.wasReleased()) {
    // 音を鳴らす
    M5.Speaker.tone(261, 200);
    delay(200);
    M5.Speaker.tone(261, 200);
    // 上部だけ背景色で塗りつぶす
    M5.Lcd.fillRect(0, 0, 320, 199, RED);
    // テキストを真ん中あたりに出す
    M5.Lcd.setTextSize(5);
    M5.Lcd.setCursor(80, 80);  // わかる。ちょっと中央に寄ってないよね。
    M5.Lcd.setTextColor(BLACK);
    M5.Lcd.print("CANCEL");
    // 下部の選択状態
    M5.Lcd.fillRect(0, 231, 320, 7, BLACK); // 下部を細く黒で塗りつぶし
    M5.Lcd.drawRect(28, 231, 80, 7, BLUE);
    M5.Lcd.fillRect(28 + 80 + 15, 231, 80, 7, WHITE);  // 選択ホワイト
    M5.Lcd.drawRect(28 + 80 + 15 + 80 + 15, 231, 75, 7, TFT_GREEN);
  } else if (M5.BtnC.wasReleased()) {
    // 上部だけ背景色で塗りつぶす
    M5.Lcd.fillRect(0, 0, 320, 199, TFT_GREEN);
    // テキストを真ん中あたりに出す
    M5.Lcd.setTextSize(5);
    M5.Lcd.setCursor(90, 80);
    M5.Lcd.setTextColor(BLACK);
    M5.Lcd.print("RESET");
    // 下部の選択状態
    M5.Lcd.fillRect(0, 231, 320, 7, BLACK); // 下部を細く黒で塗りつぶし
    M5.Lcd.drawRect(28, 231, 80, 7, BLUE);
    M5.Lcd.drawRect(28 + 80 + 15, 231, 80, 7, RED);
    M5.Lcd.fillRect(28 + 80 + 15 + 80 + 15, 231, 75, 7, WHITE);  // 選択ホワイト
    // 2 秒後、表示リセットする /////////////////////////
    delay(2000);
    // 上部だけ背景色で塗りつぶす
    M5.Lcd.fillRect(0, 0, 320, 199, TFT_BLACK);
    // テキストもリセット
    M5.Lcd.setTextSize(4);
    M5.Lcd.setCursor(20, 85);
    M5.Lcd.setTextColor(WHITE);
    M5.Lcd.print("PUSH BUTTON!");
    // 下部もリセット
    M5.Lcd.fillRect(0, 231, 320, 7, BLACK); // 下部を細く黒で塗りつぶし
    M5.Lcd.drawRect(28, 231, 80, 7, BLUE);
    M5.Lcd.drawRect(28 + 80 + 15, 231, 80, 7, RED);
    M5.Lcd.drawRect(28 + 80 + 15 + 80 + 15, 231, 75, 7, WHITE);
  }
}
```

## M5Stack に書き込んでみる

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

![image](https://i.gyazo.com/1dfc9a4a72a623e02dc6a3d008299de8.jpg)

以前のボタンとディスプレイのサンプルの動作に加えて、起動時のドミソ、OK と CANCEL ボタンで効果音が鳴るようにしています。

## LINE BOT 連携のプログラムがどのように動くか

つづいて、 LINE BOT 連携です。LINE BOT 連携のプログラムは以下のように動きます。

![image](https://i.gyazo.com/db6aa153b83623eab272864fb2ae0fb1.jpg)

書き込みと同時に Connected というメッセージが MQTT ブローカーに送信されます。

![image](https://i.gyazo.com/5d8170e76d125d1e4dbd18ccc73e9d1b.png)

LINE BOT から `sound1` ・ `sound2` ・ `sound3` とメッセージを送ります。

![image](https://i.gyazo.com/c7837e3e08d85c3282e760a8fb97076d.jpg)

たとえば、 sound1 というメッセージを LINE BOT から MQTT ブローカーへデータを送ると、M5Stack が MQTT ブローカー からデータを待ち受けているので sound1 というメッセージを受信して、ドミソとメロディが流れます。

![image](https://i.gyazo.com/068c0cfa4f80d2b0916e8d06bbf1c930.png)

実際の動作はこのようなイメージです。

## LINE BOT と連携するソースコードを試す

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-04-03-Speaker-LINEBOT` というファイル名で保存します。

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

  // ちゃんとつながったと分かるために 2 秒待ってから MQTT の処理に行く
  delay(2000);

  // MQTT の接続先設定
  mqttClient.setServer(mqttEndpoint, mqttPort);
  // MQTT のデータを受け取った時（購読時）の動作を設定
  mqttClient.setCallback(mqttCallback);
  // MQTT の接続
  mqttConnect();

  // スピーカー音量
  M5.Speaker.setVolume(2);
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

  // 文字の比較は strcmp が　0 のときで一致している判定ができます
  // https://programming.pc-note.net/c/mojiretsu5.html
  if(strcmp(message, "sound1")==0){
    // 音を鳴らす ドミソ
    M5.Speaker.tone(523, 200);
    delay(200);
    M5.Speaker.tone(659, 200);
    delay(200);
    M5.Speaker.tone(784, 200);
    // データの表示
    M5.Lcd.setCursor(0, 100);
    M5.Lcd.setTextSize(4);
    M5.Lcd.println(message);
  } else if(strcmp(message, "sound2")==0){
    // 音を鳴らす ドド！
    M5.Speaker.tone(523, 100);
    delay(100);
    M5.Speaker.tone(523, 100);
    // データの表示
    M5.Lcd.setCursor(0, 100);
    M5.Lcd.setTextSize(4);
    M5.Lcd.println(message);
  } else if(strcmp(message, "sound3")==0){
    // 音を鳴らす ソミー
    M5.Speaker.tone(784, 200);
    delay(200);
    M5.Speaker.tone(659, 500);
    // データの表示
    M5.Lcd.setCursor(0, 100);
    M5.Lcd.setTextSize(4);
    M5.Lcd.println(message);
  }
  
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

## LINE BOT 連携のプログラムを M5Stack に書き込んでみる

そして、もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## LINE BOT の Gitpod がスリープしてたら起こす

![image](https://i.gyazo.com/9bf4ba6a54072e0d6a08ae66a7c9ceca.png)

ここまで作業で時間が経過していると、LINE BOT の Gitpod がスリープしているかもしれません。

そのときは、Open Workspace で起こしてあげましょう。

## LINE BOT 連携のプログラムを M5Stack に動かしてみる

![image](https://i.gyazo.com/db6aa153b83623eab272864fb2ae0fb1.jpg)

書き込みと同時に Connected というメッセージが MQTT ブローカーに送信されます。

実際に M5Stack から接続されているデバイスID（clientID）やデータを待ち受けるトピック subscribe が表示されているので YOURNAME になっていないかや、設定した名前が Gitpod 側で設定した名前と一致しているかを確認しましょう。

![image](https://i.gyazo.com/5d8170e76d125d1e4dbd18ccc73e9d1b.png)

LINE BOT から `sound1` ・ `sound2` ・ `sound3` とメッセージしてみましょう。

![image](https://i.gyazo.com/c7837e3e08d85c3282e760a8fb97076d.jpg)

たとえば、 sound1 というメッセージを LINE BOT から MQTT ブローカーへデータを送ると、M5Stack が MQTT ブローカー からデータを待ち受けているので sound1 というメッセージを受信して、ドミソとメロディが流るので試してみましょう。

# 次にすすみましょう

左のナビゲーションから「動きセンサー」にすすみましょう。
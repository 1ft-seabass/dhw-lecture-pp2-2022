# 動きセンサー（加速度・ジャイロ・磁気）

M5Stack Gray の内蔵センサーとして動きセンサー（加速度・ジャイロ・磁気）があります。

> ![image](https://static-cdn.m5stack.com/resource/docs/static/assets/img/product_pics/core/core_mpu6886_bmm150_axis.webp)

詳しくは [M5Stack Grayの仕様](https://docs.m5stack.com/en/core/gray) に書かれていますが、引用したこちらの図の通り、地球の磁気で地面の方向が分かり、加速度・ジャイロで M5Stack Gray 自体の回転する動きを検出できます。

![image](https://i.gyazo.com/51d40e9c3fb28753ea88364fd477c1c0.jpg)

[M5Stack/IMU\.ino at master · m5stack/M5Stack](https://github.com/m5stack/M5Stack/blob/master/examples/Basics/IMU/IMU.ino)

こちらの本家のサンプルを動かすとクルクル回転させると値の様子を体感することができます。

ただ yaw が回り続けるという難しいところがあり。こちらの記事で解決できるようです。

[M5Stackの回転データが勝手に回り続けることになる問題の対策 \- Qiita](https://qiita.com/foka22ok/items/53d5271a21313e9ddcbd)

gyroXYZ accXYZ は素のセンサーの値なので慣れないと扱いにくいです、直感的なのは pitch roll yaw です。

![Flight dynamics with text \- ローリング \- Wikipedia](https://upload.wikimedia.org/wikipedia/commons/thumb/5/54/Flight_dynamics_with_text.png/1280px-Flight_dynamics_with_text.png)

pitch roll yaw　は、こちらの記事が分かりやすいです。 → [ローリング \- Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%AD%E3%83%BC%E3%83%AA%E3%83%B3%E3%82%B0)

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/be07e958f93633c9b589c9f27e79e382.jpg)

起動すると、床に置けば水平状態の `(-_-)` という顔をします。

![image](https://i.gyazo.com/2d8a6d04be1e34f78d761b2b897e99d6.jpg)

正面から向かって左方向にパタンとたおすと、顔文字が変わり pitch LEFT の文字が出ます。

![image](https://i.gyazo.com/542b497cb0a17fca6a863e8deae47087.jpg)

正面から向かって右方向にパタンとたおすと、顔文字が変わり pitch RIGHT の文字が出ます。

## ソースコードを反映＆保存

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-15-TestIMU` というファイル名で保存します。

```c
#define M5STACK_MPU6886
#include <M5Stack.h>

// それぞれの値の初期定義
// 加速度
float accX = 0.0F;
float accY = 0.0F;
float accZ = 0.0F;

// ジャイロ
float gyroX = 0.0F;
float gyroY = 0.0F;
float gyroZ = 0.0F;

// 姿勢 pitch,roll,yaw
float pitch = 0.0F;
float roll  = 0.0F;
float yaw   = 0.0F;

float temp = 0.0F; // この温度は空間の温度というより、機器自体の温度

// 左右どちらに倒れているか
float modePitch = 0;
// 以前の modePitch を記録
float modePitchPrevious = 0; 

void setup(){
  // LCD ディスプレイとシリアルは動かして、SDカードは動かさない設定
  M5.begin(true, false, true);

  // 電源の初期化
  M5.Power.begin();

  // 動きまわりのセンサー初期化
  M5.IMU.Init();

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setTextColor(WHITE);
  M5.Lcd.setTextSize(3);
  M5.Lcd.setCursor(0, 100);
  M5.Lcd.println("Test IMU");

  delay(5000);

  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(120, 100);
  M5.Lcd.println("(-_-)");
}

void loop() {
  
  // センサー取得
  M5.IMU.getGyroData(&gyroX,&gyroY,&gyroZ);  // ジャイロ
  M5.IMU.getAccelData(&accX,&accY,&accZ);  // 加速度
  M5.IMU.getAhrsData(&pitch,&roll,&yaw);  // 姿勢 pitch,roll,yaw
  M5.IMU.getTempData(&temp);
  
  // 状態を常に取得し続けて現在の値を modePitch に分類する
  if( pitch < -50 ){
    // 左に倒す
    modePitch = -1;
  } else if( pitch > 50 ){
    // 右に倒す
    modePitch = 1;
  } else {
    // 机に水平に置かれている
    modePitch = 0;
  }
  
  // 以前の値 modePitchPrevious と現在の値 modePitch を比べて変化があったら表示を変えて、値を記録しなおす
  if( modePitch != modePitchPrevious ){
    // 表示を変える
    if( modePitch == -1 ){
      // 左に倒す
      M5.Lcd.fillScreen(BLACK);
      M5.Lcd.setCursor(120, 100);
      M5.Lcd.println("(0_-)");
      M5.Lcd.setCursor(70, 130);
      M5.Lcd.println("pitch LEFT!");
    } else if( modePitch == 1 ){
      // 右に倒す
      M5.Lcd.fillScreen(BLACK);
      M5.Lcd.setCursor(120, 100);
      M5.Lcd.println("(-_0)");
      M5.Lcd.setCursor(50, 130);
      M5.Lcd.println("pitch RIGHT!");
    } else {
      // 机に水平に置かれている
      M5.Lcd.fillScreen(BLACK);
      M5.Lcd.setCursor(120, 100);
      M5.Lcd.println("(-_-)");
    }
    // 値を記録しなおす
    modePitchPrevious = modePitch;
  }
  
  delay(10);
}
```

## M5Stack に書き込んでみる

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

![image](https://i.gyazo.com/be07e958f93633c9b589c9f27e79e382.jpg)

まず起動したらテーブルの上や床に水平に置いてください。

Test IMU というタイトルが出て 5 秒後くらいに水平状態の `(-_-)` という顔をします。

![image](https://i.gyazo.com/2d8a6d04be1e34f78d761b2b897e99d6.jpg)

正面から向かって左方向にパタンとたおすと、顔文字が変わり pitch LEFT の文字が出ます。

![image](https://i.gyazo.com/542b497cb0a17fca6a863e8deae47087.jpg)

正面から向かって右方向にパタンとたおすと、顔文字が変わり pitch RIGHT の文字が出ます。

素早く倒すと、途中で軸が大きくブレるので、一瞬だけ横倒したことになる判定が出るかもしれません。

そーっと使えば問題ないですが、荒い操作でもうまく動くようにしたい方はぜひ調整してみてください。

## LINE BOT と連携するソースコードを試す

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。こちらを `dhw-pp2-study-16-IMU-LINE-BOT` というファイル名で保存します。

```c

// 動きセンサー関連を使うライブラリ
#define M5STACK_MPU6886

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

// それぞれの値の初期定義
// 加速度
float accX = 0.0F;
float accY = 0.0F;
float accZ = 0.0F;

// ジャイロ
float gyroX = 0.0F;
float gyroY = 0.0F;
float gyroZ = 0.0F;

// 姿勢 pitch,roll,yaw
float pitch = 0.0F;
float roll  = 0.0F;
float yaw   = 0.0F;

float temp = 0.0F; // この温度は空間の温度というより、機器自体の温度

// 左右どちらに倒れているか
float modePitch = 0;
// 以前の modePitch を記録
float modePitchPrevious = 0; 


void setup() {
  // init lcd, serial, but don't init sd card
  // LCD ディスプレイとシリアルは動かして、SDカードは動かさない設定
  M5.begin(true, false, true);

  // 電源の初期化
  M5.Power.begin();
  
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

  // 動きまわりのセンサー初期化
  M5.IMU.Init();
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

  // 今回はMQTT のデータを受け取った時（購読時）の動作はなし
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

  // センサー取得
  M5.IMU.getGyroData(&gyroX,&gyroY,&gyroZ);  // ジャイロ
  M5.IMU.getAccelData(&accX,&accY,&accZ);  // 加速度
  M5.IMU.getAhrsData(&pitch,&roll,&yaw);  // 姿勢 pitch,roll,yaw
  M5.IMU.getTempData(&temp);

  M5.Lcd.setTextSize(3);
  
  // 状態を常に取得し続けて現在の値を modePitch に分類する
  if( pitch < -50 ){
    // 左に倒す
    modePitch = -1;
  } else if( pitch > 50 ){
    // 右に倒す
    modePitch = 1;
  } else {
    // 机に水平に置かれている
    modePitch = 0;
  }
  
  // 以前の値 modePitchPrevious と現在の値 modePitch を比べて変化があったら表示を変えて、値を記録しなおす
  if( modePitch != modePitchPrevious ){
    // 表示を変える
    if( modePitch == -1 ){
      // 左に倒す
      M5.Lcd.fillScreen(BLACK);
      M5.Lcd.setCursor(120, 100);
      M5.Lcd.println("(0_-)");
      M5.Lcd.setCursor(70, 130);
      M5.Lcd.println("pitch LEFT!");
      // データ送信のための JSON をつくる
      DynamicJsonDocument doc(1024);
      doc["message"] = "pitch LEFT!";
      // pubJson という変数に JSON 文字列化されたものが入る
      serializeJson(doc, pubJson);
      // pubTopic 変数で指定されたトピックに向けてデータを送ります
      mqttClient.publish(pubTopic, pubJson);
    } else if( modePitch == 1 ){
      // 右に倒す
      M5.Lcd.fillScreen(BLACK);
      M5.Lcd.setCursor(120, 100);
      M5.Lcd.println("(-_0)");
      M5.Lcd.setCursor(50, 130);
      M5.Lcd.println("pitch RIGHT!");
      // データ送信のための JSON をつくる
      DynamicJsonDocument doc(1024);
      doc["message"] = "pitch RIGHT!";
      // pubJson という変数に JSON 文字列化されたものが入る
      serializeJson(doc, pubJson);
      // pubTopic 変数で指定されたトピックに向けてデータを送ります
      mqttClient.publish(pubTopic, pubJson);
    } else {
      // 机に水平に置かれている
      M5.Lcd.fillScreen(BLACK);
      M5.Lcd.setCursor(120, 100);
      M5.Lcd.println("(-_-)");
      // データ送信のための JSON をつくる
      DynamicJsonDocument doc(1024);
      doc["message"] = "(-_-)";
      // pubJson という変数に JSON 文字列化されたものが入る
      serializeJson(doc, pubJson);
      // pubTopic 変数で指定されたトピックに向けてデータを送ります
      mqttClient.publish(pubTopic, pubJson);
    }
    // 値を記録しなおす
    modePitchPrevious = modePitch;
  }

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

  // IMU センサーに合わせて待ち時間を少し加えた
  delay(10);
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

まず起動したらテーブルの上や床に水平に置いてください。

![image](https://i.gyazo.com/487cb8afe544cf2cb56ff3228d2fc56e.png)

起動すると Connected というメッセージが M5Stack から MQTT ブローカーに送信され、LINE BOT がキャッチして、そのメッセージを表示します。

![image](https://i.gyazo.com/2d8a6d04be1e34f78d761b2b897e99d6.jpg)

正面から向かって左方向にパタンとたおすと、顔文字が変わり pitch LEFT の文字が出ます。

![image](https://i.gyazo.com/542b497cb0a17fca6a863e8deae47087.jpg)

正面から向かって右方向にパタンとたおすと、顔文字が変わり pitch RIGHT の文字が出ます。

![image](https://i.gyazo.com/b7493cbbd3b68d1d84feb97e7785cc36.png)

M5Stack から MQTT ブローカーにメッセージが送信され、LINE BOT がキャッチして、そのメッセージを表示します。

# 次にすすみましょう

左のナビゲーションから「さまざまな連携例」にすすみましょう。
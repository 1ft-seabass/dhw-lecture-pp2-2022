# Wi-Fi をつないでみる

## 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/255d8e3093f898093f0f8000b06b9182.png)

Wi-Fi リストで、つなげたい Wi-FI が存在していることが分かったので、つないでみます。IoTの第一歩です。

## ソースコードを反映

Arduino IDE で新規ファイルを作成し、以下のコードをコピーアンドペーストします。

```c
#include <WiFiClient.h>
#include <M5Stack.h>
#include <WiFi.h>
 
// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";
 
////////////////////////////////////////////////////////////////////////////////
   
void setup() {
    // init lcd, serial, but don't init sd card
    // LCD ディスプレイとシリアルは動かして、SDカードは動かさない設定
    M5.begin(true, false, true);
 
    // スタート
    M5.Lcd.fillScreen(BLACK);
    M5.Lcd.setCursor(10, 10);
    M5.Lcd.setTextColor(WHITE);
    M5.Lcd.setTextSize(3);

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
    M5.Lcd.setTextSize(3);

    // Arduino のシリアルモニタ・M5Stack LCDディスプレイ両方にメッセージを出す
    // 前のメッセージが print で改行入っていないので println で一つ入れる
    Serial.println("");  // Arduino のシリアルモニタにメッセージを出し改行が最後に入る
    M5.Lcd.println("");  // M5Stack LCDディスプレイにメッセージを出す改行が最後に入る（英語のみ） 
    
    // Arduino のシリアルモニタ・M5Stack LCDディスプレイ両方にメッセージを出す
    Serial.println("WiFi Connected.");  // Arduino のシリアルモニタにメッセージを出す
    M5.Lcd.println("WiFi Connected.");  // M5Stack LCDディスプレイにメッセージを出す（英語のみ）
   
}

void loop() {
  M5.update();

  // まだ何かを待つような作業はしてないです
}
```

こちらを `dhw-pp2-study-03-TestWiFi` で保存します。

## Wi-Fi 情報を反映して、また保存

```c
// Wi-FiのSSID
char *ssid = "Wi-FiのSSID";
// Wi-Fiのパスワード
char *password = "Wi-Fiのパスワード";
```

Wi-Fi 情報を反映します。

- `char *ssid = "Wi-FiのSSID";`
  - ダブルクォーテーションで囲まれている `Wi-FiのSSID` の部分をつなぎたい Wi-Fi の SSID に書き換えます
- `char *password = "Wi-Fiのパスワード";`
  - ダブルクォーテーションで囲まれている `Wi-Fiのパスワード` の部分をつなぎたい Wi-Fi のパスワードに書き換えます

もう一度保存します。（大事）

![image](https://i.gyazo.com/45b0fd6ce672dc9a0055d45aa290e235.png)

M5Stack に書き込んでみましょう。

## 動かしてみる

![image](https://i.gyazo.com/c272c1e6a5ba0025120ba324e25b10b3.jpg)

Connected. となれば成功です。

![image](https://i.gyazo.com/77ff933c65a4b9473b00fcc03ac33466.jpg)

START . . . . . . . . と、なっている場合は、Wi-Fi がつながらないので、ずっと探している状態です。

以下を検討・確認してみましょう。

- プログラムに設定している接続したい Wi-Fi の SSID ・パスワードが合っているか確認
- 接続したい Wi-Fi 側の設定が、無線帯域 2.4GHz 帯、暗号化方式は WPA2/PSK の推奨設定かを確認
  - もし違っていたら、設定を変更できるか検討
  - もし変更が難しい場合は、スマートフォンのテザリング機能でつなげられないかを検討

# 次にすすみましょう

左のナビゲーションから「サーバーにメッセージを送る」にすすみましょう。
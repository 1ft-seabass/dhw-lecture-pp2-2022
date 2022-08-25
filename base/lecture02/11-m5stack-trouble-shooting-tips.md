# M5Stack トラブルシューティング TIPS

ここでは、セットアップ周辺でSlack などのやり取りで出てきたトラブルシューティング TIPSをまとめています。

## コンパイルしようとすると「使用できない名前のライブラリは無視します」と言われる

（小林さん遭遇）

![image](https://i.gyazo.com/5077677cee5f067946716de80392e1f0.png)

原因：以前インストールした M5Stack ライブラリと重複していると予想

対処：ライブラリの重複が怪しかったので、Mac内で「M5Stack」関係のデータ全て消してもう一度ライブラリ入れてみた（小林さん対応）

## Mac Big Sur でコンパイルすると ValueError: dlsym(RTLD_DEFAULT, kIOMasterPortDefault): symbol not found エラーが出る

（小林さん遭遇）

原因：どうやらBig Surの仕様のせいらしい

対処：[macOS Big SurでESP32のコンパイルが通らなかった件を解決したメモ \#m5stack \- Qiita](https://qiita.com/n0bisuke/items/69366541f4372cb49463)（小林さん対応）

## no protocol というエラーが出る

（さわださん遭遇）

![image](https://i.gyazo.com/c4acda3bd13af44e88a105a06efb6b21.png)

ひとまず、 M5Stack 関連のエラーじゃなさそうなので保留中。

## シリアルモニタでデバッグしようにも文字化けしてしまう

```c
void setup() {
  // init lcd, serial, but don't init sd card
  M5.begin(true, false, true);
  
  /*
    Power chip connected to gpio21, gpio22, I2C device
    Set battery charging voltage and current
    If used battery, please call this function in your project
  */
  M5.Power.begin();

  M5.Lcd.clear(BLACK);
  M5.Lcd.setTextColor(YELLOW);
  M5.Lcd.setTextSize(2);
  M5.Lcd.setCursor(65, 10);
  M5.Lcd.println("Serial example");
  Serial.println("Serial example");
}
```

このような `Serial.println("Serial example");` を実行した時に

![image](https://i.gyazo.com/5a318a7c28f406373fce40afcebd56f0.png)

文字化けしてしまう。（さわださん遭遇）

![image](https://i.gyazo.com/c1ae6857179993d8e204a2e82972eab1.png)

シリアルモニタ右下を 115200 bps に設定すると文字化けせず受信できます。

![image](https://i.gyazo.com/4679862f1ea0dce4753ce20829a05650.png)

うまくいくとこのように表示されます。
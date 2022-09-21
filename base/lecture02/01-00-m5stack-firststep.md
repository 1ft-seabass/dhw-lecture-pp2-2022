# M5Stack の導入

![image](https://i.gyazo.com/2fe8f1e2d461451f6b5212996272c3ee.jpg)

## まずはチェック

チェックがまだな人もいるかもしれないので、念のためチェックしていきましょう。

- [M5Stack セットアップチェック](10-m5stack-check.md)

### 今回のプログラムはどのように動くか

![image](https://i.gyazo.com/8b62e0c3bf9dad23c0e9ca6362aea085.jpg)

無事に書き込まれると `Hello World` の文が表示されます。

### ソースコードを反映

![image](https://i.gyazo.com/a61828fccd84836aabfac60ab103489b.png)

Arduino IDE を起動して、新規ファイルをクリックします。
```c
#include <M5Stack.h>

void setup() {
  M5.begin();

  M5.Lcd.println("Hello World");
}

void loop() {
  
}
```

上記のソースコードをコピーアンドペーストします。実はチェック時のコードと同じです。

![image](https://i.gyazo.com/48fe8f4590406a9eeb1d9cf3be21f12f.png)

コードにこのように色がついていれば OK です。ライブラリが正しくインストールされて効いている状態です。

### ファイルを保存

![image](https://i.gyazo.com/5923c3950bc2a2ef1576dd6d1afe42f4.png)

`dhw-pp2-study-01-HelloWorld` で保存しておきます。

### 確認して書き込み

念のため、もう一度、以下の設定は確認しておきましょう。

- PC から M5Stack 同梱の USB ケーブルで M5Stack につながっている
- ツール > ボード > M5Stack-Core-ESP32 を選択
- ツール > シリアルポート に M5Stack のポートが認識されている
  - Windows 10 の場合 `COM` が頭にあるシリアルポート名が表示 例: `COM3`
  - Mac の場合は `/dev/tty.SLAB_` が頭にあるシリアルポート名が表示 例: `/dev/tty.SLAB_USBtoUART`
- `#include <M5Stack.h>` などに色がついている
  - 例: ![image](https://i.gyazo.com/377362c26b20027e2a6fd3d6a6801227.png)

確認できたら、

![image](https://i.gyazo.com/62a4680dee3f56a2ca23fad41e8d28f6.png)

マイコンボードを書き込むボタンをクリックします。

![image](https://i.gyazo.com/bbbda50bc7f265d291cb3a803e7924b1.png)

コンパイルを待ちます。

![image](https://i.gyazo.com/459cb6b9ad74028375743347a5d6a5af.png)

このようなログと共に書き込まれます。

以下がログの全文です。参考までに。

```
最大1310720バイトのフラッシュメモリのうち、スケッチが347205バイト（26%）を使っています。
最大327680バイトのRAMのうち、グローバル変数が17596バイト（5%）を使っていて、ローカル変数で310084バイト使うことができます。
esptool.py v3.0-dev
Serial port COM3
Connecting.....
Chip is ESP32-D0WDQ6-V3 (revision 3)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal is 40MHz
MAC: 08:3a:f2:44:60:74
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 921600
Changed.
Configuring flash size...
Auto-detected Flash size: 16MB
Compressed 8192 bytes to 47...
Writing at 0x0000e000... (100 %)
Wrote 8192 bytes (47 compressed) at 0x0000e000 in 0.0 seconds (effective 10922.6 kbit/s)...
Hash of data verified.
Flash params set to 0x024f
Compressed 17392 bytes to 11186...
Writing at 0x00001000... (100 %)
Wrote 17392 bytes (11186 compressed) at 0x00001000 in 0.2 seconds (effective 909.4 kbit/s)...
Hash of data verified.
Compressed 347328 bytes to 154525...
Writing at 0x00010000... (10 %)
Writing at 0x00014000... (20 %)
Writing at 0x00018000... (30 %)
Writing at 0x0001c000... (40 %)
Writing at 0x00020000... (50 %)
Writing at 0x00024000... (60 %)
Writing at 0x00028000... (70 %)
Writing at 0x0002c000... (80 %)
Writing at 0x00030000... (90 %)
Writing at 0x00034000... (100 %)
Wrote 347328 bytes (154525 compressed) at 0x00010000 in 2.6 seconds (effective 1074.1 kbit/s)...
Hash of data verified.
Compressed 3072 bytes to 128...
Writing at 0x00008000... (100 %)
Wrote 3072 bytes (128 compressed) at 0x00008000 in 0.0 seconds (effective 3510.9 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
```

### 動かしてみる

書き込まれたら M5Stack を確認しましょう。

![image](https://i.gyazo.com/8b62e0c3bf9dad23c0e9ca6362aea085.jpg)

無事に書き込まれると `Hello World` の文が表示されます。

![image](https://i.gyazo.com/a7c051278279d9fb57ca6ce2e10bcb76.jpg)

そう、最小のフォントサイズ 1 だとこんなに小さいんですが、キレイに表示されるのがすごいですね。

もし「もっと文字大きくしたいなー」など思ったなら、自己フィードバックが生まれて何かしたくなってる良い傾向です！

フォントサイズが厳密に気になるひとはこちら → [M5Stack Basic と M5Stack Core2 のデフォルトフォントのサイズステップが分かったメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2021/02/12/m5stack-basic-and-core2-default-fontsize-maybe-7px-knowledge/)

# 次にすすみましょう

左のナビゲーションから「Wi-Fi リスト確認」にすすみましょう。
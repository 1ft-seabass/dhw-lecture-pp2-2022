# M5Stack セットアップチェック

[前回の宿題](../lecture01/99-homework.md) を行ってみてセットアップがうまく行っているか簡単なチェックをしておきましょう。

## 0. 進め方

以下の導入チェックを 1 → 2 → 3 の順に進めましょう。

## 1. M5Stack ボード設定の導入チェック

![image](https://i.gyazo.com/0cdeeff94e075310b5592e849232fbcb.jpg)

まだ M5Stack は PC につなぎません。

![image](https://i.gyazo.com/a61828fccd84836aabfac60ab103489b.png)

Arduino IDE を起動して、新規ファイルをクリックします。

![image](https://i.gyazo.com/2b832c5bec5263ae68159976d21ff47a.png)

ツール > ボード > M5Stack-Core-ESP32 を選択します。

![image](https://i.gyazo.com/e86802c00c19dfc5cc8ebad4e020770c.png)

ツール > シリアルポート が、グレーして選択できない状態で、このあと M5Stack のポートが増えるのを待ちます。

![image](https://i.gyazo.com/903d4cda78f3cf10c84f036cad08fe03.jpg)

M5Stack Gray に付属していた USB ケーブルを用意します。

![image](https://i.gyazo.com/2a86c7f1b721555abfaf0105f161ea9f.jpg)

PC と M5Stack をつなぎます。

![image](https://i.gyazo.com/595100dc326e18141cc74ab745a3475a.png)

Windows 10 の場合、Arduino IDE の ツール > シリアルポート に、`COM3` や `COM1` のような `COM` が頭にあるシリアルポート名が表示されていれば無事 M5Stack 用の USB ドライバがインストールされて M5Stack が認識されています。

![image](https://i.gyazo.com/9336adb21f951ddeb5f706b54b8b3923.png)

Mac の場合は、Arduino IDE の ツール > シリアルポート に `/dev/tty.SLAB_USBtoUART` のような `/dev/tty.SLAB_` が頭にあるシリアルポート名が表示されていれば無事 M5Stack 用の USB ドライバがインストールされて M5Stack が認識されています。（小林さんキャプチャ使用させていただきました！）

## 2. M5Stack ライブラリの導入チェック「コードに色がつくか」

さきほど Arduino IDE を起動して新規ファイルに、以下のソースコードをコピーアンドペーストしてライブラリに色がつくか確認してみましょう。

```c
#include <M5Stack.h>

void setup() {
  M5.begin();

  M5.Lcd.println("Hello World");
}

void loop() {
  
}
```

正常に M5Stack のライブラリがインストールされていれば、以下のように、コードに色がつきます。

![image](https://i.gyazo.com/48fe8f4590406a9eeb1d9cf3be21f12f.png)

## 3. 検証してみる

![image](https://i.gyazo.com/c374df997075f55b8975e136d189b513.png)

`2.` で確認したソースコードを保存して `dhw-pp2-99-check-sample` をつけて保存しましょう。保存しないと検証できないのでご注意ください。

![image](https://i.gyazo.com/f4f722a959ba5fb7abf9c11a5cc2c550.png)

保存できたら検証ボタンをクリックします。

![image](https://i.gyazo.com/d5fdf80f0ed1ff0dfb74df1f41716d8b.png)

しばらく（30秒～1分？）、ソースコードを実際にコンパイルして、うまくいくか検証がなされます。

![image](https://i.gyazo.com/ee6776b0c4700342fb66596074f4d8b0.png)

> 最大1310720バイトのフラッシュメモリのうち、スケッチが347205バイト（26%）を使っています。
> 最大327680バイトのRAMのうち、グローバル変数が17596バイト（5%）を使っていて、ローカル変数で310084バイト使うことができます。

のようなメッセージと共に、「コンパイルが完了しました」と表示されれば、ボード設置とライブラリがうまくインストールされて M5Stack 用のコードがコンパイルされるところまで正常だと確認できます。

これ以降は授業で扱う流れで、実際に上記コードを M5Stack に書き込まれて動作します。お楽しみに！

## もしうまく動いてなさそうって思ったら

![image](https://i.gyazo.com/2b44aa7e35f6c257520989ea7319cd51.png)

Slack でお気軽に聞いてくださいー。
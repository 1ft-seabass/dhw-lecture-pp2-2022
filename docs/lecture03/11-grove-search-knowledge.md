# Grove 情報検索ナレッジ

このページでは、センサーをはじめてつなぐので、ひとつの Grove センサーで、どのように情報をたどって、M5Stack につないでいくかを学んでいきます。

![image](https://i.gyazo.com/9fcf63bc6544379ec184829112da96a4.jpg)

Grove PIR センサーを例にお伝えします。

## センサー名称を把握

[GROVE \- ミニPIRモーションセンサ \- スイッチサイエンス](https://www.switch-science.com/catalog/3584/)

![image](https://i.gyazo.com/b89c828eb16351b4d1ba1f9deeede9c2.jpg)

まず、スイッチサイエンスさんの購入したページにアクセスしてセンサー名称を把握しましょう。

## 検索してみる → 今回は Google 検索

![image](https://i.gyazo.com/765e62fed3f486423184b79122b3b914.png)

名称で `ミニPIRモーションセンサ Wiki` というキーワードで出てきた [Google 検索結果](https://www.google.com/search?q=GROVE+-+%E3%83%9F%E3%83%8BPIR%E3%83%A2%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%BB%E3%83%B3%E3%82%B5+Wiki) です。

このように、うまく検索出来れば Seeed Studio 社の日本語の Wiki ページにたどり着きます。

その他にも、

- スイッチサイエンスのページ自体に Wiki のページがリンクされている
  - 英語 Wiki の場合が多いがなんとかなる
- スイッチサイエンスのページから Seeed Studio の商品ページをたどって、ほぼほぼリンクされている使い方 Wiki ページに行く
  - 例 : [Grove \- mini PIR motion sensor \- Seeed Studio](https://www.seeedstudio.com/Grove-mini-PIR-motion-sensor-p-2930.html)
  - だいたい、使い方 Wiki があるって、すごいと思う
- Seeed Studio の商品ページを検索して、上記と同じく、ほぼほぼリンクされている使い方 Wiki ページに行く
  - 英語ページ www.seeedstudio.com は情報がある。使い方リンクが英語。
    - 例 : https://www.seeedstudio.com/Grove-PIR-Motion-Sensor.html
  - うまくいくと日本語ページもあります。メリットは使い方リンクが日本語になりやすい。
    - 例 : https://jp.seeedstudio.com/Grove-mini-PIR-motion-sensor-p-2930.html

といった方法があります。今あげた方法を駆使すればだいたい見つけられます。

## 使い方 Wiki を参考にする

![image](https://i.gyazo.com/e38f0a9afb71138548a31180df42d1a9.jpg)

今回はこちらのページでした。 → [Grove PIRモーションセンサー \- Seeedウィキ（日本語版）](https://wiki.seeedstudio.com/jp/Grove-PIR_Motion_Sensor/)

![image](https://i.gyazo.com/d60ab7ea5718517888f331672fa69e83.png)

右の目次から Arduino の情報を探します。

![image](https://i.gyazo.com/2f49b7e640bfc5a6e4b8355f50938ac4.jpg)

ハードウェアから、デジタル・アナログ・I2Cといったつなぎ方をざっくり把握します。M5Stack では、後ほど紹介する裏面にあるピン情報と照らし合わせて、差し込むピンを決めます。

今回はデジタル入出力のつなぎ方と分かりました。

![image](https://i.gyazo.com/de9436679867cef5fc4360db343dcb4c.png)

ソフトウェアから、実際に Arduino のソースコードがあります。これソースは M5Stack にかなりの部分、そのまま移植できるので効果が大きいです。私自身も、Grove で動かすときに毎回助かっています。

## M5Stack に得た情報を置き換えていく

![image](https://i.gyazo.com/5f10e3f3b044561349b15715894af522.png)

Seeed Studio 社のソースコードは Arduino UNO で動かすことを想定したものなので、M5Stack に合わせて探っていきます。

## Grove コネクタ付近の刻印を見てつなぐ先をイメージする

Grove は、赤・黒・黄・白の4本の Grove ケーブルで、たくさんの複雑なセンサーの接続（デジタル・アナログ・I2C・SPI・UARTなど）をうまくまとめています。

![image](https://i.gyazo.com/89e6b7f598ee0c50beed6178c46c747d.jpg)

このように、各センサーの Grove コネクタの刻印をみてみると、電流を供給する VCC + と GND - があり、NC は No Connect つながない。SIG はシグナル、今回の PIR センサーの場合はデジタルの ON OFF の信号が通ります。

![image](https://i.gyazo.com/9bca8f96f96788acb170a5dd2080827f.jpg)

Grove ケーブルを挿しこむと、赤・黒・黄・白の各カラーが対応しててわかりやすくなり、つなぐときの意識をサポートしてくれます。

このあたり、ブレッドボードやはんだ付けで回路をつくるばあいは、回路全体の電流の流れからつなぐ場所を意識することを考えると、かなり対応しやすいですね。

## VCC GND に挿すピンを探っていきます

VCC GND に挿すピンを探っていきます。

![image](https://i.gyazo.com/7afcd3777488c6a4783fb3a039be2675.jpg)

M5Stack の場合、ピン番号が丁寧に書かれているので、追いやすいです。Grove の場合は、左側のピン穴に挿すものを使うことが多いので、こちらに注目します。

- 赤ケーブル
  - `5V`
- 黒ケーブル
  - G

まず、電流を供給する VCC + と GND - はこうなることが多いです。

まれに 3V 供給のセンサーの場合は赤ケーブルの 5V のところを `3V3` に挿します。

[Grove \- mini PIR motion sensor \- Seeed Studio](https://www.seeedstudio.com/Grove-mini-PIR-motion-sensor-p-2930.html)

の場合は、

![image](https://i.gyazo.com/dcbd1d752ea61aea1a724a27975ae429.png)

Techinical details の Supply Voltage が 3.3 - 5V なのでどちらでも行けますね。

## SIG のつなぎ方を探っていく

![image](https://i.gyazo.com/9bca8f96f96788acb170a5dd2080827f.jpg)

電流の供給が M5Stack のピンに置き換えられたので、残りの NC と SIG に注目しましょう。ケーブルは、白と黄です。

今回、白ケーブルにあたる、NC は No Connect つなぎません。

黄ケーブルを見ていきます。SIG はシグナル、今回の PIR センサーの場合はデジタルの ON OFF の信号が通ります。

![image](https://i.gyazo.com/f88e31194b50d59bc157774f6416a499.png)

こういった情報も、[Grove PIRモーションセンサー \- Seeedウィキ（日本語版）](https://wiki.seeedstudio.com/jp/Grove-PIR_Motion_Sensor/) に書かれているので確認しながら進めましょう。

![image](https://i.gyazo.com/7afcd3777488c6a4783fb3a039be2675.jpg)

今回は D2 （デジタル入力）なので、M5Stack の場合は、よく 2 か 5 を使います。

このあたりの置き換えのナレッジは私は以下を参考にしてます。

- [m5\-docs/gpio\.md at master · m5stack/m5\-docs](https://github.com/m5stack/m5-docs/blob/master/docs/ja/api/gpio.md)
  - もう少し深く知りたいとき
- [M5Stackでできること 〜使用可能なデジタル入力端子 \- MSR合同会社](https://msr-r.net/m5stack-gpio/)
  - デジタル入出力がどこで使えるか迷ったとき
- [らびやんさんはTwitterを使っています 「M5Stack GPIO表の記述の誤り…12,13,15はLCDでは使ってない。\(14はLCDのCSで使用\) あとG16,17はUARTのほかPSRAMで使用していると追記した方が良さそうな…。 https://t\.co/2IY5JeAoe2」 / Twitter](https://twitter.com/lovyan03/status/1214004705413627910)
  - 進化し続ける M5STack ゆえに情報が追い付いてない悩ましさもあるが、M5Stack 好きな人たちが追っていて楽しい

### 他のつなぎ方にも対応する

デジタルだけでなく、今後、Grove センサーを購入したとして、アナログ・I2C・SPI・UARTなどつなぎ方にも対応するときに、私がどうしているかをシェアします。

私の場合、以下のドキュメントの、

[M5\-Schematic/M5\-Core\-Schematic\(20171206\)\.pdf at master · m5stack/M5\-Schematic](https://github.com/m5stack/M5-Schematic/blob/master/Core/Basic/M5-Core-Schematic(20171206).pdf)

![image](https://i.gyazo.com/174c24dad65b7cea614f3e2d4948d111.png)

の図は結構分かりやすいので、ざっくりこのように読み取りながら使ってます。

- デジタル入出力
  - Arduino 上で D ではじまるもの 例：D2
  - M5Stack裏面の番号
    - 2 , 5
- アナログ入力
  - Arduino 上で A ではじまるもの 例：A0
  - M5Stack裏面の番号
    - 35 , 36
- アナログ出力
  - Arduino 上で A ではじまるもの 例：A0
  - M5Stack裏面の番号
    - 25 , 26
- UART
  - Arduino 上でつなぎ方を RX TX で表現される
  - M5Stack裏面の番号
    - 3 , 1 , 16 , 17
  - 言葉そのものの参考資料（分からなくても使えます）
    - [UARTとは？通信の仕組みを解説 – 衛星ラボ](https://eiseilab.com/uart/)
- I2C
  - Arduino 上でつなぎ方を SDA(データ線) SCL(クロック線) で表現される
  - M5Stack Gray には Grove I2C ポートがあるのでそれを使うのがおススメ
  - 言葉そのものの参考資料（分からなくても使えます）
    - [I2Cの仕組みと使い方 – 衛星ラボ](https://eiseilab.com/i2c/)
- SPI
  - Arduino 上でつなぎ方を MOSI MISO SCK SS で表現される
  - 各センサーのつなぎ方をうまく置き換えて使う。いろいろある。
  - 言葉そのものの参考資料（分からなくても使えます）
    - [SPIの仕組みと使い方 – 衛星ラボ](https://eiseilab.com/spi/)

今回の講義前半では、私の方が上記を整えて、つなぎかたを反映していますが、もしみなさんが自分で Grove センサーを購入してつなぐ場合には、上記のナレッジも意識しつつ、試行錯誤してつなげてみましょう。

![image](https://i.gyazo.com/87f24c002339296dc2cc1689299c4b78.png)

とはいえ、ブレッドボードやはんだ付けでセンサーや回路を一から組んでいくときと比べれば、Grove が応援してくれるところは多いです！うまく案内してくれるはずです！
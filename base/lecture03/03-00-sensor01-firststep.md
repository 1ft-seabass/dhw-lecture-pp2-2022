# M5Stack のセンサー連携を学ぶ その1

![image](https://i.gyazo.com/2fe8f1e2d461451f6b5212996272c3ee.jpg)

## これからセンサーをつなげていきます

## Grove システムがなぜよいか

[Grove \- Seeed Studio Electronics](https://jp.seeedstudio.com/category/Grove-c-1003.html)

![image](https://i.gyazo.com/b359d329236a0414701c4ccd40fa74d5.jpg)

Grove は Seeed Studio 社が開発した Grove ポートと呼ばれる M5Stack などベースシールド側にある入口に差し込むだけで、すぐにセンサーや動作するもの（アクチュエータ）が使える仕組みです。

3つの特徴があります。

### すぐに使える

![image](https://i.gyazo.com/8265ccd6ddf4957580a7f6aef702acfd.jpg)

Grove コネクタと Grove ポートには逆に差さないように出っ張りがあり、挿し方を間違えて壊してしまったり動かないことが避けられています。

後述するモジュール化やオープンソースであることで、つなぎ方や使い方に（あまり）迷わずすぐに使えます。

### モジュール化

光センサーの例です。

![image](https://i.gyazo.com/bba7efd90c24ac240a80295dc22733e4.jpg)

![image](https://i.gyazo.com/cc53b30650e81d15eab4e7260fa64cde.jpg)

Grove の各パーツは、他にはんだづけやブレッドボードで回路を必要がなく、モジュール（装置・機械・システムを構成する、機能的にまとまった部分）としてまとまっています。

![image](https://i.gyazo.com/a354df133058457c43d811da85e1dbc1.jpg)

たとえば、この光センサーも本来であれば、ブレッドボード上でセンサー抵抗とケーブルが必要です。また、電気の流れを深く理解して回路を作る必要があります。

![image](https://i.gyazo.com/aeb145960b5b0a1ee296c2b7f174d2ef.jpg)

もちろん、回路がうまくいっても実際に動かすマイコンに対して、間違えなくつなげる必要があります。Grove はこのあたりの大部分をカバーしてくれます。

### オープンソース

![image](https://i.gyazo.com/c22ea19f0e1c9ab4aa3e328876a35f42.jpg)

このスイッチサイエンスさんの [GROVE \- 光センサ（パネルタイプ） v1\.1](https://www.switch-science.com/catalog/2854/) のページですが、情報がとても充実しています。

[Grove \- Light Sensor \- Seeed Wiki](https://wiki.seeedstudio.com/Grove-Light_Sensor/)

![image](https://i.gyazo.com/889cae183c6a9c42235c466965761b9b.jpg)

それは、Grove の情報がオープンソースだからです。Grove パーツには、このように、Seeed Studio 社の手厚い Wiki のリファレンスがあり、そこを見るだけで Arduino としてのハードウェアのつなぎ方やソフトウェアとしてのプログラムの書き方がすぐに把握することができます。

実は、電子工作で使うセンサーでもネット上にあまり情報がなかったり、回路図や使い方といった情報も見つかりにくいと、とても開発がしにくく、実際に動かしてたその先、発想して、創造する、アウトプットするところに、なかなか辿り着くことができません。

Grove の仕組みは、オープンソースで情報を公開することによって、制作者がすぐに作れるようにサポートして、さらに触れたことのある人がお互いに情報を共有しやすくしています。

## M5Stack でも Grove が使える

![image](https://i.gyazo.com/66c41f307332b60ef4f1a35f9589751a.png)

もちろん、M5Stack でも連携ができます。外装がしっかりした M5Stack に対して、機能がひと固まりで、ケーブルを挿すだけで使える Grove をつかえば、基板がむき出しになる場所がかな少なくなり、今後できたものを見せたり触ってもらうときにも、すぐに行動に移すことができます。

![image](https://i.gyazo.com/3a83ebae6f049cf22288a13b2b939e4d.jpg)

今回の機材リストで購入した GROVE 4ピン ジャンパオスケーブルを使うことで M5Stack のピンが挿せます。

![image](https://i.gyazo.com/a40caafe567107837f463d51077fa6c4.jpg)

また、M5Stack には Grove のポートを備えています。ここは I2C 系のセンサーやアクチュエータが使えますが、I2C 機能をオフにすることで、デジタル入出力・アナログ入力・PWM といったやり取りにも使えます。

I2C でつなぐセンサーは高性能で、少し高価（1500～3000円以上）ではあるので、みなさんのつくりたいものに必要になったら検討してみましょう。授業でも I2C センサーについても、状況に応じてご紹介していく予定です。

# 次にすすみましょう

左のナビゲーションから次の教材にすすみましょう。
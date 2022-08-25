# 3Dプリンタプロトタイピング TIPS

![image](https://i.gyazo.com/2fe8f1e2d461451f6b5212996272c3ee.jpg)

やや泥臭い TIPS をいくつかお伝えします。

## 3D プリンタもプロトタイピングに活用する方法はある

![image](https://i.gyazo.com/b56d2101009011ec03d2e2f94a190d86.jpg)

先週お伝えしましたが、3Dプリンタは、このようなプロセスを踏んで、現実世界にオブジェクトを作りだします。

- 発想
  - 現実世界で活用したいところを見極める
- 採寸・設計
  - 活用したい場に合うボリュームをなるべく正確に把握し設計する
- モデリング
  - CAD ソフトで活用場所で求める形を CAD データで作り出す
- 成型
  - 3D プリンタの性能に合わせて CAD データから出力する
- 判断
  - うまく出力できれば完了
  - 修正するポイントがある場合は現実世界で活用したいところを見極めに戻る

どのパートもなかなかに難易度が高く、慣れるまでは、起動コストがかかりやすいのが難しいところです。

![image](https://i.gyazo.com/477468be7d81767232b5e5f416a193db.jpg)

このまま、プロトタイピングに取り入れると、ここに割くパワーと時間がかかりすぎて滞りやすいです。

ですが、これからご紹介する手法は、上記の流れをうまくコントロールできます。

## 段ボールのような工作のプロトタイプと組み合わせる手法

採寸・設計の部分は、定規などの測り方や目測で徐々に詰めていくものですが、慣れていないと計測ミスが起き、モデリングや成型に影響が出て、やりなおししがちで時間がかかりやすい部分です。

![image](https://i.gyazo.com/75ca5288c1e7750632239d8165aa222d.jpg)

このように、前もって紙にスキャンで写し取ったり、段ボールなどで仮の箱を作ったものを展開して計測いくと、定規などの測り方や目測に頼らず、イメージ通りの形状に辿り着くことができます。

- [Raspberry Pi Zeroの形状を写しとりIllustratorから123D Designに落としこむまでのメモ　前編 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2016/01/09/raspberry-pi-zero-3dprinter-make-stl_1/)
- [Raspberry Pi Zeroの形状を写しとりIllustratorから123D Designに落としこむまでのメモ　後編 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2016/01/10/raspberry-pi-zero-3dprinter-make-stl_2/)

## 形状データを配布しているところで近いオブジェクトからはじめる

もうひとつおススメの方法は、採寸・設計・モデリングをある程度一気に進められる方法として、形状データを配布しているところで近いオブジェクトからはじめる方法があります。

たとえば、M5Stack でも、

- [M5Stackにスタックできるケースを作る – ししかわのマウス研修 Part\.29 \| アールティ　移動型ロボットブログ](https://rt-net.jp/mobility/archives/12211)
- [M5stackの3Dデータ公開 \| 株式会社応用技術研究所](https://hagane-karakuriya.com/2021/03/18/m5stuck%E3%81%AE3d%E3%83%87%E3%83%BC%E3%82%BF%E5%85%AC%E9%96%8B/)
- [M5StackのCADデータを公開しました \| BotaLab](https://botalab.tech/m5stack_cad_data/)

このように、有志の方が配布いただいています。これをうまく使って、CAD ツールで立方体に押し当ててから形に合わせて削除すれば、ピッタリにハマる形状の出来上がりです。

本来であれば、0.3 mm ほど開いた形状を広げないと、3D プリンターの誤差によってハマらないなどありますが、おおまかなものにすぐにたどり着けます。

こういった形状データを探していくことも、まるでソフトウェア開発において、ある程度出来上がっているライブラリを探すような感覚と似たようなアプローチで、プロトタイプを加速することができます。

## LEGO パーツの試行錯誤例

![image](https://i.gyazo.com/f383b4bb9e253fa6eaff9b1342197227.jpg)

私が行った LEGO と電子工作の試行錯誤例があります。

> ![image](https://i.gyazo.com/b8e9640cdcb1832b960945fdb4f69d59.jpg)
>
> [DMM\.makeでlittleBitsとLEGO連携パーツ（フリーCADデータ）を出力したメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2015/11/02/dmm-make-littlebits-lego/)

電子工作のブロックと LEGO ブロックを組わせて形状の試行錯誤がしやすくならないかというところでこのようなことを行いました。

LEGO のようなメジャーなパーツは以下のようなサイトで 3D プリンタ用のデータを見つけることができます。

- [Thingiverse \- Digital Designs for Physical Objects](https://www.thingiverse.com/)
- [無料の3Dモデル \- Free3D\.com](https://free3d.com/ja/)
- [Free 3D Models and Objects Archive\. Download: 3ds , obj , gsm , max models](https://archive3d.net/)

> ![image](https://i.gyazo.com/94d34c00c5561d7b219eb6447fc3630f.jpg)
> 
> - [littleBitsマウントボードと連携するLEGOパーツの試行錯誤メモ 前編 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2015/11/10/littebits-lego-3dprinter-try-and-error-1/)
> - [littleBitsマウントボードと連携するLEGOパーツの試行錯誤メモ 後編 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2015/11/10/littebits-lego-3dprinter-try-and-error-2/)

このように、この制作に関するほとんどの形状データを、うまくみつけてプロトタイピングを加速することができました。

![image](https://i.gyazo.com/6e360f9dfb2bd3747c45ad2d647181b2.jpg)

- その他の文献
  - [Raspberry Pi Zeroケースの設計ミスを改善しフタまで実装するまでのメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2016/02/07/raspberry-pi-zero-3dprinter-make-stl_3/)
  - [東京メイカーさんにダヴィンチ3Dプリンタの出力レクチャーを受けてきたメモ　前編 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2016/02/12/tokyomaker-3dprinter-lecture-1/)
  - [東京メイカーさんにダヴィンチ3Dプリンタの出力レクチャーを受けてきたメモ　後編 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2016/02/23/tokyomaker-3dprinter-lecture-2/)

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから「アウトプットしフィードバックを得る手法」にすすみましょう。


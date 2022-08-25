# 外装や設置のプロトタイプの実践

![image](https://i.gyazo.com/2fe8f1e2d461451f6b5212996272c3ee.jpg)

## LEGO + Grove

レゴブロックにもくっつく Grove 純正の Wrapper というものがあります。

![image](https://i.gyazo.com/e8135d74dfba41775128e0964bf6b6fe.jpg)

![image](https://i.gyazo.com/4bfb62f8276dfa23672d1515b9b7c0d9.jpg)

これにGroveをつけると連結できたりレゴブロックにもくっつし、ネジ止めで木材などに固定できます。

- 参考記事
  - [レゴブロックにもくっつくGrove純正のWrapperがステキだったメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2017/09/14/grove-wrapper-is-cool/)
- 購入先
  - [Grove \- Blue Wrapper 1\*2\(4 PCS pack\) \- Seeed Studio](https://www.seeedstudio.com/Grove-Blue-Wrapper-1-2-4-PCS-pack.html)
  - [Grove \- Wrapper 1 x 2 青（4個パック） \- スイッチサイエンス](https://www.switch-science.com/catalog/6999/)

## Grove の設置に便利な長ーいケーブル

今後、Grove センサーを扱っていくときに、 M5Stack から Grove を遠くに離して設置したいケースもあると思います。

> ![imaege](https://d2air1d4eqhwg2.cloudfront.net/images/5217/500x500/6314d39d-8e6e-4366-aaeb-43ceefdd0a9d.jpg)
> 
> [M5Stack用GROVE互換ケーブル 200 cm（1個入り） \- スイッチサイエンス](https://www.switch-science.com/catalog/5217/)

M5Stack 用の Grove 長い互換ケーブルというのがあるので、覚えておくと良いでしょう。もちろん、離しすぎると、電流が弱くなりセンサーがうまく動かない可能性もありますが、そのあたりは自分で試してみると良いと思います。

こういった延長、単純なデジタル入力センサーであれば、2 m 程度であれば全く問題なかったです（実体験）

そして、色々な長さのケーブルがあるので、シチュエーションに応じて使っていきましょう。

- [M5Stack用GROVE互換ケーブル 20 cm（5個入り） \- スイッチサイエンス](https://www.switch-science.com/catalog/5214/)
- [M5Stack用GROVE互換ケーブル 50 cm（2個入り） \- スイッチサイエンス](https://www.switch-science.com/catalog/5215/)
- [M5Stack用GROVE互換ケーブル 100 cm（1個入り） \- スイッチサイエンス](https://www.switch-science.com/catalog/5216/)
- [M5Stack用GROVE互換ケーブル 200 cm（1個入り） \- スイッチサイエンス](https://www.switch-science.com/catalog/5217/)

### LAN ケーブルで延長するちょっと荒っぽいアイデア

このナレッジは自己責任ではありますが、私が試した手に入りやすい LAN ケーブルを利用して電子工作のケーブルに使ってみたというナレッジです。 

![image](https://i.gyazo.com/e0a417b0084caa939a81cb7b1f0455fa.jpg)

[GroveケーブルをLANコネクタを合体させてGroveでのデジタル入出力がLANケーブルで延長できたメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2019/01/11/grove-cable-meets-lan-cable/)

![image](https://i.gyazo.com/a896ee3cac56af66c6f601b6f1824071.jpg)

[市販のLANコネクタ基板キットにGroveコネクタをくっつけるメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2019/01/29/grove-cable-meets-lan-cable-2/)

こういった、大きく展開できるナレッジを持っておくと、設置だけでなく展示するときも自由に考えられるようになるのでおススメです。

## M5Stack Basic用汎用ケース

タッパーを使ったケースの知見もありますが、ぴったりのサイズで樹脂が加工しやすいM5Stack Basicのためにデザインされた汎用ケースというものもあります。

> ![image](https://d2air1d4eqhwg2.cloudfront.net/images/6732/500x500/b0134df9-1ddd-423e-b227-66c4d15d2405.jpg)
> 
> [M5Stack Basic用汎用ケース FUSION CONTAINER \- スイッチサイエンス](https://www.switch-science.com/catalog/6732/)

ある程度、大きさが似たボリュームで収まるときや、ある程度しっかりした見た目を求めていく場合にはこういう選択肢もありでしょう。

私が使う場合は、似たボリュームのタッパーで一回収める実験をして、目測を整えてから使います。

## グルーガンの活用事例

電子工作でグルーガンを使う例として、いくつかご紹介します。

> ![image](https://i.gyazo.com/68cabedd8ddddb53a7a9b2a1f6a4d4ae.jpg)
>
> [NefryBTのGroveポートに直接つなげるためテープLED電子工作をしてみたメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2017/10/19/nefrybt-grove-port-brushup/)

テープ LED のような長くぶら下げたり、結合部分が負荷で外れやすいものは、グルーガンで固定すると便利です。

> ![image](https://i.gyazo.com/774caf83aec475e071d19204f0a67bfb.jpg)
>
> [Wio Node＋Grove リレー＋USB延長ケーブルでUSBライトをIoT化させるメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2016/12/04/wio-node_grove-relay_usb-cable_iot-hack/)

このような、展示用の配線を補強することにも使えます。

その他にも、良い文献がありました。見ていきましょう。

- [Make: Japan \| ホットグルーの11の応用、裏技、改造方法](https://makezine.jp/blog/2016/07/8-great-hot-glue-tips-tricks-hacks.html)
- [電子工作用ではないけど便利な工具　ホットメルトで部品を固定 \| RaspberryPiクックブック](https://www.denshi.club/parts/2016/09/post-15.html)
- [グルーガンの使い方、選び方、修理【図解】](https://diytools1.com/2016/05/13/post-14695/)

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから「3Dプリンタプロトタイピング TIPS」にすすみましょう。


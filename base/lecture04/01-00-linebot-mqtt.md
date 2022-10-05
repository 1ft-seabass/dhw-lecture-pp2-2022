#  LINE BOT と M5Stack 連携の MQTT による発展を学ぶ

![image](https://i.gyazo.com/2fe8f1e2d461451f6b5212996272c3ee.jpg)

## いままでの LINE BOT の仕組み

![image](https://i.gyazo.com/4bed29421b7aaea13d3caf413b7d95c8.png)

まず、シンプルに Gitpod で LINE BOT のサーバーを立てることで、ユーザーから見た LINE BOT でオウム返しができています。

![image](https://i.gyazo.com/4fc47f8fa3d09bb9e57c199a3eabcc2d.png)

そこに、Gitpod の LINE BOT のサーバーで、M5Stack からメッセージを受け付ける入口も作っているので、M5Stack からメッセージを受け取ると、それがユーザーから見た LINE BOT にメッセージが届いています。

![image](https://i.gyazo.com/825b91311bb72283f3a8ecbff2a0dd29.png)

ただ、HTTP の場合は M5Stack から Gitpod の LINE BOT サーバーは公開されているので連絡は出来るのですが、その反対の流れ、Gitpod の LINE BOT サーバーから自宅のルーターなどを通じて奥のほうにある M5Stack に直接メッセージを届けるのは難しいです。

## MQTT という技術を使うことによって LINE → M5Stack も可能にする

![image](https://i.gyazo.com/85fc418d4f7144df2bf53ec41124ef38.png)

MQTT という技術を使うと「自分がどんな名前の機器か」「どこからデータを待ち」「どこへデータを知らせる」という情報を MQTT ブローカーに知らせてあげると、いろいろなデバイスでデータが飛び交っても、目的のところにちゃんと届くようにしてくれます。

しかも双方向。まるで、住所と表札のようなものです。

つまり、ちゃんと表札と住所を持てば M5Stack → LINE だけでなく LINE → M5Stack も可能になるんです。

## 今回は講師の方が MQTT ブローカーを用意しています

![image](https://i.gyazo.com/8643891ef601e8e7d99f1a7b8bdb21c9.png)

本来であれば MQTT ブローカーという指揮者のような役割のサーバーが必要ですが、今回は私（講師）の方が、CloudMQTT というサービスで、ひとつブローカーを立ち上げているので、そのまま使いましょう。

### 今期終了まで使えます、以後は自分でつくりましょう

![image](https://i.gyazo.com/d5d28d3e431e48c644bafdeff11f650f.png)

本カリキュラムが終わる今期終了まで、使えるようにしているので、制作のためにお使いください。

もし、自分でつくって使いたくなったら、私のブログ記事を参考に作ってみましょう。

- [CloudMQTT で最小プラン Humble Hedgehog でインスタンスを作り最低限の設定をするメモ 2021年9月版 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2021/09/23/cloudmqtt-setting-minimum-plan-202109/)

# 次にすすみましょう

左のナビゲーションから次の教材にすすみましょう。
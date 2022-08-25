# LINE BOT と MQTT の仕組みを起動する

## 大事なこと

- ここからは Gitpod の LINE BOT 側の作業です
- Chrome ブラウザのなるべく最新版でアクセスしましょう

## 使う Git リポジトリ

すでに、今回の LINE BOT をはじめる環境は作ってあります。

https://github.com/1ft-seabass/dhw-pp2-2021-m5stack-linebot-mqtt

![image](https://i.gyazo.com/3828efa3f5163641d47e6e61b4ba4bf1.png)

こちらを使います。前回の授業で使ったものとは違うのでご注意ください。

## Gitpod で構築開始

Gitpod は GitHub 公開されたリポジトリであれば、すぐに `https://www.gitpod.io/#` のあとに GitHub リポジトリのある URL をつけた状態でアクセスすると構築できます。

ということで、以下にアクセスします。

https://www.gitpod.io/#https://github.com/1ft-seabass/dhw-pp2-2021-m5stack-linebot-mqtt

構築がはじまります。

![image](https://i.gyazo.com/668504b5f7cf347e325cd4c51ef7c403.png)

すすんでいきます

![image](https://i.gyazo.com/5acec3af8060d1e00fe3df60209d47da.png)

ここまですすむとだいぶ良い感じです。

![image](https://i.gyazo.com/0b1c1b37a07db7ebb55c225516813080.png)

しばらく待つと、このようにブラウザ上で Visual Studio Code が起動し、さきほどの GitHub の Git リポジトリにあるファイルが移植されています。

このあとは、このブラウザ上で Visual Studio Code で作業を進めていきましょう。

## npm install する

`package.json` に、すでに今回使う `express` や `@line/bot-sdk` ライブラリについて登録されています。

また、今回使う `mqtt` という MQTT のライブラリも登録されています。

`npm i` することでインストールを開始します。

```bash
npm i
```

コマンドでインストールします。

## 作成したBOTのチャンネルシークレットとチャンネルアクセストークンを反映

```js
// 作成したBOTのチャンネルシークレットとチャンネルアクセストークン
const config = {
  channelSecret: '作成したBOTのチャンネルシークレット',
  channelAccessToken: '作成したBOTのチャンネルアクセストークン'
};
```

[LINE BOT 作成手順](../lecture02/12-line-bot-create.md) で、作成したBOTのチャンネルシークレットとチャンネルアクセストークンを、それぞれ反映します。

わりと編集中にシングルクォーテーション消してしまったり、channelSecretとchannelAccessTokenを、逆に書いたりしてハマるので気をつけましょう。

## M5Stack から来るプッシュ通知の送り先のユーザーIDを反映

まず、プッシュ通知の送り先であるこの BOT のユーザーIDを LINE Developers から取得します。

https://developers.line.biz/ja/

こちらからログインしましょう。

![image](https://i.gyazo.com/b4cff116ffa19c5ed6b6b2c98e15cedb.png)

今回使っている BOT のチャネル設定に移動します。

![image](https://i.gyazo.com/1e959b391cb50becbdff3fd3ca39b3e2.png)

下にスクロールしていくと `あなたのユーザーID` という項目があるので、これをメモしておきます。

```js
// プッシュメッセージで受け取る宛先となる作成したBOTのユーザーID'
const userId = '作成したBOTのユーザーID';
```

`作成したBOTのユーザーID` の部分を先ほどメモしたユーザーIDで書き換えます。

ここまで設定出来たらファイルを保存します。

## MQTT の接続設定

冒頭でもお知らせしたとおり、本来であれば MQTT ブローカーという指揮者のような役割のサーバーが必要ですが、今回は私（講師）の方が、CloudMQTT というサービスで、ひとつブローカーを立ち上げているので、そのまま使いましょう。

```c
// 今回使う CloudMQTT のブローカーアドレス
const clientMQTTAddress = 'CloudMQTT のブローカーアドレス'

// 今回使う CloudMQTT のポート番号
const clientMQTTPort = 1883;

// 今回使う CloudMQTT のユーザー名
const clientMQTTUserName = 'CloudMQTT のユーザー名';

// 今回使う CloudMQTT のパスワード
const clientMQTTPassword = 'CloudMQTT のパスワード';
```

ここの設定を Slack でお知らせする設定で置き換えましょう。

![image](https://i.gyazo.com/c43f8c427941e95754eb23b151d168ce.png)

このような関係性で動作しています。

### 自分の名前の半角英数字で決める

このあと、M5Stack と LINE BOT がやりとりするトピック名などで、半角英数字の自分の名前を決めておきましょう。

### M5Stack からのデータ受信を待つ MQTT トピックに自分の名前を入れる

```js
clientMQTT.on('connect', function () {
  // 接続時にログ
  console.log('MQTT Connected!');
  // M5Stack からのデータ受信を待ちます
  // M5Stack の A B C ボタンを押したときに送られてきます
  clientMQTT.subscribe('/dhw/pp2/mqtt/YOURNAME/publish');
```

こちらのコードに M5Stack からのデータ受信を待つ MQTT トピック `'/dhw/pp2/mqtt/YOURNAME/publish'` の `YOURNAME` のところに自分の名前を入れます。

半角英数字です。自分の名前は `hogehoge` の場合は `'/dhw/pp2/mqtt/hogehoge/publish'` となります。

![image](https://i.gyazo.com/aa6ac3144d65af9e4e9ab3f09df70242.png)

M5Stack から LINE BOT へやり取りするイメージです。

### M5Stack にデータを送る MQTT トピックに自分の名前を入れる

```js
  // M5Stack が待っているトピックにデータを送る
  clientMQTT.publish('/dhw/pp2/mqtt/YOURNAME/subscribe', jsonString);
```

こちらのコードに、M5Stack にデータを送る MQTT トピック `'/dhw/pp2/mqtt/YOURNAME/subscribe'` の `YOURNAME` に自分の名前を入れます。

半角英数字です。自分の名前は `hogehoge` の場合は `'/dhw/pp2/mqtt/hogehoge/subscribe'` となります。

![image](https://i.gyazo.com/3b5fdcfd4083513c0e07f82e9cf4ca9a.png)

LINE BOT から M5Stack へやり取りするイメージです。

### 11月19日（金）まで使えます、以後は自分でつくりましょう

本カリキュラムが終わる 11月19日（金） ごろまで、使えるようにしているので、制作のためにお使いください。

もし、自分でつくって使いたくなったら、私のブログ記事を参考に作ってみましょう。

- [CloudMQTT で最小プラン Humble Hedgehog でインスタンスを作り最低限の設定をするメモ 2021年9月版 – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2021/09/23/cloudmqtt-setting-minimum-plan-202109/)

## app.js を起動してみる

これで準備完了です。

```bash
node app.js
```

こちらのコマンドで app.js を起動してみます。

![image](https://i.gyazo.com/3cd262e2aee82f79b83949de4972a6e8.png)

このように起動します。

![image](https://i.gyazo.com/fb93043a1f85b294ccaa5db911db6165.png)

すぐに新しいブラウザがもうひとつ開いて、今回起動したサーバーのルートアドレスが表示されます。

### 今回のサーバー URL をメモ

![image](https://i.gyazo.com/24e2225aa2e4f7d09b89c6d2ed6ec387.png)

ブラウザのアドレスを見て今回のサーバー URL をメモしておきましょう。

## Webhook URL の更新

https://developers.line.biz/ja/ こちらから、

![image](https://i.gyazo.com/5d1457a134175c620f142f92177ab373.png)

今回使っている BOT の Messaging API 設定に移動します。

![image](https://i.gyazo.com/c85ed28c0caa79951404327636ef1a44.png)

Webhook URL の項目に移動して以下の手順を行います。（大事な設定なので、一息タイミングを置いています）

### さきほどの URL に `/webhook` をつけて反映

![image](https://i.gyazo.com/3c32b8b9286ac4292628af21c546e08b.png)

さきほどの URL `https://**********.gitpod.io` に `/webhook` をつけて Webhook URL の項目に入力して更新ボタンをクリックします。

![image](https://i.gyazo.com/3c32b8b9286ac4292628af21c546e08b.png)

同時に Webhook の利用がオンになっていることも確認しましょう。

## BOT に返答が来るか確認しましょう

一旦ここで、以下のように動作するか確認してみましょう。

![image](https://i.gyazo.com/caea033f69762e2626a7bfc1a1425f7c.png)

送る文字列は、M5Stack で表示できる半角英数字を試してみましょう。

# 次にすすみましょう

左のナビゲーションから「M5Stack 側の仕組みをつくる」にすすみましょう。
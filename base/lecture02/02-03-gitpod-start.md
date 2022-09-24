# Gitpod で今回の LINE BOT の仕組みをはじめてみる

## 大事なこと

- Chrome ブラウザのなるべく最新版でアクセスしましょう

## 今回作る仕組み

![image](https://i.gyazo.com/4bed29421b7aaea13d3caf413b7d95c8.png)

まず、シンプルに Gitpod で LINE BOT のサーバーを立てることで、ユーザーから見た LINE BOT でオウム返しができています。

## 使う Git リポジトリ

すでに、今回の LINE BOT をはじめる環境は作ってあります。

https://github.com/1ft-seabass/dhw-pp2-m5stack-linebot-gitpod-2021 

![image](https://i.gyazo.com/7ae827af1638bebefb1a7d0d7dedce94.png)

こちらを使います。

## Gitpod で構築開始

Gitpod は GitHub 公開されたリポジトリであれば、すぐに `https://www.gitpod.io/#` のあとに GitHub リポジトリのある URL をつけた状態でアクセスすると構築できます。

ということで、以下にアクセスします。

https://www.gitpod.io/#https://github.com/1ft-seabass/dhw-pp2-m5stack-linebot-gitpod-2021

構築がはじまります。

![image](https://i.gyazo.com/668504b5f7cf347e325cd4c51ef7c403.png)

すすんでいきます

![image](https://i.gyazo.com/5acec3af8060d1e00fe3df60209d47da.png)

ここまですすむとだいぶ良い感じです。

![image](https://i.gyazo.com/f074c981bd29de8df0e1b20fb7f6309b.png)

しばらく待つと、このようにブラウザ上で Visual Studio Code が起動し、さきほどの GitHub の Git リポジトリにあるファイルが移植されています。

このあとは、このブラウザ上で Visual Studio Code で作業を進めていきましょう。

## npm install する

`package.json` に、すでに今回使う `express` や `@line/bot-sdk` ライブラリについて登録されているので、 `npm i` することでインストールを開始します。

```bash
npm i
```

コマンドでインストールをはじめます。

![image](https://i.gyazo.com/ae75dab42b90a82312247226cee490d1.png)

はじめてコピーアンドペーストするとブラウザ上で、許可するか聞かれるので、許可しましょう。

![image](https://i.gyazo.com/519376f4d0284d485f5083d0593fc2a7.png)

無事インストールされました。

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

LINE Developer コンソールの自分の LINE BOT 設定に戻ります。

![image](https://i.gyazo.com/5d1457a134175c620f142f92177ab373.png)

今回使っている BOT の Messaging API 設定に移動します。

![image](https://i.gyazo.com/c85ed28c0caa79951404327636ef1a44.png)

Webhook URL の項目に移動して以下の手順を行います。（大事な設定なので、一息タイミングを置いています）

### さきほどの URL に `/webhook` をつけて反映

![image](https://i.gyazo.com/3c32b8b9286ac4292628af21c546e08b.png)

さきほどの URL `https://**********.gitpod.io` に `/webhook` をつけて Webhook URL の項目に入力して更新ボタンをクリックします。

![image](https://i.gyazo.com/3c32b8b9286ac4292628af21c546e08b.png)

同時に Webhook の利用がオンになっていることも確認しましょう。

## BOT にオウム返しが来るか確認しましょう

一旦ここでちゃんと動いているかオウム返しを確認してみましょう。

![image](https://i.gyazo.com/3bf69c731aca0696a7f70f2dfdd9a670.png)

こちらで設定は出来ました。

## LINE BOT のプレゼンテーション時に便利なPC版アプリ

LINE BOT のプレゼンテーション時に便利なPC版アプリを紹介します。画面共有時にも状況が見せやすくなるのでおススメです。

![image](https://i.gyazo.com/895157df89c2ef3c8127e202c065eb76.png)

- パソコンでLINEを利用する｜LINEみんなの使い方ガイド
  - https://guide.line.me/ja/services/pc-line.html

こちらをみつつ、インストールしていきましょう。

![image](https://i.gyazo.com/a9812231b44c092ae5c3b7a072207e7a.png)

QR コードログインがおススメです。

# 次にすすみましょう

左のナビゲーションから「M5Stack + LINE BOT 連携」にすすみましょう。
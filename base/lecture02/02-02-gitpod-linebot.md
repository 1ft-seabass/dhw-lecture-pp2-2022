# Gitpod で LINE BOT と M5Stack 連携を学ぶ

![image](https://i.gyazo.com/2fe8f1e2d461451f6b5212996272c3ee.jpg)

## Gitpod とは

![image](https://i.gyazo.com/6451152f68865b7eabd6baefa944d8c9.jpg)

新しい開発環境を数秒で自動で構築しブラウザ上で起動するサービスです。開発環境のベースは既存の Git リポジトリをベースにすることができ、たとえば GitHub のようなサービスから開発環境をすぐに構築することできます。

### 手元の PC で環境構築する大半のことを準備してくれる

![image](https://i.gyazo.com/381be0c207f01631d83107e92d1d7f3b.png)

たとえば、前回学んだ [M5Stack から LINE BOT にメッセージを送ろう](../lecture02/02-02-line-bot-push.md) ですが、手元のPCで以下の準備をします。

- Node.js の開発環境を構築する
  - OS ごとに設定が違い、既存のインストールしたもの影響を受ける可能性がある
- Visual Studio Code をインストールしてエディタを準備する
  - ダウンロードなどそれなりの時間
- フォルダを作成して Visual Studio Code ではじめる
  - フォルダを作るだけだが開発しやすい場所にする必要がある
- LINE BOT を動かすために `express` や `@line/bot-sdk` ライブラリをインストール
  - `npm install` すればよいが、場合によっては、インストール時にエラーが出たり、ライブラリの不足による追加インストールなどがある
- app.js のソースコードをコピーアンドペーストあるいは自分でソースコードを準備
  - 各プロジェクトに応じたプログラムが動く

といったことを済ませてから、ようやく開発をすることができます。

### 開発したものの URL の公開も準備してくれる

![image](https://i.gyazo.com/bf10f3c76ff0107bb620d2b509b63f47.png)

また、LINE BOT が出来上がった時に ngrok を使って公開した URL を発行できるようにしますが、そういった部分も Gitpod も替わりに準備してくれます。

## LINE BOT とは

![image](https://i.gyazo.com/7552a5640c527078da82aa458641bb01.png)

LINE BOT は、LINE 上で動作する BOT です。後述する Messaging API を使ってあなたのサービスとLINEユーザーの双方向コミュニケーションを可能にします。

### LINE BOT と LINE Notify の違い

ポイントは双方向コミュニケーションができるかできないかの違いです。

LINE BOT は双方向コミュニケーションが可能で、IoT やデバイスを組み合わせると、現実世界と色々なかかわりができます。

なにかの特定のメッセージをユーザーが送ると LINE BOT が反応しセンサーから現実世界のデータを受け取って LINE に返答するようなことができます。動力のあるもの（アクチュエータ）と絡めると遠隔操作ができて自分の制作物でできることがひろがります。

もちろん、LINE Notify と同じように通知の役割もできます。デバイスが現実世界のデータをセンサーで取得して、データが何らかの値を越えた場合に通知するような仕組みもできます。

### LINE BOT の仕組み

- Messaging APIの概要
  - https://developers.line.biz/ja/docs/messaging-api/overview/ 

Messaging APIを使って、ユーザー個人に合わせた体験をLINE上で提供するボットを作成できます。

作成したボットは、LINEプラットフォームのチャネルに紐づけます。チャネルを作成すると生成されるLINE公式アカウントをボットモードで運用すると、LINE公式アカウントがボットとして動作します。

Messaging APIの仕組み
https://developers.line.biz/ja/docs/messaging-api/overview/#how-messaging-api-works

> ![image](https://i.gyazo.com/98f96b5609ecdd010d65a1e6ea8d9eee.png)
> 
> https://developers.line.biz/ja/docs/messaging-api/overview/#how-messaging-api-works より

Messaging APIを使って、ボットサーバーとLINEプラットフォームの間でデータを交換できます。リクエストは、JSON形式でHTTPSを使って送信されます。

- ユーザーが、LINE公式アカウントにメッセージを送信します。
- LINEプラットフォームからボットサーバーのWebhook URLに、Webhookイベントが送信されます。
- Webhookイベントに応じて、ボットサーバーからユーザーにLINEプラットフォームを介して応答します。

今回は YOUR SYSTEM と表現されている、Webhookイベントに応じて、ボットサーバーからユーザーにLINEプラットフォームを介して応答するサーバー部分をつくります。

- Messaging APIでできること・料金について
  - https://developers.line.biz/ja/docs/messaging-api/overview/#what-you-can-do 

# 次にすすみましょう

左のナビゲーションから「Gitpod で LINE BOT」にすすみましょう。
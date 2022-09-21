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

# 次にすすみましょう

左のナビゲーションから「Gitpod で LINE BOT」にすすみましょう。
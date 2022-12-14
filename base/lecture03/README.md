# 第3回 プロトタイピング発展概論・環境構築

![image](https://i.gyazo.com/ee01b5f25d0bed14e38b6ad0f4828a7d.png)

## この授業の概要

```
第3回　2022年10月05日（水） 7限　19:20 ～ 20:50　遠隔授業
```

※書かれている時間は予想の所要時間です。前後する可能性があります。

- はじめに 5 分 → このページ
- 宿題について 10 分
- M5Stack のボタンやディスプレイの実装を学ぶ 20 分
- M5Stack のセンサー連携を学ぶ その 1 30 分
- SNS からのアウトプットの事例や世界観を把握する 10 分
- M5Stack 事例を通してプロトタイプをアウトプットする世界観を学ぶ 10 分
- 余裕があれば
  - Grove 情報検索ナレッジ

## はじめに

![image](https://i.gyazo.com/cb9b9c279ea25ef482912ec9db7ff276.png)

- 途中退席
  - トイレなど急な用事で途中退席したいときは Zoom のコメントしつつで、いつでも行ってください。
  - それにより授業の方は止めませんが、なるべくこちらの資料で後追いができるようにしておりますので、抜けた間の把握はよろしくお願いします。
- コミュニケーションツールについて
  - Slack が中心となります。重要な情報は、デジキャンの掲示板も併用する予定ですが、基本的に Slack がメインとします。
  - Slack は1日1回以上は定期的にチェックください。
  - 質問や自分の制作物の進み具合など気軽に交流していきましょう！
- Zoom での授業について
  - ビデオについて
    - できるだけ、ビデオは ON でお願いします。
    - 手を動かしているときなど、雰囲気を見たいと思っています。
    - マシンスペックによってはキツいかもしれないので、そういう方は OFF でもOKです。
  - マイクについて
    - 通常はミュートでおねがいします。
    - ですが授業中に講師と会話をする場合があるので、マイクの設定もチェックしておいてください。
  - 画面共有
    - オンライン授業では、授業時に画面共有を使う機会が多いです。うまく行かないときの伝達や、疑問があるときの質問などなど。
- 授業の雰囲気を SNS に公開する場合があります
  - 公開してほしくない方は事前におっしゃってください。Slack の DM など。

### SNSアカウント

- 演習で LINE を利用する予定のため、LINEアカウントが必須です。
- 制作物はSNSへシェアを想定しているため、Twitter や Instagram などの公開アカウントが必須です。

### ツイート時の推奨ハッシュタグ

ツイート時は `#protoout #DHGS` をつけてお願いします。

- `#DHGS`
  - デジタルハリウッド大学院のハッシュタグなのでつけてみましょう。
    - [デジタルハリウッド大学院さん \(@DHGS\) / Twitter](https://twitter.com/dhgs)
- `#protoout`
  - プロトタイプしてアウトプットする意味で使います。ほかの人のアウトプットも見れるかも。

### その他の注意点（シラバスに記載）

- 演習形式で前後の関係性が連続しているため、欠席は不可です。
- 制作物を進めるにあたって外装や設置のために自分で物品購入する可能性があります。

## 分からないことあれば Slack で気軽に聞いてください

これから自分で作っていく時間が増えていくはずなので、つまづいているときには悩みすぎずSlack を活用して、聞いてくださいね～。改めてお伝えしておきます。

![image](https://i.gyazo.com/82ad117f19690778bd79c3df6bdaccfd.png)

## 第 3 回の心構え

![image](https://i.gyazo.com/2cb6bb2065f94760eb847eb5a9c5de21.png)

第 3 回では、IoT開発ボード M5Stack 入門です。

M5Stack のボタンやディスプレイの実装をはじめ、センサー連携について学びます。

- 宿題でのアウトプットもしながら最終制作の作るイメージを固めていきましょう。
- M5Stack の具体的な連携を学びながら作ってアウトプットすることで自分が使える力として定着させていきましょう。
- 第 5 回に第 6 回はじめの締め切りでアウトプットの宿題が出ますが、最終制作へ向けて制作物デモもありますし、作りながら徐々に良くしていくことが大切です。
- センサー連携については、今日第 3 回と次回の第 4 回で 2 回にわたって行います。

## 第 3 回のゴール

![image](https://i.gyazo.com/37ccdda7457e2a55fe177b4fc8973767.png)

今回のゴールは、以下の通りです。

- LINE BOT へ M5Stack からメッセージをだす仕組みを改めて把握する
- M5Stack のディスプレイやボタンを基礎から発展させ一歩踏み込んだ使い方を把握する
- M5Stack へのセンサーのつなぎ方を把握する
- M5Stack から行われるアウトプットを眺めて自分のアウトプットを小さくでもつたーとでするレベル感を把握する

## 今回はじめる前にできてると良いこと（理想形）

![image](https://i.gyazo.com/2426191c63343eb3f98402e2d3e238b1.png)

理想形ではあるので、現実に合わせて調整して進めていく予定です！

- LINE Developer アカウントを使用でき LINE BOT の設定が友達登録まで済んでいる
  - LINE BOT の作り方は 前回授業の [LINE BOT を作成する](../lecture02/12-line-bot-create.md) を参考にしましょう
- 今回使う LINE BOT の以下情報をプログラムにすぐ使える状態
  - 情報
    - 今回使う LINE BOT のチャンネルシークレット
    - 今回使う LINE BOT のチャンネルアクセストークン
    - 今回使う LINE BOT のユーザー ID
  - チャンネルシークレット・チャンネルアクセストークンは前回授業の [LINE BOT を作成する](../lecture02/12-line-bot-create.md) を参考、ユーザー ID は [M5Stack から LINE BOT にメッセージを送ろう](../lecture02/02-02-line-bot-push.md) を参考にしましょう。
- M5Stack に Arduino IDE からプログラムを書き込むことができる
- M5Stack で Wi-Fi がつながり先週試した HTTP や LINE Notify につながる
- 自分の Gitpod のアカウントが自分の GitHub のアカウントから作成できている
  - [Gitpod のアカウントを取得する](11-gitpod-account.md) を参考にアカウントを作りましょう。

## 授業開始

では授業をはじめましょう！

左のメニューから「Gitpod で LINE BOT と M5Stack」をクリックしましょう。

## デジキャンアンケートよろしくお願いします！

![image](https://i.gyazo.com/ae63e038ccb92474433c508557f40fda.png)

デジキャンのアンケートが事務局の方から出てますが、期日内で入力しましょう～。出席チェックと共に、私もみなさんのリアクションを気にしております。

## お疲れ様でした！

![image](https://i.gyazo.com/8c25c983712563658decb7babb379011.png)


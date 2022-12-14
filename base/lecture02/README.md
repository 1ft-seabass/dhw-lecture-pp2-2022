# 第2回 プロトタイピング発展概論・環境構築

![image](https://i.gyazo.com/ee01b5f25d0bed14e38b6ad0f4828a7d.png)

## この授業の概要

```
第2回　2022年09月28日（水）7限　19:20 ～ 20:50　遠隔授業
```

- はじめに 5 分
- 宿題のお話 5 分
- IoT開発ボード M5Stack の導入 20 分
- IoT開発ボード M5Stack から LINE BOT にメッセージを送る 30 分
- SNS からのアウトプットの事例や世界観を把握する 15 分
- 次回への準備 15 分

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

## 第 2 回の心構え

第 2 回では、いよいよ M5Stack 環境構築です。

今日で、うまくいけば、M5Stack でプログラムが書き込めるようになりインターネットにもつながるようになるはず。

M5Stack の持つ機能や魅力を把握しながら、自分の最終制作で作るものをイメージしていきましょう。

![image](https://i.gyazo.com/2cb6bb2065f94760eb847eb5a9c5de21.png)

また、第 3 回の終わりに  M5Stack で小さく作って　Twitter へアウトプットする宿題を出します。期限は第 4 回開始までの予定です。

ですので、今回の段階で、ある程度どういう風に手を動かすかのイメージをつけて、徐々に制作時間を作っていくとよいでしょう。最終制作へ「作りつづける」ウォームアップでもあります。

## 第 2 回のゴール

![image](https://i.gyazo.com/37ccdda7457e2a55fe177b4fc8973767.png)

今回のゴールは、以下の通りです。

- M5Stack が完成したソースコードを書き込む形で動かす流れを把握する
- M5Stack を少し崩して変更したり、デバッグするためのやり方を把握する
- M5Stack のボタンやディスプレイの基本的な動きやインターネットへのつなぎ方を把握する
- M5Stack と LINE サービス（LINE Notify ・ LINE BOT）がソースコードを書き加える形で連携できる
- M5Stack をとりまくアウトプットを元に自分のこれからのアウトプットに生かせる糧にする

## 今回はじめる前にできてると良いこと（理想形）

![image](https://i.gyazo.com/2426191c63343eb3f98402e2d3e238b1.png)

理想形ではあるので、現実に合わせて調整して進めていく予定です！

- 機材リストで示した機材が購入できていて揃っている状態
  - M5Stack baseline は今日の授業で使うので必須
- Arduino IDE に M5Stack の開発環境が整っている状態
  - 前回の宿題でセットアップが済み
  - Slack で連絡した事前チェックが済んでいるとよりよい（トラブルなく始められる確率が高まります）
- プロダクトプロトタイピング I ベースの話
  - LINE Developer アカウントが取得済みで LINE BOT 制作がすぐ取り掛かれる状態

とくに、「プロダクトプロトタイピング I ベースの話」については、生徒さんごとに事情が変わってくるかもなので、LINE Developer アカウントだけあればできる LINE Notify もやってみる予定です。

### もし可能なら M5Stack 事前チェックをしましょう（Slack連絡済み）

授業前にセットアップがうまく行っているか事前確認しましょう。

- [M5Stack セットアップチェック](10-m5stack-check.md)

### M5Stack トラブルシューティング TIPS（Slack連絡済み）

セットアップ周辺でSlack などのやり取りで出てきたトラブルシューティング TIPSをまとめています。（進行形）

- [M5Stack トラブルシューティング TIPS](11-m5stack-trouble-shooting-tips.md)

## 授業開始

では授業をはじめましょう！

左のメニューから「M5Stack の導入」をクリックしましょう。

## デジキャンアンケートよろしくお願いします！

![image](https://i.gyazo.com/ae63e038ccb92474433c508557f40fda.png)

デジキャンのアンケートが事務局の方から出てますが、期日内で入力しましょう～。出席チェックと共に、私もみなさんのリアクションを気にしております。

## お疲れ様でした！

![image](https://i.gyazo.com/8c25c983712563658decb7babb379011.png)


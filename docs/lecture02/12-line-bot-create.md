# LINE BOT を作成する

[1時間でLINE BOTを作るハンズオン \(資料\+レポート\) in Node学園祭2017 \#nodefest \- Qiita](https://qiita.com/n0bisuke/items/ceaa09ef8898bee8369d#1-bot%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)

こちらの、「1. Botアカウントを作成する」～「Botと友達になろう」まで進めます。

## LINE Developers にログイン

![image](https://i.gyazo.com/4bb4c0bffb3c961ea749ec833e2e826b.jpg)

LINE Developers の右上のメニューから自分の LINE アカウントでログインします

https://developers.line.biz/ja/

![image](https://i.gyazo.com/415c9566b4b1d2e51b17f55f59345701.png)

`LINE アカウントでログイン` ボタンをクリックします。

![image](https://i.gyazo.com/5efe71ed2c45acc347a40b34340880f5.png)

メールアドレスとパスワード、もしくは、QRコードでログインできます。

### 言語設定が日本語になっているか確認

![image](https://i.gyazo.com/8391997e7db3ce012a4e4f3010085805.png)

ログインしたあと右下に言語設定のエリアがあるので `日本語` になっているか確認しましょう。以後の流れは、日本語で設定された画面の前提で進みます。

### ログインできない場合は

* LINEのスマートフォンアプリ側で設定を確認しましょう。
* 設定がまだの方はLINEのスマートフォンアプリ画面から 設定>アカウントでメールアドレスを設定できます。
    * [LINEでメールアドレスを新規登録・確認・変更・解除（削除）する方法 \| アプリオ](https://appllio.com/line-mail-address-settings)

## プロバイダーの作成

ログインしたらプロバイダーの作成を行います。

プロバイダーは、自分が作るLINE BOTなどの開発者名やチーム名、企業名になります。
初回だけディベロッパー登録で個人情報を聞かれると思うので回答してから進めましょう。

![image](https://i.gyazo.com/d69a72b6767e358c7bf4808a6546a5ec.png)

コンソール（ホーム）を押してプロバイダーの画面に行きます。

![image](https://i.gyazo.com/545aeaed54b1081be46ccc97ada43006.png)

作成ボタンを押して進みます。

![image](https://i.gyazo.com/4b73740ebdb9a26dfb8fa4457bd2eefc.png)

プロバイダー名は今回は デジハリ開発 と入力して作成ボタンを押して進めましょう。

![image](https://i.gyazo.com/08d33da3d350eef0510f5643d4f26809.png)

デジハリ開発 の プロバイダー のチャネル設定に移動されます。

![image](https://i.gyazo.com/f5dacf9f99c96cef4a5b2a7ba2b0c30d.png)

今回は LINE BOT を作るので `Messaging API` をクリックします。

## BOTの情報入力

BOTの情報を入力します。

![image](https://i.gyazo.com/5e922a871132eda6c0a122f00a9d1c02.png)

チャネルの種類は先ほどMessaging API を選択して入れば、 `Messaging API` となっています。
プロバイダーは デジハリ開発に設定されています。

![image](https://i.gyazo.com/a56c6059ddb93f8a9ea0261adc19ba00.png)

チャネルアイコンは、BOTの顔になるので、お好みの画像を入れましょう。
チャネル名は、今回は「デジハリBOT」にします。

もちろん、自分の自由な名前にしていただいてもOKです！

![image](https://i.gyazo.com/b69ca35da7df9ec380e826dcfafd5636.png)

チャネル説明・大業種・小業種は、とりあえず、なんでもOKですが、悩んだら画像のようにしておきましょう。

![image](https://i.gyazo.com/5384b4ee7c11faa87c6bc93377971b45.png)

メールアドレスは、BOTの開発者にLINE側からお知らせがあるときに受け取るメールアドレスを入力します。
プライバシーポリシーURL/サービス利用規約URL（任意）こちらは、今はなくても大丈夫です。

![image](https://i.gyazo.com/e9ca90c74fc6b980d12f3e680fc8e21f.png)

LINE公式アカウント利用規約 の内容と、LINE公式アカウントAPI利用規約 の内容を確認して、チェックボックスを選択して、作成ボタンをクリックします。

![image](https://i.gyazo.com/2c9c61b4c0e134f050dc84271fc55bd2.png)

こちらは OK をクリック。

最後に、情報利用に関する同意についてというページが出るので、スクロールして内容を確認したら、同意ボタンをクリックできるようになるので、クリックして進めます。

![image](https://i.gyazo.com/673c5772019b52faf8708042f926cddf.png)

作成が完了すると、作成されたデジハリBOTの設定ページに移動します。

## チャネルシークレットをメモする

![image](https://i.gyazo.com/673c5772019b52faf8708042f926cddf.png)

のちほど必要になるチャネルシークレット・チャネルアクセストークンをメモしておきます。

![image](https://i.gyazo.com/2ecb660f54ea6642859393d38c58261f.png)

上部のメニューでチャネル基本設定をクリックします。

![image](https://i.gyazo.com/2a14f044f7b5e7983868bb879b9c8464.png)

チャネル基本設定でチャネルシークレットをスクロールを探して、チャネルシークレットをコピーしてメモしておきます。

## チャネルアクセストークンをメモする

![image](https://i.gyazo.com/09da02fd0ecf211aae686575ae26f09a.png)

上部のメニューで Messageing API 設定をクリックします。

![image](https://i.gyazo.com/257d7f2945334257509452089e1b2c2f.png)

Messageing API 設定からチャネルアクセストークン（ロングターム）を探します。
チャネルアクセストークン（ロングターム）がまだ発行されていないので発行ボタンをクリックします。

![image](https://i.gyazo.com/14fe620ea842aaf6ea2ce51d5782b52e.png)

チャネルアクセストークンが発行されるので、赤い矢印で示したコピーできるボタンをクリックしてコピーしてメモしておきます。

これくらい長い文字列を手で選択してコピーすると、カーソルがズレたりうまく全部コピーできなくて、あとでバグを生みやすいので、コピーボタン利用でお願いします！

## チャネルアクセストークンとチャネルシークレットは BOT の大切な設定です

![image](https://i.gyazo.com/19c9bce2a892144f6c514d805098b70d.png)

チャネルアクセストークンとチャネルシークレットは BOT の大切な設定です。

間違いなくメモできているか、いま一度、確認しましょう。

## BOT の設定を変更する

BOT の設定をしていきます。

![image](https://i.gyazo.com/463bc511b0eb7f6d489a2370c7d9705d.png)

上部のメニューで Messageing API 設定をクリックします。

![image](https://i.gyazo.com/dba92aed365f3d3717448f297c420c11.png)

つづいて、LINE公式アカウント機能の欄で、たとえば、応答メッセージの横にある編集ボタンをクリックするとLINE公式アカウントマネージャーに移動します。

![image](https://i.gyazo.com/d8bc8b44c44577d411a231690486c9da.png)

移動しました。

![image](https://i.gyazo.com/e16e36a28c3405b9ed672f8aa707cb8b.png)

右メニューから応答設定をクリックします。

![image](https://i.gyazo.com/bbbfa50856c244333e29f0c0cf080af4.png)

- 応答メッセージ
  - オフ
- あいさつメッセージ
  - オフ
- Webhook
  - オン

に設定します。

![image](https://i.gyazo.com/3e5dbe3839bb48b74d90dd2ce06d214f.png)

アカウント設定をクリックします。

![image](https://i.gyazo.com/087291cfb097af213b2393cc9802ce82.png)

チャットへの参加の項目で、`グループ・複数人チャットへの参加を許可する` をクリックして設定します。

![image](https://i.gyazo.com/e16e36a28c3405b9ed672f8aa707cb8b.png)

Webhookのオンはうまく反映されない場合があります。もう一度、右メニューから応答設定をクリックして、確認しましょう。

![image](https://i.gyazo.com/e1acdc8c35cb5427786b5d3d8a2317fa.png)

Webhook　がオンになっていればOKです。


## BOTと友達になろう

ここまで準備が出来たら、BOTと友達になりましょう。

LINE Developers の画面に戻り、自分のプロバイダーから、今回の LINE BOT を選択します。

![image](https://i.gyazo.com/3005dee9322bd8cac7ed7c54fa394525.png)

もし、プロバイダーが見つからない方は、自分のプロフィールページに移動してから、右メニューにあるプロバイダーをクリックします。

![image](https://i.gyazo.com/2dbb7bc5badd542ee4f6235a0a27ae71.png)

プロバイダーの中のチャネルを選ぶ画面に移動するので、デジハリBOT を選択しましょう。

![image](https://i.gyazo.com/ce2241de3c8e56ce58b7fc2f0f7267c7.png)

上部のメニューで Messageing API 設定をクリックします。

![image](https://i.gyazo.com/3c7059023e9dbc2fb5aeb3b668e2e92d.png)

ここで表示されるQRコードを使って、自分が作成したBotアカウントと友達になりましょう。

![image](https://i.gyazo.com/361130735c8a807e31a7cd22383f07f7.png)

Android や iOS の LINE アプリの画面です。先ほどのQRコードに合わせて読み取ります。 OS やバージョンによって配置が違うかもしれませんのでご注意ください。

![image](https://i.gyazo.com/5c39802cd9f04681c0a41928041da04f.png)

追加をクリックして、友達追加します。追加すると、友達登録され、ウェルカムメッセージが表示されます。

これで、BOTの友達登録の完了です。
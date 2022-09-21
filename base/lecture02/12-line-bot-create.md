# LINE BOT を作成する

## LINE Developers にログイン

![image](https://i.gyazo.com/4bb4c0bffb3c961ea749ec833e2e826b.jpg)

LINE Developers の右上のメニューから自分の LINE アカウントでログインします

https://developers.line.biz/ja/

![image](https://i.gyazo.com/415c9566b4b1d2e51b17f55f59345701.png)

`LINE アカウントでログイン` ボタンをクリックします。

![image](https://i.gyazo.com/5efe71ed2c45acc347a40b34340880f5.png)

QRコードログインをクリックします。

![image](https://i.gyazo.com/a6834bc86db3d0f024a0cb5886664883.png)

QR コードが表示されます。 LINE アプリでスキャンするとログインできます。QR コードのスキャン方法は下部のリンクをクリックすると表示されます。

![image](https://i.gyazo.com/e3856b10d3a1b933f284f7f81f3fac64.png)

### 言語設定が日本語になっているか確認

![image](https://i.gyazo.com/8391997e7db3ce012a4e4f3010085805.png)

ログインしたあと右下に言語設定のエリアがあるので `日本語` になっているか確認しましょう。以後の流れは、日本語で設定された画面の前提で進みます。

## プロバイダーの作成

ログインしたらプロバイダーの作成を行います。

プロバイダーは、自分が作るLINE BOTなどの開発者名やチーム名、企業名になります。
初回だけディベロッパー登録で個人情報を聞かれると思うので回答してから進めましょう。

![image](https://i.gyazo.com/4b22410ca3a52c095b6bfd6649fa1b65.png)

コンソール（ホーム）を押してプロバイダーの画面に行きます。

![image](https://i.gyazo.com/545aeaed54b1081be46ccc97ada43006.png)

作成ボタンを押して進みます。

![image](https://i.gyazo.com/efc6caead4c0f2920732fd8532914c3c.png)

プロバイダー名は今回は デジハリ開発 と入力して作成ボタンを押して進めましょう。

![image](https://i.gyazo.com/61e46e7a58e050e565d773e6a0a6668a.png)

デジハリ開発 の プロバイダー のチャネル設定に移動します。

![image](https://i.gyazo.com/f5dacf9f99c96cef4a5b2a7ba2b0c30d.png)

今回は LINE BOT を作るので `Messaging API` をクリックします。

## BOTの情報入力

BOTの情報を入力します。

![image](https://i.gyazo.com/64bb0cbb98a7df993ac506cd3c9a367c.png)

チャネルの種類は先ほどMessaging API を選択して入れば、 `Messaging API` となっています。
プロバイダーは デジハリ開発に設定されています。

![image](https://i.gyazo.com/a56c6059ddb93f8a9ea0261adc19ba00.png)

チャネルアイコンは、BOTの顔になるので、お好みの画像を入れましょう。
チャネル名は、はじめてなのでファーストBOTのような名前にしてみましょう。

もちろん、自分の自由な名前にしていただいてもOKです！

![image](https://i.gyazo.com/39977a611932d9aa5e9c01e2bceae929.png)

会社・事業者の所在国・地域 は日本にします。

![image](https://i.gyazo.com/9abdb29b15bcb6e28302e74dbb4f1b64.png)

チャネル説明・大業種・小業種は、とりあえず、なんでもOKです。

![image](https://i.gyazo.com/5384b4ee7c11faa87c6bc93377971b45.png)

メールアドレスは、BOTの開発者にLINE側からお知らせがあるときに受け取るメールアドレスを入力します。
プライバシーポリシーURL・サービス利用規約URL（任意）こちらは、今はなくても大丈夫です。

![image](https://i.gyazo.com/e9ca90c74fc6b980d12f3e680fc8e21f.png)

LINE公式アカウント利用規約 の内容と、LINE公式アカウントAPI利用規約 の内容を確認して、チェックボックスを選択して、作成ボタンをクリックします。

![image](https://i.gyazo.com/9bb6158eb239c235720580b68a68e042.png)

こちらは OK をクリック。

最後に、情報利用に関する同意についてというページが出るので、スクロールして内容を確認したら、同意ボタンをクリックできるようになるので、クリックして進めます。

![image](https://i.gyazo.com/704eef78095005b9ac0abc7ab4b80b7a.png)

作成が完了すると、作成されたBOTの設定ページに移動します。

## チャネルシークレットをメモする

![image](https://i.gyazo.com/704eef78095005b9ac0abc7ab4b80b7a.png)

のちほど必要になるチャネルシークレット・チャネルアクセストークンをメモしておきます。

![image](https://i.gyazo.com/b171de5b865c48f2b48f3e62251dcdc8.png)

上部のメニューでチャネル基本設定をクリックします。

![image](https://i.gyazo.com/2a14f044f7b5e7983868bb879b9c8464.png)

チャネル基本設定でチャネルシークレットをスクロールを探して、チャネルシークレットをコピーしてテキストエディタにメモしておきます。

## チャネルアクセストークンをメモする

![image](https://i.gyazo.com/cd281b38be487550450e876bb6ce2916.png)

上部のメニューで Messageing API 設定をクリックします。

![image](https://i.gyazo.com/876f2d4ffd41e7d0790ff7d88a82e136.png)

Messageing API 設定からチャネルアクセストークン（長期）を探します。
チャネルアクセストークン（長期）がまだ発行されていないので発行ボタンをクリックします。

![image](https://i.gyazo.com/14fe620ea842aaf6ea2ce51d5782b52e.png)

チャネルアクセストークンが発行されるので、赤い矢印で示したコピーできるボタンをクリックしてコピーしてテキストエディタにメモしておきます。

これくらい長い文字列を手で選択してコピーすると、カーソルがズレたりうまく全部コピーできなくて、あとでバグを生みやすいので、コピーボタン利用でお願いします！

## チャネルアクセストークンとチャネルシークレットは BOT の大切な設定です

![image](https://i.gyazo.com/19c9bce2a892144f6c514d805098b70d.png)

チャネルアクセストークンとチャネルシークレットは BOT の大切な設定です。

間違いなくメモできているか、いま一度、確認しましょう。

## BOT の設定を変更する

BOT の設定をしていきます。

![image](https://i.gyazo.com/cd281b38be487550450e876bb6ce2916.png)

上部のメニューで Messageing API 設定をクリックします。

![image](https://i.gyazo.com/bd2dd6be610af6e21fd006dc9a7ad6ee.png)

つづいて、LINE公式アカウント機能の欄で、応答メッセージの横にある編集ボタンをクリックするとLINE公式アカウントマネージャーに移動します。

![image](https://i.gyazo.com/ed0cdfa1c0f10b134ea45612aa7521bb.png)

移動しました。

![image](https://i.gyazo.com/e16e36a28c3405b9ed672f8aa707cb8b.png)

右メニューから応答設定をクリックします。

![image](https://i.gyazo.com/d1f780f9dd135650333c0dc8501686be.png)

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

Webhook がオンになっていればOKです。

## BOTと友達になろう

ここまで準備が出来たら、BOTと友達になりましょう。

![image](https://i.gyazo.com/cb97bb47e0e99f2b628eef519be1463d.png)

ブラウザのタブから LINE Developers の画面に戻り、自分のプロバイダーから、今回の LINE BOT を選択します。

![image](https://i.gyazo.com/1e19d385d4e77ea4fa22815c417538b0.png)

もし、プロバイダーが見つからない方は、自分のプロフィールページに移動してから、右メニューにあるプロバイダーをクリックします。

![image](https://i.gyazo.com/a811fe5961d2ef499b0b99c3c8a4baa3.png)

プロバイダーの中のチャネルを選ぶ画面に移動するので、今回の BOT を選択しましょう。

![image](https://i.gyazo.com/cd281b38be487550450e876bb6ce2916.png)

上部のメニューで Messageing API 設定をクリックします。

![image](https://i.gyazo.com/3c7059023e9dbc2fb5aeb3b668e2e92d.png)

ここで表示されるQRコードを使って、自分が作成したBOT アカウントと友達になりましょう。

![image](https://i.gyazo.com/361130735c8a807e31a7cd22383f07f7.png)

Android や iOS の LINE アプリの画面です。先ほどのQRコードに合わせて読み取ります。 OS やバージョンによって配置が違うかもしれませんのでご注意ください。

![image](https://i.gyazo.com/e505719445b806bd804a4059dc84e120.png)

追加をクリックして、友達追加します。

![image](https://i.gyazo.com/835c35b63626f283dd60bc449fee2cc0.png)

追加すると、トークができるようになります。

![image](https://i.gyazo.com/171890302d2ed822bc3cfb8aa38ea828.png)

再度 BOT を探す場合は友達リストから公式アカウントで探すことができます。

これで、BOTの友達登録の完了です。
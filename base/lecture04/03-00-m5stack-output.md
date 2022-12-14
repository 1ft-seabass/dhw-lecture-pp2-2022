# いままでのセンサー・ボタン・ディスプレイ実装とアウトプットへの関係性を学ぶ

![image](https://i.gyazo.com/2fe8f1e2d461451f6b5212996272c3ee.jpg)

すでに制作をはじめていますが、内蔵センサー・Grove センサー・ボタン・ディスプレイ・スピーカーあたりのひと通り学んだところでアウトプットへの関係性を振り返ってみましょう。

## 内蔵ディスプレイ

![image](https://i.gyazo.com/aa25587bac5a50c1697b651f668a2cd3.png)

広いエリアで視覚的に伝わるのが内蔵ディスプレイです。背景色やテキストをはじめ、定期的に位置や大きさを変えていくことによってアニメーションも可能です。

ツイート時でも実際に触ってもらう場合でも、パッと触りたくなる案内をディスプレイに出せるようにしましょう。

ツイート時は動画や写真ですぐにわかるような見せ方を心がけると、説明が少なく直感的に伝わるコンテンツが作りやすいです。実際に触ってもらう場合では、触れるときに迷わないような案内を表示すると、同様の効果が得られます。

どちらにも言えることですが、何が起こっているかについても、丁寧に表示していくことで、使う人見る人がびっくりしない優しいユーザーインターフェースになります。

また、スマートフォンや Web での見せ方伝え方も、 M5Stack のディスプレイの見せ方に活用できる場合があります。うまく加えていきましょう。

## 内蔵スピーカー

![image](https://i.gyazo.com/2496cf10b5cab471ec4548eb3a979167.png)

聴覚に情報を伝えるのは音です。

ディスプレイで見ている情報に対して補助的に音をつけるのも、より分かりやすさが増して効果的です。

音の特性は見たときの補助的な伝達だけではありません。たとえば、視覚に頼らない伝達。M5Stack そのものを見なくても、隣の部屋で M5Stack から音が聞こえれば何かしらの情報を伝えることができます。

唐突な音では周囲をびっくりさせる可能性はあるので取り扱いには注意が必要ですが、あらかじめ、どういった音が鳴るかを示したり、説明が無くても直感的に良し悪しが分かるメロディを組み合わせれば、ディスプレイだけではない効果的な伝達できます。

## 内蔵ボタン

![image](https://i.gyazo.com/d7f5598b726b15ce670584a255837a2c.png)

M5Stack の 3 つのボタンは実装しやすく、M5Stack を触れた人が M5Stack と次のコミュニケーションをするための大切な入口になります。ディスプレイと組み合わせて説明付きのボタン操作で、分かりやすく自分のコンテンツに案内することもできるでしょう。

ボタンをただ押すだけだと 3 つの操作しかできないように思いますが、長押しや素早い2度押しを組み合わせることで操作が広がります。複雑なコマンドもできるので、たとえば、素早い2度押しで仕組みのリセットをしたり、デバッグモードを長押しに仕込んでみたり、自分のつくった M5Stack をうまく拡張していきましょう。

## 内蔵バッテリー

![image](https://i.gyazo.com/45a1659f2b05f40e1df08b10e7abb9ce.png)

普段は USB で書き込んでいるため、常時給電されているので気づきにくいですが、 M5Stack はバッテリーがあるため、USB から離しても、内蔵バッテリーが続くかぎり給電なしで動かすことができます。

ですが、[m5\-docs](https://docs.m5stack.com/en/core/basic) にもあるとおりで、`110mAh @ 3.7V` と、あまり大きい電池ではありません。

また、Wi-Fi やインターネット接続を繰り返す処理は電池の消費が激しくてすぐにバッテリーがなくなってしまうでしょう。

もちろん、サッと見せるときには心強い味方になってくれますが、使い続けるときには、他のモバイルバッテリーをつなげたり、PCやコンセントからの優先での給電も意識して自分のコンテンツを考えていきましょう。

IoT では、電源問題はつねに意識する事柄なので、うまく付き合っていきましょう。

モバイルバッテリーによっては M5Stack 程度の電流だと省エネ判定されて1分くらいで勝手に自動 OFF になるものも存在します。

- [【白色】IoT機器用モバイルバッテリー cheero Canvas 3200mAh \- スイッチサイエンス](https://www.switch-science.com/catalog/2618/)
- [【黒色】IoT機器用モバイルバッテリー cheero Canvas 3200mAh \- スイッチサイエンス](https://www.switch-science.com/catalog/3167/)

のような、IoT 向きの自動電源 OFF のない、手動で電源 ON / OFF できて給電を維持できるモバイルバッテリーもあるので、状況に応じて検討しましょう。（個人的には、1積もっておいて損はないです）

すぐ誰かに見せるか？ずっと使い続けるか？といったところバッテリーの扱いと密接に関係するので、つくったあとにどういうシチュエーションで仕組みを使っていくかを考えながら進めましょう。

## Grove センサー

![image](https://i.gyazo.com/ad4c9453685b492d9b9601a2c7464b15.png)

Grove センサーは以前の授業ですでに良さは伝えていますが、あらためて、M5Stack アウトプットの目線から言うと M5Stack から先の「広がり」を担当しています。

- [Grove \- スイッチサイエンス](https://www.switch-science.com/collections/all/cat:%E3%83%96%E3%83%A9%E3%83%B3%E3%83%89%E5%B0%8F%E5%88%86%E9%A1%9E_Grove)
- [Grove \- Seeed Studio 社](https://jp.seeedstudio.com/category/Grove-c-1003.html)

には、豊富な Grove シリーズが紹介されているので、アイデアの種として考えて自分の作りたいものと対話させていくのはおススメです。実は、講師の田中も定期的にエクササイズと考えて、リストを眺めながら発想を膨らませています。

![image](https://i.gyazo.com/e4ff3aa54d04bb9ef85faeabc28925a6.jpg)

↑ [Grobe センサ検索 \- スイッチサイエンス](https://www.switch-science.com/search?type=article%2Cpage%2Cproduct&q=Grove*+%E3%82%BB%E3%83%B3%E3%82%B5*) の一部

自分の発想をもとに使える技術を探して覚えていく探索も大切です。そして、このように技術（センサー）をベースに現実世界を想像しながら発想することも大切です。

発想と技術を行き来させることで自分のスキルを磨いていきましょう。

# 質疑応答

![image](https://i.gyazo.com/aba8ccd625e7320883851b71ebd0caf2.png)

ここまでで質問があればどうぞ！

# 次にすすみましょう

左のナビゲーションから次の教材にすすみましょう。
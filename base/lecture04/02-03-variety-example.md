# さまざまな連携例

ここまでは、内蔵センサーや Grove センサーを LINE BOT につなげるような事例で「みなさんにもすぐに試せる」ことを中心にお伝えしてきましたが、M5Stack の連携にもいろいろな広がりがあります。

おもに私の事例を軸に、いくつかダイジェストで紹介していきます。

## より突っ込んだ M5Stack 操作

### MQTT に送る JSON をよりカスタマイズしてビジュアル変更演出実験

![image](https://www.1ft-seabass.jp/memo/uploads/2020/10/controlling-m5stack-from-node-red-mqtt-json-senarios_00-644x358.jpg)

JSON で、シナリオデータを送って、ディスプレイの背景色を変えたり、文字を変えたり、かつ変わる秒数を細かく指定出来たりという事例。

[M5Stack で MQTT から Node\-RED 経由で JSON データでまとめてシナリオを渡してビジュアルが変わる仕組みを作ったメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2020/10/31/controlling-m5stack-from-node-red-mqtt-json-senarios/)

### LINE Beacon 例

![image](https://www.1ft-seabass.jp/memo/uploads/2021/01/m5stack-line-beacon-with-node-red-line-bot_00-644x314.png)

LINE Beacon の信号を M5Stack から出せると、M5Stack はビーコンを出すだけの仕組みでセンサーやボタンなどと連携ができて、ビーコン検出後の LINE での通知は LINE BOT に任せることができて、また違った用途がイメージできます。

[M5Stack に LINE Beacon で書き込み Node\-RED で作った LINE BOT とやり取りするメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2021/01/03/m5stack-line-beacon-with-node-red-line-bot/)

## センサーで考えが広がる例

### サーマルカメラ例

![image](https://www.1ft-seabass.jp/memo/uploads/2021/05/grove-thermal-camera-ir-array-mlx90640-110-degree-firststep_00-800x448.jpg)

対象物の熱の状態を見る熱センサー サーマルカメラ を M5Stack の連携。本来、サーマルカメラは 1 pixel ごと情報が来るため処理負荷が高いですが M5Stack だとバッチリ取得できます。

[Grove 赤外線サーマルカメラ IRアレイ MLX90640 110度 を M5Stack で動かしたメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2021/05/05/grove-thermal-camera-ir-array-mlx90640-110-degree-firststep/)

- 難しめのセンサー接続についての試行錯誤を見てみたい人は是非。

### CO2 センサー例

![image](https://www.1ft-seabass.jp/memo/uploads/2021/03/m5stack-connect-grove-co2-sensor-scd30_01-644x366.jpg)

COVID-19 の時期には欠かせないセンサー CO2 センサー。こちらの連携例。

[M5Stack と Grove CO2＆温度＆湿度センサー SCD30 をつないで計測するメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2021/03/08/m5stack-connect-grove-co2-sensor-scd30/)

- 難しめのセンサー接続についての試行錯誤を見てみたい人は是非。
- CO2 センサーのような、いま社会で必要度が高いものを取得できると、いろいろな使い方がパーッと広がってきます

## 見た目大事系

### ハロウィンの光り物にピッタリ

![image](https://zoe6120.com/wp-content/uploads/2019/07/IMG_1645.jpg)

M5Stack + テープLED の事例。はんだ付けが必要だが、光って強い。

[M5StackとESP32でNeoPixel互換LEDテープを点灯する – zoe log](https://zoe6120.com/2019/07/03/990/)

## ほかの技術との連携で広がっていく例

### ローコードツール Node-RED と MQTT で連携する例

![image](https://www.1ft-seabass.jp/memo/uploads/2018/05/m5stack-meets-nodered-with-mqtt_01.png)

[M5StackとNode\-REDをMQTTで連携するメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2018/05/10/m5stack-meets-nodered-with-mqtt/)

### クラウドとの連携 IBM Cloud

![image](https://www.1ft-seabass.jp/memo/uploads/2018/05/m5stack-meets-ibm-watson-iot-platform_01-644x271.png)

[M5StackをIBM Watson IoT Platform \(QuickStart\) にデータ送信＆可視化するメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2018/05/24/m5stack-meets-ibm-watson-iot-platform/)

### クラウドとの連携 AWS

![image](https://www.1ft-seabass.jp/memo/uploads/2018/05/m5stack-meets-aws-iot_01-644x336.png)

[M5Stackにつないだデジタル入力をAWS IoTにデータ送信するメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2018/05/25/m5stack-meets-aws-iot/)

### スプレッドシート的なデータの貯め方ができる AirTable 連携例

![image](https://www.1ft-seabass.jp/memo/uploads/2021/10/m5stack-airtable-api-connect_01-800x415.png)

[M5Stack から HTTPS で AirTable にデータを書き込むメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2021/10/07/m5stack-airtable-api-connect/)

### むちゃくちゃ組み合わせた事例

![image](https://www.1ft-seabass.jp/memo/uploads/2018/07/tello-fly-m5stack-command-force-land_02.png)

鍵をかけると、ドローンが飛ぶという、謎の連携。ドローンの UDP プロトコルを Node-RED が中継して M5Stack ＋EAO 鍵スイッチ の反応を届けています。

[トイドローン Tello をM5Stack＋EAO 鍵スイッチで緊急着陸させるメモ – 1ft\-seabass\.jp\.MEMO](https://www.1ft-seabass.jp/memo/2018/07/18/tello-fly-m5stack-command-force-land/)


## 使いこなすためにパワーは使いますが頑張りましょう～

![image](https://i.gyazo.com/8bb6da4820529871e09aa5d80d1483c8.png)

ここまでくると、いろいろな知見知った時に、自分でうまく切り取って自分の作りたいものに加えることが大切になってきますががんばりましょう！

# 次にすすみましょう

左のナビゲーションから次の教材にすすみましょう。
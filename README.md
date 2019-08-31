# JAWS-UG 高知 2019/8/31 サーバレスハンズオン


# 前提
slack使ったことのない人はこのワークスペースに。利用している方は、そのワークスペースでも大丈夫です。

https://join.slack.com/t/jaws-kochi-0831/shared_invite/enQtNzMxOTYwMTUwOTQ2LWY1OWVlNzgyNTkxNDc1OTk2OWVmZmEzZGUzNmI3MjRlYjhjOTRlZGNhODBkODc0NjlmNGE1ZDBlM2M3YmYzYzc


# 操作手順
1.SlackのWorkspaceにIncoming Webhookを登録
2.SlackにPOSTするプログラム作成 (Python)
3.AWSの設定

Incoming Webhookアプリを使用する場合の登録手順は以下になります。

## 1.SlackのWorkspaceにIncoming Webhookを登録
A.slack appを作成
B.チャンネルを選択
C.Webhook URLをコピー
(名前、アイコンなどをカスタマイズ)

### A.slack appを作成
https://api.slack.com/incoming-webhooks#posting_with_webhooks

create your slack app をクリック
![k_1](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_1.png)


### B.チャンネルを選択

app名を入力し、チャンネルにJAWS高知を選択する
![k_3](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_3.png)


送信先にgeneralを選択
![k_4](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_4.png)



### c.Webhook URLをコピー
下のような画像のURLがでてくるので、それをメモ帳にコピーしておく。

![k_5](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_5.png)


このコマンドをターミナルで打ち、自分が指定したチャンネルに送信できているか確認
curl -X POST -H 'Content-type: application/json' --data '{"text":"Hello, World!"}'
https://hooks.slack.com/services@@@@@@@@@@@@@@@@

## 2.slackにPOSTするプログラムを作成(Python)

```
import json
import urllib.request

def post_slack():
    message = "好きなmessage"
    send_data = {
        "username": "自分の名前",
        "icon_emoji": ":grinning:",
        "text": message,
    }
    send_text = "payload=" + json.dumps(send_data)
    # URLにはご自分のWebhook URLを入力してください
    request = urllib.request.Request(
        "https://hook.slack.com/~~~~~~~~", 
        data=send_text.encode("utf-8"), 
        method="POST"
    )
    with urllib.request.urlopen(request) as response:
        response_body = response.read().decode("utf-8")
```

## 3.AWSの設定 <- こっから本編

名前は任意のものを入力してください。
ランタイムは今回はPython 3.7を選択します。
ロールは、特に権限を持たないロールを一つ作成して割り当てます。

すでにあるコードの下に、先ほどのコードを貼り付ける。
post_slack()
```
def lambda_handler(event, context):
    # TODO implement
    # ここにpost_slack() 
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
# この下に貼り付け
```
できたらtestをクリックして、適当な名前をつけてテストを作ってください
![k_6](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_6.png)

## オプション

完成した人は、自由に触ってみてください。
・環境変数にWebhookURLを格納して利用
・username,icon,textの変更
・Lambdaの起動方法を設定　cloud watch,api gateway等

### api gatewayでLambdaを起動
https://qiita.com/baikichiz/items/2de7c4c0dcf9b051037a

### cloudwatchでLambdaを起動
Designerからtriggerを追加をクリック
![k_7](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_7.png)


rate(5 minutes)をschedule expressionに設定 -> 5分ごとに起動するようになる
![k_8](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_8.png)


# お片づけ
## slackのお片づけ
slackを開き、右のサイドバーにあるAppsをクリック。
自分のアプリの設定を開き Remove app をクリック。

## lambdaのお片づけ
lamdbaコンソールを開き。自分の作った関数をクリック。
create functionの隣にあるActionsから、deleteを選び、削除する。
![k_9](https://github.com/MoritaDaichi/8_31_jaws_kochi/blob/master/k_9.png)

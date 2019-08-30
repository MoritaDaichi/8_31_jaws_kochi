# JAWS-UG 高知 2019/8/31 サーバレスハンズオン
ラムダの説明等は別のslideで行う -> slide shareのurlを貼っておくとよし




# 前提
slackのワークスペースに入ってください。 下のリンクから
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

### B.チャンネルを選択
generalを選んで作成

### c.Webhook URLをコピー
下のような画像のURLがでてくるので、それをメモ帳にコピーしておくか、slack URLを
画像5

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
画像6

## オプション

完成した人は、自由に触ってみてください。
・username,icon,textの変更
・Lambdaの起動方法を設定　cloud watch,api gateway等

### cloudwatchでLambdaを起動
Designerからtriggerを追加をクリック
画像7



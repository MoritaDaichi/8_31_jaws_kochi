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
1.Incoming Webhookアプリを登録 (設定を追加)
2.チャンネルを選択
3.Webhook URLをコピー
(名前、アイコンなどをカスタマイズ)



slack + lambda
  + testで実行
  + cloud watchで実行 -> 毎朝投稿できるよね
  + api gatewayで実行 + dynamodb
  + 

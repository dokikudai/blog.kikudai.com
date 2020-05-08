+++
date = 2020-05-07T15:00:00Z
draft = true
tags = ["sns", "s3", "aws"]
title = "aws 初回登録（サインアップ）後のtodo（その2）"

+++
## サマリー

1. 請求関連の設定
   * コストと使用状況レポートの作成
   * 請求アラームの作成（請求額が〇〇以上になったらアラート）
2. MFAからU2Fに切替

## 1. 請求関連の設定

### コストと使用状況レポートの作成

[コストと使用状況レポート の作成 - コストと使用状況レポート](https://docs.aws.amazon.com/ja_jp/cur/latest/userguide/cur-create.html) に従い `コストと使用状況レポート` を作成します。  
途中、レポート保存用のS3バケットを作成する必要があります。  
バケット名の命名規則は、[弊社で使っているAWSリソースの命名規則を紹介します | Developers.IO](https://dev.classmethod.jp/articles/aws-name-rule/) が参考になります。  
例えば今回のバケット名は `bill-prod-log-[アカウントID]` です。

### 請求アラームの作成（請求額が〇〇以上になったらアラート）

[AWS の予想請求額をモニタリングする請求アラームの作成 - Amazon CloudWatch](https://docs.aws.amazon.com/ja_jp/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html#turning_on_billing_metrics) に従い `請求アラーム（請求額が〇〇以上になったらアラート）` を作成します。  
途中、SNSトピックを作成する必要があります。  
ここでは `billing-prod-report-sns` としてみました。  
SNSの認証メールが配信されるのでメール内の `Confirm subscription` リンクを押して認証します。

## 2. MFAからU2Fに切替
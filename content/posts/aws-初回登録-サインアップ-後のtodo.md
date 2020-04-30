+++
date = 2020-04-29T15:00:00Z
draft = true
tags = ["aws"]
title = "aws 初回登録（サインアップ）後のtodo"

+++
## サマリー

新規 aws アカウント 作成後の対応をまとめました。

基本的に aws ドキュメントの [IAM のベストプラクティス](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html#create-iam-users) の実施になりますが、それ以外で必要となった作業も追記しました。

1. ルートユーザーのセキュリティ対策
2. IAM ユーザー/ロールによる請求情報へのアクセス有効化
3. アカウントIDのエイリアス化
4. Administrator ユーザー作成し、以後 Administrator で作業
5. 強制MFAポリシーの作成とアタッチ
6. FinanceManager ユーザーの作成

## この記事の対象者

AWS 認定ソリューションアーキテクト – アソシエイト 程度の知識を持っている方であれば、これらの作業は問題なく可能です。

IAMのユーザー、グループ、ロール、ポリシーの基本的な理解が必要になります。

## 1. ルートユーザーのセキュリティ対策

[IAM のベストプラクティス - AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html#create-iam-users) の実施
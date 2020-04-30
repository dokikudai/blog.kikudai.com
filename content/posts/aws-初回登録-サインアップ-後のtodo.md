+++
date = 2020-04-29T15:00:00Z
draft = true
tags = ["aws"]
title = "aws 初回登録（サインアップ）後のtodo"

+++
## サマリー

新規 aws アカウント 作成後の対応をまとめました。

基本的に aws ドキュメントの [IAM のベストプラクティス - AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html#create-iam-users) の実施になりますが、それ以外で必要となった作業も追記しました。

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

### 15桁以上のパスワード

以下を参考にルートユーザーのログインパスワードを15桁以上にします。

[パスワードは複雑さより長さが大切 - Google 検索](https://www.google.com/search?q=%E3%83%91%E3%82%B9%E3%83%AF%E3%83%BC%E3%83%89%E3%81%AF%E8%A4%87%E9%9B%91%E3%81%95%E3%82%88%E3%82%8A%E9%95%B7%E3%81%95%E3%81%8C%E5%A4%A7%E5%88%87)

以前は Lastpass というパスワード管理アプリを利用していましたが、最近 Bitwarden を利用しています。はやくパスワードに変わる認証方式になってほしいです。

[Bitwarden - Google 検索](https://www.google.com/search?sxsrf=ALeKk00FMXFghXpGvtTSZfaxnj2PqOxeQA%3A1588260315551&ei=2-2qXvuqIZLVmAW7sKmQBA&q=Bitwarden&oq=Bitwarden&gs_lcp=CgZwc3ktYWIQAzIECCMQJzIECCMQJzICCAAyAggAMgIIADICCAAyAggAMgIIADoECAAQR1D4U1j4U2D8V2gAcAJ4AIABWogBWpIBATGYAQCgAQKgAQGqAQdnd3Mtd2l6&sclient=psy-ab&ved=0ahUKEwi7hK3fupDpAhWSKqYKHTtYCkIQ4dUDCAw&uact=5)

さくっと15桁以上のパスワード発行したい場合は、こちらのコマンドをどうぞ。（git bashなどの利用にて）

    cat /dev/urandom | LC_CTYPE=C tr -dc '\[:alnum:\]' | head -c 16

### MFAの有効化

[MFA の有効化](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html#enable-mfa-for-privileged-users) の通りです。

できれば、U2Fデバイスの有効化としたいところです。

## 2. IAM ユーザー/ロールによる請求情報へのアクセス有効化

[アクセス許可の管理の概要 - AWS 請求情報とコスト管理](https://docs.aws.amazon.com/ja_jp/awsaccountbilling/latest/aboutv2/control-access-billing.html#ControllingAccessWebsite-Activate) に従って、ルートユーザー以外でも請求情報を管理できるようにします。
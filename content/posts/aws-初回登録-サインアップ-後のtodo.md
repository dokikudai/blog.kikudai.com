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
3. Administrator ユーザー作成し、以後 Administrator で作業
4. アカウントIDのエイリアス化
5. 強制MFAポリシーの作成とアタッチ
6. FinanceManager ユーザーの作成

## この記事の対象者

AWS 認定ソリューションアーキテクト – アソシエイト 程度の知識を持っている方であれば、これらの作業は問題なく可能です。

IAMのユーザー、グループ、ロール、ポリシーの基本的な理解が必要になります。

## 1. ルートユーザーのセキュリティ対策

### 15桁以上のパスワード

以下を参考にルートユーザーのログインパスワードを15桁以上に。

[パスワードは複雑さより長さが大切 - Google 検索](https://www.google.com/search?q=%E3%83%91%E3%82%B9%E3%83%AF%E3%83%BC%E3%83%89%E3%81%AF%E8%A4%87%E9%9B%91%E3%81%95%E3%82%88%E3%82%8A%E9%95%B7%E3%81%95%E3%81%8C%E5%A4%A7%E5%88%87)

以前は Lastpass というパスワード管理アプリを利用していましたが、最近 Bitwarden を利用しています。はやくパスワードに変わる認証方式になってほしいです。

[Bitwarden - Google 検索](https://www.google.com/search?sxsrf=ALeKk00FMXFghXpGvtTSZfaxnj2PqOxeQA%3A1588260315551&ei=2-2qXvuqIZLVmAW7sKmQBA&q=Bitwarden&oq=Bitwarden&gs_lcp=CgZwc3ktYWIQAzIECCMQJzIECCMQJzICCAAyAggAMgIIADICCAAyAggAMgIIADoECAAQR1D4U1j4U2D8V2gAcAJ4AIABWogBWpIBATGYAQCgAQKgAQGqAQdnd3Mtd2l6&sclient=psy-ab&ved=0ahUKEwi7hK3fupDpAhWSKqYKHTtYCkIQ4dUDCAw&uact=5)

さくっと15桁以上のパスワード発行したい場合は、こちらのコマンドをどうぞ。（git bashなど利用）

    cat /dev/urandom | LC_CTYPE=C tr -dc '\[:alnum:\]' | head -c 16

### MFAの有効化

[MFA の有効化](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html#enable-mfa-for-privileged-users) を実施しました。

できれば、U2Fデバイスの有効化としたいところです。

## 2. IAM ユーザー/ロールによる請求情報へのアクセス有効化

[アクセス許可の管理の概要 - AWS 請求情報とコスト管理](https://docs.aws.amazon.com/ja_jp/awsaccountbilling/latest/aboutv2/control-access-billing.html#ControllingAccessWebsite-Activate) に従って、ルートユーザー以外でも請求情報を管理できる設定をしました。

## 3. Administrator ユーザー作成し、以後 Administrator で作業

以降はルートユーザ以外で作業可能なため、 [IAM のベストプラクティス - AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html#create-iam-users) の `個々の IAM ユーザーの作成` を実施しました。

[最初の IAM 管理者のユーザーおよびグループの作成 - AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/getting-started_create-admin-group.html) を参考に Administrator ユーザーを作成。

このとき、改めてグループ、ロール、ポリシー等を [\[AWS\]管理ポリシーとインラインポリシーの違いが分からなかったので改めてIAMポリシーのお勉強をする - Qiita](https://qiita.com/Batchi/items/a2dde3d2df27568cc078) で復習しました。

また、ユーザー作成時のタグ付ですが、タグの効果的な使い方が今ひとつだったので、 ぐぐって眺めてみたところ [Tagの効能 | AWSコスト削減・IaaSインフラ構築のシンプライン株式会社](https://www.simpline.co.jp/tech/tag%E3%81%AE%E5%8A%B9%E8%83%BD/) が一番しっくりきました。

とりあえず、Administrator ユーザーのタグ（タグ名=値）として、

* email=メアド（Administratorに割り当てるメアド）
* todo=https://trello.com/c/.../.

の2つを設定。

todoのがtrelloなのは、これらの作業をtrelloで管理していたためです。

## 4. アカウントIDのエイリアス化

[AWS アカウント ID とその別名 - AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/console_account-alias.html#CreateAccountAlias) で、 `AWS アカウントの別名の作成` を実施しました。

これでコンソールログインのとき、アカウント ID (12 桁) はなく、アカウントエイリアスでログインできるようになります。

## 5. 強制MFAポリシーの作成とアタッチ

[チュートリアル: ユーザーが自分の認証情報および MFA を設定できるようにする - AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/tutorial_users-self-manage-mfa-and-creds.html) を参考に `強制MFAポリシー` を作成します。

早速このポリシーを Administrator ユーザーにアタッチ。

アタッチ後、 IAM 以外のサービスにアクセスすると、エラーでサービスが利用できなくなります。 IAM にて Administrator ユーザーの [MFA の有効化](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html#enable-mfa-for-privileged-users) を実施しました。

Administrator ユーザーでログインし直すと、 IAM 以外のサービスも利用出来るようになりました。

## 6. FinanceManager ユーザーの作成

[チュートリアル: ユーザーが自分の認証情報および MFA を設定できるようにする - AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/tutorial_users-self-manage-mfa-and-creds.html) に従って請求情報を管理する FinanceManager ユーザーを作成しました。
---
title: '[Amazon Honeycode] NoCodeで出社管理アプリを開発してみた'
date: 2020-09-30 01:17:07
categories:
- NoCode(LowCode)
- "Amazon Honeycode"
tags:
- "Amazon Honycode"
- NoCode
- LowCode
- AWS
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/94332581-6db5b100-0011-11eb-9989-a46b4db98f47.png
---

Amazonが最近発表した[Amazon Honeycode](https://builder.honeycode.aws/auth/login)ベータ版の使い方の解説記事。
Excelやスプレッドシートによる業務運用をNoCodeでパッとアプリ化してみました。AWSの開発者ブログの記事を参考にしています。

<!--toc-->

## 概要
### Amazon Honeycodeとは？
- AmazonのNoCodeサービス
    - コーディング無しでビジネスアプリケーションを実現するツール
    - 非エンジニアでも簡単に使えます
    - エンジニアも開発前のプロトタイピングに利用して、仕様を精査してから本格的な開発に入ることができます
- モバイル対応のWeb アプリケーションを構築可能
    - Webブラウザ/iOS/Android 用のネイティブアプリから利用

### 機能
主な機能は３つ
- Table: データの定義
- Builder: アプリのUIを定義
- Automations: 自動アクションの規定

#### Table
- 行列を持つデータを格納する機能
    - スプレッドシートと大体同じ

![Amazon Honeycode Table](https://user-images.githubusercontent.com/68212997/94331269-2544c600-0006-11eb-802d-3f745e5a9b2b.png)

- できること
    - データのフィルタリング
    - データの書式設定
    - 他のTableのデータ参照

#### Builder
- アプリのUIをカスタマイズする機能
    - ボタンクリック時の処理や画面遷移などを定義

![Amazon Honeycode Table](https://user-images.githubusercontent.com/68212997/94331328-7b196e00-0006-11eb-981f-d861af2b1dd7.png)


#### Automations
- 自動的にアクションを実行させる機能
    - Trigger
        - スケジュールされた日時やデータが追加・変更されたタイミング
    - Actions
        - 通知の送信やデータ操作

![Amazon Honeycode Automations](https://user-images.githubusercontent.com/68212997/94331387-d186ac80-0006-11eb-9180-11dee88482e7.png)


## 開発手順
- 今回作るアプリ
    - シンプルな承認アプリ
        - Excel, スプレッドシートの台帳管理で失う無駄な工数を削減します
    - 従業員が出社や社外訪問をする際に、マネージャーへ申請するケースを想定します

### 開発の流れ
1. サンプルデータ(csvファイル)を準備
2. Honeycodeへサンプルデータをインポート
3. サンプルデータを元にデータモデルを作成
4. アプリケーションを作る (ウィザード編)
5. アプリケーションを作る (Builder編)
6. 承認フローを定義


### 1. サンプルデータ(csvファイル)を準備
まずは出社申請業務に必要なデータをCSV形式で用意。データ件数は少量でも構わないが、選択式で入力を行うマスタデータ項目を網羅しておくと、後のデータモデル作成が楽になる。

- [CSVとは？（非エンジニア向け）](https://shop-pro.jp/yomyom-colorme/66698)
    - comma separated valuesの略称
    - ,でデータを区切ったファイル形式
    - Excelや[googleスプレッドシート](https://www.google.com/intl/ja_jp/sheets/about/)にもcsv形式で出力という機能がついています

1. 以下の様なサンプルデータを作成
    - 作り方はスプレッドシートでもExcelでもメモ帳でもOK

<img width="1316" alt="sampledata_googleスプレッドシート" src="https://user-images.githubusercontent.com/68212997/94337716-a95b7580-0027-11eb-9ec1-09b1d327ba9f.png">

- サンプルデータの形式は以下のように設定

|列名|論理名|
|--|--|
|application date|申請日|
|applicant|申請者|
|work day|出社日|
|destination|行き先|
|reason|理由|
|manager|承認者|
|result|承認結果|



2. CSVを出力
- スプレッドシートの例

<img width="1266" alt="スプレッドシートcsv形式でダウンロード" src="https://user-images.githubusercontent.com/68212997/94332007-3264b380-000c-11eb-884f-85178bc1701c.png">


ここで用意した.csv形式のファイルを、後でHoneycodeに読み込ませます


### 2. Honeycodeへサンプルデータをインポート
#### ログイン/アカウント作成
- [Amazon Honeycode](https://builder.honeycode.aws/auth/login)にログイン
    
<img width="1266" alt="Amazon Honeycode login" src="https://user-images.githubusercontent.com/68212997/94332395-d439cf80-000f-11eb-9517-bb7fa3b4173d.png">

- 未登録であればCreate Oneから作成
    - 確認メールが送信されます

![Amazon Honeycode Create Account](https://user-images.githubusercontent.com/68212997/94332412-077c5e80-0010-11eb-83b5-3e320ace2bb9.png)

- メールのConfirm Nowより承認
    - 続けてログインを実施
<img width="1195" alt="Amazon Honeycode 承認画面" src="https://user-images.githubusercontent.com/68212997/94332500-bb7de980-0010-11eb-9ab5-3c368262d113.png">

- 初めてログインするとこんな感じ

<img width="1666" alt="Amazon Honeycode 初ログイン" src="https://user-images.githubusercontent.com/68212997/94332526-013ab200-0011-11eb-8bf6-986a6cd0764b.png">


#### サンプルデータのインポート
- 右上のCreate workbook, IMportCSV fileからインポートを押下
    - 先ほど用意したcsvファイルを選択してImportを実施

![Amazon Honeycode サンプルデータのインポート](https://user-images.githubusercontent.com/68212997/94333212-3bf11a00-0012-11eb-8aa7-5b3dec0747e3.png)

- import後にTableの画面になります

<img width="1432" alt="Amazon Honeycodeファイルimport後" src="https://user-images.githubusercontent.com/68212997/94338114-d8271b00-002a-11eb-8407-e7803535e9ed.png">

### 3. サンプルデータを元にデータモデルを作成

- Table機能を使ってデータモデルを作ります

#### WorkbookとTable名を変更
- ・・・アイコンから変更

![WorkbookとTable名の変更](https://user-images.githubusercontent.com/68212997/94340990-fef04c00-0040-11eb-96c2-3dc3e495aa87.png)

- 今回は以下の様に設定
    - Workbook
        - attendance management
    - Table名
        - attendance

#### 列の書式設定
- 日付であればDateなどのデータの型を定義することで、不正な入力を防ぐことができる

- データ型の変更方法
    - 列を選択
    - 画面上部の Formatsを押下
    - COLUMN FORMAT を指定
    - Applyで適用

![Amazon Honeycode 列の書式設定](https://user-images.githubusercontent.com/68212997/94341572-627c7880-0045-11eb-869f-c9634afcd20c.png)

- 今回は以下の様に設定
    - 例えば、Date型にすると”年/月/日”の形式のデータ以外は入力できなくなります
    

|列名|COLUMNFORMAT|
|--|--|
|application date|Date|
|applicant|Contact|
|work day|Date|
|Manager|Contact|

- 日付を入力する列にDate、人名を入力する列にContactを設定しました

#### 選択式の入力にする（マスターテーブルの作成）

- 残ったdestination列と result列を選択式の入力にするために、マスターテーブルを作成します。

- 画面上部の Wizardsを押下
    - 表示されたペインで Create Picklistsを選択

![Amazon Honeycode Wizards](https://user-images.githubusercontent.com/68212997/94354992-1c61fc00-00bb-11eb-9c79-cdedc1e6cdd0.png)

- Create picklists for:
    - Tableを選択(今回はattendance)
- For unique values in:
    - 列を選択
    - 今回はdestination
- Apply

![image](https://user-images.githubusercontent.com/68212997/94356088-546e3c80-00c5-11eb-82a1-d5313865835a.png)

- ＋Add Newからresult列についても同様に設定
- GOを押下

![Amazon Honeycode Create Picklists](https://user-images.githubusercontent.com/68212997/94356290-a9ab4d80-00c7-11eb-912d-a77e23e571fc.png)

- 設定後は以下の様になります

<img width="1026" alt="Amazon Honeycode マスターテーブル作成後" src="https://user-images.githubusercontent.com/68212997/94356341-4110a080-00c8-11eb-901c-501600dc492a.png">


- サンプルデータでマスタデータが網羅されていない場合は、この段階でマスターテーブルに対してデータを追加
    - つまり他の選択肢もある場合は追加が必要
- データの追加はスプレッドシート左下の「＋」から実施

データモデルの作成は以上でOK

### 4. アプリ(UI)の開発 App Wizardによる自動生成
次はアプリのUI（画面）を作ります

#### UIの作り方
- Amazon HoneycodeにおけるUIの作り方
    - Builder
        - 真っ白な画面にデータ項目やボタンを1つずつ配置しながら画面を構成
    - App Wizard
        - 自分で一から作らず、テンプレを利用

今回はウィザードで基礎を作り、詳細をBuilderで改修する流れで構築

#### ウィザードの利用
- ウィザードを起動
    - 画面上部の Build appを押下
    - Use app wizardを選択

![Amazon Honeycode use wizard](https://user-images.githubusercontent.com/68212997/94554933-c5644e80-0295-11eb-8c0a-913e23dfdffc.png)


- App Wizardを開くと表示される説明ムービー

![About App Movie](https://user-images.githubusercontent.com/68212997/94554933-c5644e80-0295-11eb-8c0a-913e23dfdffc.png)


- App Wizaradを利用する場合の開発手順
    0. データモデルを読み込み、そこからAP画面を自動生成
    1. 一覧画面
    2. 詳細画面
    3. フォームを編集

#### Step1: Tableの選択
事前に作成したデータモデルを一覧表示する画面を自動生成します

<img width="1485" alt="Amazon Honeycode App Wizard Step1" src="https://user-images.githubusercontent.com/68212997/94556014-6d2e4c00-0297-11eb-8f7d-08ec6ceafe5e.png">

1. 右のサイドナビのSourceを選択
    - 今回はattendance(事前に作成したデータTable名)
    - 以下の様にアプリの画面（データの一覧画面）が自動生成されます

    <img width="1345" alt="Amazon Honeycode App Wizard自動生成したUI" src="https://user-images.githubusercontent.com/68212997/94557862-1a09c880-029a-11eb-86fc-06b9b8d12bc0.png">


#### Step3: 画面やデータ設定を編集
<img width="926" alt="Amazon Honeycode App Wizard Step3" src="https://user-images.githubusercontent.com/68212997/94556404-f80f4680-0297-11eb-9e96-4e0902586bbf.png">


2. 一覧画面のデザイン
    - 必要のないデータ項目を消します
    - データ項目の削除
        - Xボタンで削除
    - データ項目の追加
        - ＋Add column
    - データ順序の入れかえ
        - データ項目左側のドットをドラッグ
    - 設定が終わったらNext

![Amazon Honeycode 一覧画面のデザイン](https://user-images.githubusercontent.com/68212997/94557057-0316a680-0299-11eb-9710-ee1121a70cfd.png)


3. 単票画面（詳細画面）のデザイン
    - 先ほどの行単位の話
        - 今回は単一の申請の詳細をみる画面
        - 一覧画面で表示されないデータ項目もすべて表示したい
    - データ項目を変更可能にする
        - 鉛筆アイコンをクリック
        - 今回はマネージャーによる承認行為を行うので result を変更可能に
    - 設定が終了後にNext

![Amazon Honeycode App Wizard make detail](https://user-images.githubusercontent.com/68212997/94559605-740b8d80-029c-11eb-9233-bc0288067e45.png)


4. フォーム画面のデザイン
    - 申請データを登録する際の画面
    - 今回は申請者がresultを決定する必要がないため、Xで削除
    - 完了したらApply

![Amazon Honeycode App Wzard designe form](https://user-images.githubusercontent.com/68212997/94560443-a79ae780-029d-11eb-94ce-f3105e16f975.png)

ここまでで基礎となるアプリケーションが完成した状態。画面右上の X からウィザードを終了。終了すると、残りはBuilderでの編集しかできないので注意（App Wizardは使えない）

--------------------

### 5. アプリの開発　Builderによるカスタマイズ
App Wizardで作ったアプリの詳細をBuilderで編集する

- Builder の起動
    - 画面左上の Builder アイコンを押下
    - アプリを選択
        - 今回作ったアプリ(Attendance)を選択

![Amazon Honeycode Start Builder](https://user-images.githubusercontent.com/68212997/94562266-14af7c80-02a0-11eb-81d8-da5cea6e4577.png)

- Builderの立ち上げ画面

![Amazon Honeycode Builder](https://user-images.githubusercontent.com/68212997/94562839-cea6e880-02a0-11eb-8d8a-4f74d05c612d.png)



#### Builderの概要
- Screen（アプリの画面）に各Object(部品)を配置していく
    - Ex.) データを表示する部品, 処理の起点となるボタン

![Amazon Honeycode Builder Add object](https://user-images.githubusercontent.com/68212997/94562597-7cfe5e00-02a0-11eb-84e9-e36d9522ea93.png)

#### チュートリアル1: Screen
- ここでデータの追加やアプリ画面のスタイル、レイアウトのアレンジなどを行う

<img width="1470" alt="Amazon Honeycode About Screen" src="https://user-images.githubusercontent.com/68212997/94563302-7f14ec80-02a1-11eb-904d-a807a3365435.png">


#### チュートリアル２: Properties
- ソース、表示、アクションなどのデータプロパティを設定する

<img width="1470" alt="Amazon Honeycode Configure & edit" src="https://user-images.githubusercontent.com/68212997/94563302-7f14ec80-02a1-11eb-904d-a807a3365435.png">


#### チュートリアル３: The data view
- ソースデータを視覚的に参照

<img width="1654" alt="Amazon Honeycode data view" src="https://user-images.githubusercontent.com/68212997/94563866-4295c080-02a2-11eb-8a2f-5b50a432f208.png">

---------------

### 不要な機能の除去
#### 詳細画面
- 今回単票画面(詳細画面)に含まれる不要な機能を除去する

1. 編集したいアプリの画面を移動
    - 今回弄るのは詳細画面(attendance detail)
        - 画面左側のペイン Screens で単票画面である attendance detail を選択

2. 画面中央のペインで Jump to item 機能を含む Block を選択

3. 画面右側のペイン BLOCK PROPERTIES で DISPLAY タブを選択

4. Set visibility に =FALSE と入力

![attendance detailの編集](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-21.f4b0d0472a106066c4660a558af6a54015662a6e.png)


#### データフォームの修正
申請者が申請日を編集する必要が無い（自動で決定する為）ため、機能を削除します


1. 画面左側のペイン Screens で単票画面である attendance form を選択

2. 画面中央のペインで application date のデータ値を入力する Data Cell を選択


3. 画面右側のペイン DATA CELL PROPERTIES で Editable チェックを Off にする

4. 画面右側のペインで DATA タブ、Set type で Variable を選択する

5. Set initial value に =NOW()+9/24 と入力する
    - Amazon Honeycode では現在時刻を示す NOW() や今日を示す TODAY() などの関数と数式を利用可能です。NOW() 関数、TODAY() 関数のいずれも UTC を基準としているため、JST への変換を行っている

-------
- 申請者を記録するapplicantも修正
- 代理申請を除くと申請者は操作を行ったユーザーと判断できるため、以下の手順に沿って申請者の初期値にログインユーザーを設定するとともに、利用者に入力をさせない仕様とする


1. 画面中央のペインで applicant のデータ値を入力する Data Cell を選択

2. 画面右側のペイン DATA CELL PROPERTIES で Editable チェックを Off に変更
    - 自動入力させる為、編集不能にする

3. 画面右側のペインで DATA タブ、Set type で Variable を選択

4. Set initial value に =$[SYS_USER] と入力
    - 自動的にユーザ名を入れる

手直しは以上で完了

### 6. 承認フローを定義(Automationsで機能を開発)
- Automationsでメール送信機能を開発する
    - 本アプリの利用者
        1. 申請者（出社する社員）
        2. 承認者（マネージャー）
    - 上記の間をつなぐフローに必要な以下機能を作る

- 今回作る機能
    1. 申請者が出社申請を登時、マネージャーへ承認依頼メールを送信
    2. マネージャーの承認実施時、申請者へ結果通知メールを送信

### Automations
- Automationsを起動
    - 左のAutomationsアイコン → ＋
![Amazon Honeycode Automations 起動](https://user-images.githubusercontent.com/68212997/94564794-60175a00-02a3-11eb-89ba-03a5a1cf1a18.png)


#### まずは承認依頼メールを発信する機能を作る

- Automationsの名称を変更
    - 上部のスリードット
    - 今回はapproval requestと命名
    ![Amazon Honeycode Change Automation Name](https://user-images.githubusercontent.com/68212997/94565715-838ed480-02a4-11eb-8e6c-106061a04417.png)


- 処理が発生するタイミングを指定(Row Added)
    - Row Added or Deleted
    - In table
        - 申請データを格納するattendance を選択
    - Starts when
        - row is added to を選択
    - 今回のタイミング
        - 申請者による出社申請を起点

    ![Amazon Honeycode Row Added](https://user-images.githubusercontent.com/68212997/94566403-40813100-02a5-11eb-9b2d-e0a8e76ea7e9.png)

テーブルの行が追加されるとき＝承認依頼を実施した時


- 処理内容を定義(Add Actions)
    - Add actions 
    - Notify(通知機能)を選択
    ![Amazon Honeycode Add Actions](https://user-images.githubusercontent.com/68212997/94566735-a5d52200-02a5-11eb-86d7-3764aeca3a7f.png)
    - 送信先、件名、メッセージを入力
        - データ項目名も指定可能
        - データ項目名を指定した箇所は処理対象のデータ値に置換された上でメールが送信されるため、宛先や文面を動的に構成可能
        - 今回は”=manager”を指定

- 承認依頼メールを定義
    - 線が引かれている箇所は =manager との形式でデータ項目名を指定している
    - 表記のデータ値はあくまで一例であり、送信されるメールの文面は Automations で処理されるデータの値を用いて構成される

    ![Amazon Honeycode 承認依頼メール](https://user-images.githubusercontent.com/68212997/94567825-d5385e80-02a6-11eb-9621-289bdca0cd33.png)

=データ名でデータモデルから参照してくれる
緑のラインが入っている箇所


- 変更を確定
    - 画面右上のPublishを押下
    - Publish されていない、つまりは変更が加えられている状態では画面右上に Draft と表示される

![Publish Automation](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-30.5a1798cd9d49eaeb4088f8ae7067ab55069ef53a.png)

- Notifyで以下のような警告が表示される場合がある

![Amazon Honeycode notification alert](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-31.664a64c8daa5a674b29e2abf277e21f71c712c1b.png)



#### 申請者への結果通知メール送信機能を作成
- Automationを追加
    - 画面左側の ＋ をクリックして画面を起動

![add automation](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-32.1d0540b2722023bdb2d3e145a2020a0e692f89f9.png)

- automation Trigger
    - 処理が発生するタイミングは承認者による承認結果登録を起点とする
    - Column Changes を選択
    - In table
        - 申請データを格納する attendance を選択
    - Starts when this column changes
        - 承認結果データである `=[result]` を選択
    - Run automation if this formula is TRUE
        - 承認結果の入力を示す式 =`[result]<>""` を入力

![Set Automation Trigger](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-33.cfa4ef36cf895b098feb641fc6536f3fb0487bc6.png)



- 結果通知メールを定義

```
=applicant　様

以下の出社申請への承認結果をご連絡致します。

申請日：=
申請者：
出社希望日：
出社先：
出社理由：

承認者：
承認結果：

本メールは、配信専用のアドレスで配信されています。
本メールに返信されても、返信内容の確認およびご返信ができません。
あらかじめご了承ください。

```

![Amazon Honeycode 申請メール例](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-35.e4518be1e7285ab66d659659e51a0e0359b5a5bf.png)

- Publishで編集を完了

ここまででアプリの開発は完了
次に関係者にアプリを公開する


### アプリを公開

#### Teamにユーザを招待
Honeycodeで開発したアプリを使ってもらうには、事前に以下が必要

- 利用者にAmazon Honeycodeのアカウントを作成してもらう
- 同じTeamに入ってもらう

- 招待の手順
    - 画面左下の Teams アイコンをクリック
    - チーム名をクリック
    - 画面右上の Add team member をクリック

![Amazon Honeycode Teamへの招待](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-39.baa88c65c4121533e2b5d7972cd88717280815f8.png)


- Connect AWS account
    - 有料プランにすればAWSのアカウントとも連携可能なよう
    ```
    To upgrade your team plan, connect to your AWS account.
    ```

![Amazon Honeycode add member](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-39.baa88c65c4121533e2b5d7972cd88717280815f8.png)


- Invite to join
    - 表示された画面でメンバーのメールアドレスを入力
    - 役割を Admin と Member のいずれかから選択
    - Invite ボタンをクリック
    - 役割の違い
        - Admin : Workbook、アプリケーション、Automations の作成。Plan とチームメンバーの管理
        - Member : Workbook、アプリケーション、Automations の作成


![Invite to join](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-40.d2695faff24f5fc0f21ffb8d94b0d75e1a3e5803.png)


- 招待を受けたユーザーには以下のようなメールが送信される
    - Join ボタンをクリックしてもらう

![Amazon Honeycode invite mail](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-41.1de35eca9514a24ed0a760d06e059af8cf6c316b.png)


事前の登録作業は以上で完了。
一度Honeycodeのアカウントを登録すれば他のアプリを作っても使いまわせるが少し手間ではある印象


#### share
作成したアプリケーションの公開は関係者に Shareする

- 画面左上の Amazon Honeycode アイコンをクリック
- Workbook とアプリケーションの一覧を表示 
- 公開したいアプリケーション右端のスリードットの左側をカーソルでポイント
    - Share ボタンが表示されるのでクリック

![Amazon Honeycode 作成したアプリを公開](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-36.f33cf32a94c7f6e19f82391abc1d9f3b8800b549.png)



- 公開先のユーザアカウントを選択する
    - ユーザー名やメールアドレスで検索
    - リストに追加
    - Update ボタンをクリック

![Amazon Honeycode Share App](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-37.c78be716f6213589013e0b58f24df002728b6fd5.png)

- 利用可能になった旨がメールで通知される

![Amazon Honeycode 利用通知メール](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-38.6677a11861696040e27565cd2e1a07d9331fbe4d.png)



--------
## アプリを利用してみる

### スマホで利用
- まずはアプリケーションの公開通知を受け取ったユーザーがスマートフォンから出社申請をする

- Amazon honeycode アプリをインストール
    - [iOS](https://apps.apple.com/jp/app/amazon-honeycode/id1502619411)
    - [Android](https://play.google.com/store/apps/details?id=com.amazon.aws.honeycode&hl=ja)

- ログイン
    - アカウントを持っていなければここで作成

- アカウント作成時に入力したメールアドレスとパスワードでログイン
    - 自分に公開されているアプリケーションの一覧が表示される
    - 今回作成したアプリケーションの Attendance をタップして起動

![Ammazon Honeycode login画面](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-43.cc8ec316ab24eb4201ce1b5d81281cceba3e95a5.png)

- アプリケーションが起動すると、ホーム画面の申請一覧が表示される
    - データモデルを作成する際にインポートしたサンプルデータを削除していない場合はリストに表示される

![ホーム画面の申請一覧](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-44.8660bf2217c6813af6a0315185745a619a2234a7.png)


- 画面右上の + Add をタップして出社申請を行う
![+Add](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-44.8660bf2217c6813af6a0315185745a619a2234a7.png)

- 出社申請のエントリー画面で入力可能な項目を埋めていく

- データモデルの作成をする際に各データ項目の formats を定めた。アプリケーションでもその結果が踏襲され、データに合わせた入力が可能

- データ入力が完了したら Done ボタンをクリックし、入力を確定

![](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-49.8f43f03e30b0f2449d4f99d3effeeb5177bff773.png)

- 申請一覧画面に遷移し、直前の操作で作成した申請が一覧に表示される

![](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-50.dfd2290c5aaa2f089365cb474e072ef127704c4d.png)


- 申請データの登録に合わせて Automations が実行され、マネージャーにはこのようなメールが送信される

![](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-51.f9c835850b9b85c9dd4eea6c2ad784a89b11b361.png)


### Web ブラウザで申請を承認
- 続いては承認依頼通知を受け取ったマネージャーが出社申請を承認する
- Amazon Honeycode は Webブラウザからも利用可能なため、マネージャーはブラウザから承認を行ってみる
- Chrome、Edge、Firefox、Safari などの Web ブラウザで Amazon Honeycode にアクセス

- 今回作成したアプリケーションの Attendance をクリックして起動

![](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-52.4280892d08c050a2ebfc5558cc8fc929e17606c5.png)


- アプリが起動すると、ホーム画面の申請一覧が表示される
    - 承認結果が空白のデータをクリックし、出社申請の承認を実施

![](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-53.e9c69922cb736196617488765db08b57927b0c38.png)


- 出社申請の詳細画面が表示される
    - 内容を確認して承認結果を選択

![](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-54.cfd1dfef4d7df89ff56e1d23383712155745181d.png)



- 選択すると自動的にデータが保存され、申請者にはこのようなメールが送信された

![](https://d1.awsstatic.com/Developer%20Marketing/jp/magazine/2020/img_honeycode-attendance-management-55.d0c6c9a1de06dd78db8e21b65c7ade5ba5c2752d.png)


無事に出社申請業務を実施できたので、これで動作確認は完了

### 所感
- iOS や Android のアプリストアから Amazon Honeycode アプリをダウンロードしてログインするだけで、利用者に必要なアプリケーションが表示され、配布の手間が少ない点はメリット
    - honeycodeへのアカウント登録とTeamへの加入は必要だが、以降はアプリを追加してもすぐにShareできる
- AWSアカウントとの連携も利用すれば、より配布の手間を削減できそう

## 参考
### その他
- [[UI Bakery] NoCodeでAngularのUIをプロトタイピング(コード出力も可能なツール)](/UI-Bakery-NoCodeでAngularのUIをプロトタイピング-コード出力も可能/)

### その他
- [aws builders.flash](https://aws.amazon.com/jp/builders-flash/202008/honeycode-attendance-management/)

---
title: '[無料RPA] Windows Power Automate インストール~Webの自動操作'
date: 2021-03-03 15:35:16
category: 
- Tool Tips
- RPA
tags:
- RPA
- Windows Power Automate
thumbnail: https://user-images.githubusercontent.com/68212997/109756485-62ebf180-7c2b-11eb-9cc7-2c8fa30b014c.png
---

- MSが提供開始したRPAツールについてのメモ
- Windows 10ユーザーは無料で使える
- Webの簡単な操作程度なら10分で誰でも覚えることができる

<!-- toc -->

## インストール
- 以下の"Download free"からインストーラーをダウンロード
    -[Power Automate](https://flow.microsoft.com/en-us/desk)

- インストーラーを起動
![Windows Power Automate Installer](https://user-images.githubusercontent.com/41946222/109744335-77bd8a80-7c15-11eb-9870-3be0db88d107.png)

- 次へ
- 使用条件にチェック/インストール
![Windows Power Automate Installer2](https://user-images.githubusercontent.com/41946222/109744657-06caa280-7c16-11eb-9443-73bd1b9a9196.png)

- 以上でインストールは完了
![Windows Power Automate Installer 3](https://user-images.githubusercontent.com/41946222/109744948-822c5400-7c16-11eb-99d8-74af149ed4f4.png)

## ブラウザ拡張機能の設定
- メインブラウザがChromeの場合は以下を入れたほうがいい
![image](https://user-images.githubusercontent.com/41946222/109745219-e222fa80-7c16-11eb-8832-3b3aea0f4a0a.png)

- [Microsoft Power Automate](https://chrome.google.com/webstore/detail/microsoft-power-automate/gjgfobnenmnljakmhboildkafdkicala)
```
Power Automate を使用すると、コンピューターの手動のプロセスとタスクを自動化できます。

拡張機能をインストールすると、Web スクリプト、データ抽出、Web テスト、Web フォームへの入力、API 呼び出しなどが自動化されます。また、Power Automate Desktop の Web レコーダーも有効になります。
```

## セッティング
- Power Automate Desktopを起動
![Power Automate Desktop](https://user-images.githubusercontent.com/41946222/109745480-45ad2800-7c17-11eb-9684-656271197d06.png)

- メールアドレスを入力/Windowsアカウントを選択
![Power Automate Desktop Setting](https://user-images.githubusercontent.com/41946222/109746495-d5070b00-7c18-11eb-9115-3da87b878b6c.png)

- 国を選択/開始する
![Power Automate Desktop Setting 2](https://user-images.githubusercontent.com/41946222/109746697-27e0c280-7c19-11eb-9757-08d815e57b7a.png)

- 以上でセットアップは完了
![Power Automate Desktop Top](https://user-images.githubusercontent.com/41946222/109746813-58286100-7c19-11eb-9bea-f3fddb1134c1.png)

## 使用方法
### フローの作成
- "新しいフロー"を押下
![Power Automate Desktop Making Flow](https://user-images.githubusercontent.com/41946222/109746929-97ef4880-7c19-11eb-895d-a4ad89ff0d34.png)

- フロー名を入力して作成を押下
![Power Automate Desktop Making Flow 2](https://user-images.githubusercontent.com/41946222/109747058-d2f17c00-7c19-11eb-85b5-17d05646c48e.png)

- 後はアクションや変数を設定してワークフローを作り上げていく

### Power Automateのアクション
![Power Automate Desktop Action](https://user-images.githubusercontent.com/41946222/109747437-780c5480-7c1a-11eb-821c-b2028352b92f.png)

- ざっと見た限り他のRPAツールで可能な大抵のことができそうでした
    - AWSなどの他者クラウドの操作まで
    - 無料なので、RPAベンダーは駆逐されそう

### 使用例1: Webページの自動起動
- まずはWebページを自動で起動させてみました
- アクション/Webオートメーション/Webフォーム入力/新しいChromeを起動する
    - 各パラメータを設定後に保存

![Power Automate Desktop Chrome](https://user-images.githubusercontent.com/41946222/109748348-10ef9f80-7c1c-11eb-8786-4fd90a6c8576.png)

- 上の実行ボタンを押すことでchromeが自動起動することを確認できる

### 使用例2: ボタンの自動押下
- 以下のアクションを追加する
    - アクション/Webオートメーション/Webフォーム入力/Webページのリンクをクリックします

![image](https://user-images.githubusercontent.com/41946222/109750996-ddfbda80-7c20-11eb-9d2f-599d78a1d211.png)

- UI要素の追加を押下
    - 実際の画面から要素を選択できる
    - 今回はGithubを開いてNew Issueボタンを押すように設定した

![Power Automate Desktop Sample Flow](https://user-images.githubusercontent.com/41946222/109751439-d1c44d00-7c21-11eb-904c-76157f7f17cf.png)

## 所感
- 使用例のようにボタンを押させていくだけでも、誰でも大抵のWEBアプリの単純操作を自動化できる
    - Webアプリの簡単な動作テストもできそう(エンジニアであればテストコードを書くべきだが)

- Microsoftにしては直感的に操作可能
    - 使用例程度のフローであれば、特にドキュメントを見ずに5分もかからず作れました

- ページ上の要素からのデータ取得や、TeamsやExcel等の他のアプリとの連携、条件分岐なども使っていけば、自由度は高そう
    - スクレイピングは自分でプログラムを書くとなると面倒

- まずは会社の勤退システム(Webアプリ)の自動入力から初めてみようと思います

## 関連
- [iPad ProでWEB AP開発 & RPA](./iPad-Pro%E3%81%A7WEB-AP%E9%96%8B%E7%99%BA/)
- [Windows power Automate](https://flow.microsoft.com/ja-jp/)
---
title: Githubの Issue & Project(カンバン)でプロダクトバックログ/タスクを管理する（アジャイル開発の運用）
date: 2021-01-04 23:59:37
categories:
- Tool Tips
- Github
tags: 
- Github.com
- アジャイル
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/83426237-d200a380-a469-11ea-8dd5-6fe92939fc8c.png
---

- アジャイル開発におけるバックログ管理をGitで完結させる手法について、他人に説明するためのメモ
  - プロダクトバックログとは？
    - `ロードマップと要件に基づいて開発チームが行う作業に優先順位を設定したリスト`
    - アジャイル開発においてはタスクや検討事項を”プロダクトバックログ”として定義する(フィーチャーとも呼ぶ)
        - バックログをリスト化 ⇒ 優先度付け ⇒ 順次消化 ⇨ チームのベロシティを計測して時期計画策定 というサイクル(イテレーション/スプリント)を回す方式が一般的
  - 他のタスク管理ツールも色々試したが、個人的にはこの手法が最も効率が良いと感じた
  - Zennなどを組み合わせてバーンダーンチャート化するとより状況を可視化しやすい(Velocityを計測しやすい)

### 前提知識
- Issue
  - ここにバックログを登録していく
  - ラベルでカテゴリライズ可能
    - Ex. Question(質問), Bug(バグ), enhuncement(追加機能), Documentation(ドキュメント整備)

- Projects (カンバン)
  - Issueを一覧化するGitの機能
  - 複数リポジトリに分割して開発する場合でも、Projectを使ってissueをまとめることが可能  

これらのGitの機能を使って、プロダクトバックログの管理を実施する。
他のタスク管理ツールは使用しない。


## バックログ管理の大まかな流れ
0. Project（カンバン）を作成する
1. バックログをIssueとして登録
  - 小タスクをチェックボックス形式で設定
  - 生成したIssueはカンバン(Project)のTo Doに自動的に反映される

2. カンバンよりバックログ(Issue)一覧を確認。着手するものを選択
  - 着手を開始したIssueは、カンバン内でIn Progress columnへ移動

3. 完了したタスクにチェックを入れていく
  - Pull Request時にはIssue番号をコメントに入れる
  - そうすることでPRのClose時に自動的にIssueもDoneとなる

4. 全タスクが完了したIssueはDone columnへ移動
  - 1スプリントに幾つのIssueをDoneに持っていけるか？でチームのVelocityを計測する
  - Velocityを元に、次スプリントでどこまでやるか？を決める。アジャイルは適当に進めて良いというのは間違い  
  - 可視化ツールとの連携でバーンダーンチャート化するとより状況を可視化しやすい
    - 経験上、企業の開発環境(Github Enterprise)では使えないことが多い

### 0. Project（カンバン）を作成する
バックログ一覧(Issue一覧)を確認する画面だと考えてよい
Issueとしてバックログを登録する前に、Project(カンバン)を作成する必要がある
後に登録したIssueがこちらのTo doに自動的に表示される

![Github Project](https://user-images.githubusercontent.com/68212997/102298622-cfdcdd00-3f94-11eb-8202-2f87a956120d.png)
- github 任意のリポジトリ画面
  - Projctsタブ
  - Creaate a Project
- プロジェクト設定画面
  ![Project Setting](https://user-images.githubusercontent.com/68212997/102304187-387d8700-3fa0-11eb-9530-2d163a9bafd7.png)
  - PJ名と説明文を入力
  - Template (今回はAutomated kanban)
    - None: デフォルトのもの
    - Basic kanban
      - To do, In progress, Doneの列を持つ基本的なカンバン式ボー
    - Automated kanban
      - 各列にある課題やプルリクエストを自動的に移動させるためのトリガーが組み込まれている
    - Automated kanban with reviews
      - プルリクレビューのためのトリガーを追加したカンバン
    - Bug triage
      - To do、High priority、Low priority、Closedの各カラムでバグのトリアージと優先順位付けを行うもの
  - Create project
- 作成されたカンバン
  ![git project kanban](https://user-images.githubusercontent.com/68212997/102305259-d70ae780-3fa2-11eb-8549-f0d6602df768.png)

### 1. Issueでタスクを登録
次にIssueでタスクを登録していく。ここで作成するIssueに事前に作成したProjectを紐づけすることで、カンバンから一覧として確認可能になる。
- Issueの作成方法
  - GHE Issueタブ
  - New Issue
  - 右側の各種設定
  - コメント入力
  - Submit new issue
- タスク以外の相談事項もIssueで発行する
![Issueを活用した相談例](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2015/06/question.png)
Issue作成時に記載するコメントのテンプレートや、設定のルールをチーム内で事前に定めておく
Read.meやWikiに載せておく

### Issue作成時の設定ルール
以下を一通り設定することでタスク管理の効率化を図れる。  
後でカンバンからIssueの設定を変更することも可能。
- Assignees
  - タスクの担当者を指定する
- Labels
  - バックログをカテゴライズ可能
    - 元々主なカテゴリのラベルが用意されており、追加も可能
      - Ex. bug(バグの修正時), documentation(資料作成), enhancement（機能拡充）
    - タスクの重要度や、重さまでラベル化するかはチームで相談して決める
- Milestones
  - 期限を指定する

### Issueコメントフォーマット
Issueを生成する際に.md形式で説明を書くことができる
以下のようなフォーマットをチームで定めておくとよい

- ルール例
  - `WHY/WHATを必ず書く`
    - 根本的な内容の後戻りを防ぐため
  - タスクリスト記法で未完了/完了の変更を行う
    - 以下のように書くと、チェックボックスとして扱うことが可能になる
      - これでバックログに含まれる小タスクを表す
    ```
    - [ ] Task
    ```
- Issueコメントフォーマット例
  - バックログ毎にissueを生成する場合の例。より細かい単位でタスク毎にissueを作るチームも多い
  - 各メンバがGitに慣れていれば、タスク単位でIssueを切るとうまくいく
```
## 概要
### WHY
なぜこのIssueが必要になったのか？
### WHAT
何をするのか？
## タスク
以下の書式で書くとチェックボックスとして扱える
- [ ] Task a
- [ ] Task b
### 関連課題
---
関連する課題があればここにリンク形式で載せる（Backlog）
### 親課題
---
親の課題があればここにリンク形式で載せる（Backlog）
### 備考
---
別途記載する必要があれば書く
## Reference
参考文献があれば記載
```

- こういったフォーマットをテンプレートとしてGitに登録しておくと利便性が高まる
  - 守らないメンバが出て形骸化してしまう事態を防ぐ
  - 少なくともREADMEか、Wiki等のメンバが閲覧可能な場所に置いておく


### issueテンプレートの登録方法
先ほどの様なフォーマットを複数種登録して、issue生成時にユーザに選択させることができる
- Settingsタブ/Options/Features/issues/setup templateを押下 
![github issue Setup Templates](https://user-images.githubusercontent.com/68212997/102591360-3f99c600-4155-11eb-9d0f-72f8eac3b3cb.png)
- Add Template
  - Custom templateを選択することでチーム独自のTemplateを登録できる
  - 元々Bug ReportとFeature requestが用意されている
  - 以下のようにTemplateを追加していき、後は"Preview and edit"から用途に応じたフォーマットを用意する
    - 混乱を避けるために一種にするか、用途ごとに分けるかはチームで相談する
![github issue add template](https://user-images.githubusercontent.com/68212997/102592337-ac619000-4156-11eb-94ce-3bf53d74b54d.png)
- 元々用意されているFeature requestの中身は以下
  - これを利用すれば、機能提案を非同期コミュニケーションで行うことが可能になる
  - 開発の中で思いついた機能をミーティングの場を待たずにすぐさま提案でき、開発スピードの向上を見込める
![github issue Feature Request](https://user-images.githubusercontent.com/68212997/102592736-3dd10200-4157-11eb-89d1-5cddd5a07197.png)
- custom issue template
  - ここに独自のフォーマットを登録する
  - 設定項目は以下
    - Template name
    - About
      - tempalteの説明 
    - Template content
      - 上記のプロダクトバックログの登録用のフォーマットをここに入力
    - Optional additional items
      - Issue default title
        - デフォルトのタイトルを指定可能
      - Assignees
        - このテンプレートを活用してissueを生成する際の担当者を設定可能
        - 対応者が確定するレベルで細かくテンプレートを用視するのであれば有用
      - Labels
        - このテンプレートを活用してissueを生成する際のlabelを指定可能
        - 指定しておけばLabelのつけ忘れを回避できる
- 各Templateの編集後
  - propose changes
  - Commit changes

### Project(カンバン)の運用フロー
以下の流れで、バックログの進捗状況を可視化する
- 着手を開始したIssueをIn Progressへ移動
- 完了したタスクのチェックボックスを埋めていく
- 全タスクが完了したIssueをDoneへ移動

### Pull Request関係のルール
- Pull Request発行時には、コメントに #Issue番号 と書く
  - GitHubではPull Requestも1つのIssueとして扱われる
  - 対象Issueのコメント欄上部に、タイムライン形式でコミットが表示されるようになる
  - Issue番号をPull Requestのコメントに追加することで、Pull Request（小さなIssue）を束ねたIssue が作成できる。これをうまく利用することで、細かいタスクが各所に散らばる問題を防ぐことが出来る
- Issue完了時にはPull Requestのコメントに　Fix #issue-id と書く 
  - Tips: : Pull Requestのマージと同時にIssueを閉じる方法
  - commitメッセージ、Pull Requestのメッセージ、もしくはPull Requestのコメントで、issue idの前にfixかcloseが含まれていると、Pull RequestがマージされたタイミングでIssueも同時に閉じられることになる。
  - 記載例
  ```
  Fix #1234
  ```
  ```
  To close #3456
  ```
  - このように書くとIssueの閉じ忘れが減る


## 関連 
- 参考
  - [GitHubでタスク管理ができる新機能「Project」超入門](https://qiita.com/tonkotsuboy_com/items/f627ed6b3e4c6313e78f)
  - [開発者のタスク管理をGitHubで行ったらうまくいった話](https://dev.classmethod.jp/articles/github-issue-driven-dev/)
------
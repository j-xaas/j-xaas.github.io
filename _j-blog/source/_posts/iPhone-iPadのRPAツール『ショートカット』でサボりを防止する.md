---
title: iPhone/iPadのRPAツール『ショートカット』でサボりを防止する
date: 2020-04-09 00:28:09
category: 
- Tool Tips
- RPA
tags: 
- iPad Pro
- RPA
- iPhone
- ショートカット
- Siri
toc: true
---
  
　サボりを防止するために、勉強と関係の無いアプリを開いたら自動的に叱られるようにしてみました。  
ネタ記事ですが、Appleのショートカットアプリはかなり応用が効く便利ツールなので覚えておいて損はないです。  
  
- 海外版松岡修造として著名なシャイア・ラブーフさんに叱られます

{% youtube mW6S8-mofXE %}

<!-- toc -->

## 基礎知識
- RPA (Robotic Process Automation)
    - 定型的な事務作業をSoftware型のロボットが自動化する概念
        - 昨今の働き方改革ブームの文脈でバズワードになりがち
    - 直感的に利用可能なサービスが沢山出ているので、使えないとまずいです

- ショートカット
    - Apple純正のRPAアプリ
        - このアイコンのAppです
          
        <div style="text-align:center;">
        <img src="https://user-images.githubusercontent.com/41946222/78804246-e0e35080-79fa-11ea-82a5-5621aaefd7a5.png" height="180px" width="200px">
        </div>

    - 導入の手間が無い
        - iPhoneやiPadなら元から入っているため
        - 特に課金要素は無いので利用しないのはもったいないです
    - RPA入門者向け
        - これを使えない人が仕事でPC用のRPAツールを使うのは無理です
            - 直感的に使えるショートカットで慣れておきましょう
    - Siriからも実行可能
        - できる幅が広いので、使いこなせば大抵の作業を一言で完了できます

## 手順
1. ショートカットアプリを開く

2. 画面下部の”オートメーション”を選択

3. 画面右上の＋マークを選択
    - ”個人用オートメーションを作成”を戦t買う

4. ”ショートカットを作成”を選択
    - ここから以下のように設定します
        - トリガー
            - ”Appを開く”
        - アクション
            - URLを開く

5. 下にスワイプして"Appを開く"を選択

6. "Appを選択"画面で作業を邪魔するアプリを選択
    - 今回はTwitterを登録してみます

7. 次へ
    - アクションの設定画面に飛びます
    - ”アクションを追加”を選択

8. ”WEB” ⇒ ”URLを開く” を選択

9. 青字の”URL”にyoutubeのURLを指定
    - Do itおじさん動画のURL
        - https://www.youtube.com/watch?v=nwW4CDGucVs
    - 別の動画にも応用可能です

10. ”実行前に尋ねる”を無効化
    - 完了
    - 以上で適用されます

11. テスト
    - Twitterを開いてみましょう
    - 画面上部にオートメーションを実行と表示され、Do itおじさんの動画に強制的に飛ばされます

{% youtube mW6S8-mofXE %}

- 解除方法
    - オートメーションを編集
    - "このオートメーションを有効"の横のタブを無効化すればOK
    - 試験前にアプリをわざわざ消すより手軽に対策できます


## まとめ/応用例
- 今回は”指定したURLを開く”という最も基本的な機能を利用しました
- ショートカットはもっと複雑なことも沢山出来るので是非試してみてください

- 例
    - 位置情報をTriggerとした操作
        - 外出時に自動で予定表に入れた目的地への経路案内を表示
    - 時間をTriggerとした操作
        - タイマーを設定した時間に、IoT家電を操作
            - カーテンの開閉
            - 電気を点灯

## IT界隈の方向けの応用例
- 最近は手軽にAPI連携を確立できるサービスがいくらでもあるので簡単に応用できます

- 例
    - [IFTTT](https://ifttt.com/my_applets)との連携
        - This(Trigger)とThat(Action)を定めることで簡単に400種類以上のサービスとの連携を可能にするクラウドサービス
            - TriggerにWebhookを定めると所定のURLに対するリクエストだけで動く仕組みを作れます
        - つまり、ショートカットからIFTTTのURL宛にリクエストを送るだけで様々なサービスを動かせます
    - [FaaS (Function as a Service)](https://knowledge.sakura.ad.jp/15940/)と連携
        - [AWS Lambda](https://aws.amazon.com/jp/lambda/)や[Google Functions](https://cloud.google.com/functions?hl=ja)辺りをキックして動かせば大抵のシステムと連携可能です

## 関連記事
- [iPad ProでWEB AP開発 & RPA](https://j-xaas.github.io/2020/03/25/iPad-Pro%E3%81%A7WEB-AP%E9%96%8B%E7%99%BA/)
    - 同じ要領で開発にも応用しています
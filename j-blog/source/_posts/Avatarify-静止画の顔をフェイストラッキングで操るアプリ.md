---
title: Avatarify 手軽なディープフェイクの作り方　〜画像の顔をフェイストラッキングで操る〜
date: 2020-08-06 22:58:16
categories:
- AR
- App
tags:
- AR
- iOS
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/89621335-22faa100-d8cc-11ea-9830-3679ed5c3e07.png
---

最近流行りのディープフェイクを体験できるiOSアプリです。（PC版もあります）
一時期これを利用して、有名人や友人になりきってWEB会議する遊びが流行りました。

## 概要
- [Deepfake](https://ledge.ai/deep-fake/)とは
    - Deep Learningを用いて作り出される偽の動画や画像
        - ネット上では250枚ほどの写真をアップロードすると、2日程度でディープフェイク動画を作成するオンラインサービスもあり、1本あたり2.99ドル（約300円）ほどの低価格で取引されているらしい
    - 活用シーン
        - 最近では映画で特殊メイクの代わりに用いられることがあります。一方でポルノに勝手に有名人の顔を合成されて問題にもなっています。

- [Avatarify](https://apps.apple.com/us/app/avatarify-ai-face-animator/id1512669147)
    - 機能
        - 静止画の顔をリアルタイムで操作
            - リアルタイムで、手軽なのがこれまでのディープフェイクツールとの差分
        - フェイストラッキングで自分の顔に合わせて動く
    - 動作環境
        - iPhone/iPad
            - App Storeからインストール
        - PC版はPythonが必要
            - gitからインストールする

<img width="1018" alt="Avatarify" src="https://user-images.githubusercontent.com/68212997/89620566-cf3b8800-d8ca-11ea-998c-f68acbd5323b.png">


## 使い方
### iOS版
1. 事前に動かしたい静止画を用意
    - 実際の人物でも絵や動物でも動きますが、目/口がはっきりしていないとむりでした
2. 画像を登録
    - ここで設定した目/口の箇所が動きます
3. インカメで自分の顔を認識させて操作

以下の動画で解説しています

{% youtube LFv-5_st9rw %}

- ZoomなどのWEB会議で利用する場合は、背景にこれを表示するだけです。

### PC版(github)
- [github avatarify](https://github.com/alievk/avatarify)
    - 上記にソースが公開されています
    - 使用方法はRead.meを参照

- 公式のPC版デモ
{% youtube Q7LFDT-FRzs %}



- その他：DeepFaceLab
    - 動画の加工にはこっちが主に利用されているようです

<img width="844" alt="Deepface Lab" src="https://user-images.githubusercontent.com/68212997/89622466-1119fd80-d8ce-11ea-980e-49be7be11965.png">


PC版の使い方もそのうちまとめます。Pythonやgitが動く環境を構築済みの人ならすぐできそう。


## 関連記事
- [らくがきARの使い方](/らくがきARの使い方/)
- [[AR.js x A-Frame] WebAR入門～マーカーベースで3Dオブジェクトを表示するAPを開発する～](/ar-js-x-a-frame-WebAR入門/)
- [Github DeepFaceLab](https://github.com/iperov/DeepFaceLab)
- [【超簡単！誰でも使える！】有名人になりきってZOOM飲み会が出来るAvatarifyが凄すぎるので、使用方法をまとめてみた。](https://note.com/dobamine/n/n71951956feda)
- [ZoomやSkypeでリアルタイムに他人になりすませるオープンソースのディープフェイクツール「Avatarify」](https://gigazine.net/news/20200417-zoom-skype-avatarify/)

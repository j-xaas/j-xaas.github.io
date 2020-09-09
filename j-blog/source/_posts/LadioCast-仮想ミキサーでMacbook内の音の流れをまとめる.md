---
title: '[LadioCast] 仮想ミキサーでMacbook内の音の流れをまとめる'
date: 2020-09-08 21:52:22
category: 
- SmartHome
- DTM
tags: 
- LadioCast
- Steinberg UR44C
- DTM
- リモートセッション
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/92405492-a67c0c00-f170-11ea-8e10-6821e0a811e6.png
---

仮想ミキサーで音声の複数の入出力をまとめる手法について。MacBook Pro(2020) MacOS 10.15を利用しています

<!--toc-->

## 利用するツール

### SoundFlower
- [Soundflower](https://soundflower.softonic.jp/mac)
    - システム内部（Mac内部）の音声を読み込むために必要
    - Mac標準のサウンド設定で出力先をSoudflower(2ch)にすることで、他のソフトウェアに音を流すことができる
    - 使用例
        - YoutubeやDAWソフトの音(システム内部の音声)をSoundflowerに送り、ZoomやMeetの様な入力にSoudflowerを選択することで、通信相手にも聞かせる


### LadioCast
- [LadioCast](https://apps.apple.com/jp/app/ladiocast/id411213048?mt=12)
    - 入出力を各４つまで選択可能
    - L, Rも振り分け可能
    - 使用例
        - Soudflower経由のシステム内部の音声と、外部ポートからインターフェイスやコンデンサーマイクで流れてくる音をLadioCastでまとめてから、
        任意のコンテンツに出力
        - Recを行うDAWソフトとライブ配信ツール、返しに同時出力

<img width="894" alt="ladiocast" src="https://user-images.githubusercontent.com/68212997/92405492-a67c0c00-f170-11ea-8e10-6821e0a811e6.png">


## 利用例
- 音の流れ
<img width="1578" alt="sound_stream" src="https://user-images.githubusercontent.com/68212997/92479277-4e9bde80-f21e-11ea-9379-2711cdad986b.PNG">

- Ex. Macから出力される音声(youtubeなど)をZOOMにルーティングする方法
    - 1. Macのサウンド出力をSoundFlower(2ch)にする
    - 2. LadioCastを起動し、入力1を「SoundFlower(2ch)」、ルーティングを「メイン」と「Aux1」にする
    - 3. LadioCastの出力メインを「SoundFlower(64ch)」にする ※Zoomのマイク入力になる
    - 4. Zoomのマイクを「SoundFlower(64ch)」にする

## おまけ： 現在のDTM環境
![DTM構成](https://user-images.githubusercontent.com/41946222/79885198-61f60b00-8431-11ea-97cc-6298ae1c92a3.png)
![DTM構成②](https://user-images.githubusercontent.com/41946222/77458772-10625c80-6e42-11ea-9998-40235fa4a44c.png)

- DTM環境のセッティング自体については以下の記事にまとめています
    - [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)

## 参考
- [【リモートワーク】SoundFlowerとLadioCastを使いZoomでブラウザ再生したYouTubeの音を出す（Mac用）※追記あり](https://note.com/yamac/n/n64779ba3b870)

## Loopback
- [Loopback](https://loopback.softonic.jp/mac)
    - MacOS 10.10.0~10.13で動作
    - 利用中のMacOSは10.15であるため非対応でした

## 関連記事
- [[SYNCROOM] リモートセッションのやり方(YAMAHAの遠隔合奏アプリ)](/SYNCROOM-リモートセッションのやり方-YAMAHAの遠隔合奏アプリ/)
- [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)
- [iPad Pro (第4世代)でDTM環境を構築](/iPad-ProでDTM環境を構築/)
- [[SYNCROOM/NETDUETTO] YAMAHAのリモートセッションツールを試してみた](/YAMAHAのリモートセッションツールSYNCROOMのベータ版：NETDUETを試してみた/)
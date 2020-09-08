---
title: '[LadioCast] 仮想ミキサーでMacbook内の音の流れをまとめる'
date: 2020-09-08 21:52:22
tags:
---

- 利用中のOS
    - MacOS 10.15

## 利用するツール

### SoundFlower
- [Soundflower](https://soundflower.softonic.jp/mac)
    - Macから出力される音声を読み込むために必要

### LadioCast
- [LadioCast](https://apps.apple.com/jp/app/ladiocast/id411213048?mt=12)
    - 入出力を各４つまで選択可能
    - L, Rも振り分け可能

<img width="894" alt="ladiocast" src="https://user-images.githubusercontent.com/68212997/92405492-a67c0c00-f170-11ea-8e10-6821e0a811e6.png">


## 利用例
- 音の流れ
<img width="1578" alt="sound_stream" src="https://user-images.githubusercontent.com/68212997/92479277-4e9bde80-f21e-11ea-9379-2711cdad986b.PNG">

- Ex. Macから出力される音声(youtubeなど)をZOOMにルーティングする方法
    - 1. Macのサウンド出力をSoundFlower(2ch)にする
    - 2. LadioCastを起動し、入力1を「SoundFlower(2ch)」、ルーティングを「メイン」と「Aux1」にする
    - 3. LadioCastの出力メインを「SoundFlower(64ch)」にする ※Zoomのマイク入力になる
    - 4. Zoomのマイクを「SoundFlower(64ch)」にする

## 参考
- [【リモートワーク】SoundFlowerとLadioCastを使いZoomでブラウザ再生したYouTubeの音を出す（Mac用）※追記あり](https://note.com/yamac/n/n64779ba3b870)

## Loopback
- [Loopback](https://loopback.softonic.jp/mac)
    - MacOS 10.10.0~10.13で動作
    - 利用中のMacOSは10.15であるため非対応でした

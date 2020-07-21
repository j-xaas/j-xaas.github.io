---
title: '[Spark AR] ARフィルターの作り方'
date: 2020-07-05 03:45:39
categories:
- AR
tags:
- AR
- Spark AR
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/86519480-0a830b00-be76-11ea-82d2-b365dabd28da.png
---
​
Spark AR Studioというソフトを使って、ARフィルターを作る手法を解説します。

<!-- toc -->
​
## Spark AR Studioの準備
​
- 以下のサイトからPCへダウンロード
    - [Download Spark AR Studio](https://sparkar.facebook.com/ar-studio/download/)
    - 指示にしたがってインストールしましょう
​
![Spark AR Studio Download](https://user-images.githubusercontent.com/41946222/86513437-34700980-be45-11ea-964c-ff3f1f8e768d.png)
​
​
- スマホで動作確認するアプリもダウンロードしておいてください
    - [App Store](https://itunes.apple.com/app/facebook/id1231451896?fbclid=IwAR0j4At5Kjl1SIF5GWpb0beM2_BCd1ThImhFbWHtYWad918CsofyUEN9jlY)
    - [Google Play](https://play.google.com/store/apps/details?id=com.facebook.arstudio.player&fbclid=IwAR1ZAnpJRAUAS5gbDBJ7WIYBwIjfoviNIkFiTAOnKmP2rCAR95QSOgnuqYQ)
​
![Spark AR Player](https://user-images.githubusercontent.com/41946222/86513446-4e115100-be45-11ea-92b5-8e3b1e35ef6b.png)
​
​
​
## ARフィルターの作成方法
- 先ほどインストールした、以下のアプリを開きます
![Spark AR Studioアイコン](https://user-images.githubusercontent.com/41946222/86513480-99c3fa80-be45-11ea-9284-2e8bc7dfc97f.png)
​
- まずはFacebookアカウントでログイン
![Login](https://user-images.githubusercontent.com/41946222/86513571-25d62200-be46-11ea-999a-d72ad37c59b0.png)
​
- アプリを起動するとこんな感じ
![image](https://user-images.githubusercontent.com/41946222/86515785-88371e80-be56-11ea-8c5d-edbf1d1a784b.png)
​
### チュートリアル
1. Viewpoint
    - カメラの視点を確認できます
​
![SparkAR/ViewPoint](https://user-images.githubusercontent.com/41946222/86516013-4909cd00-be58-11ea-8679-ab5435d59246.png)
​
​
2. Simulator
    - 右上のSimulatorでデバイスでの同さを確認可能です
​
![SparkAR/Simulator](https://user-images.githubusercontent.com/41946222/86515872-2e832400-be57-11ea-87fa-dc470427a5fd.png)
​
3. Sidenav
    - 左のサイドナビで、Simulatorを停止することができます
​
![SparkAR/Sidenav](https://user-images.githubusercontent.com/41946222/86515957-e284af00-be57-11ea-9086-8453bfd155cc.png)
​
4. Scene Panel
    - Objectを足すことができます
​
![SparkAR/Scene Panel](https://user-images.githubusercontent.com/41946222/86516098-cdf4e680-be58-11ea-89b2-c29feb9d9012.png)
​
5. Assets Panel
    - マテリアルやテクスチャのアセットを追加できます
    - 下のAdd AssetsからLocal端末上に保存しているデータを読み込めました。
​
![SparkAR/Assets Panel](https://user-images.githubusercontent.com/41946222/86516167-55daf080-be59-11ea-925a-092451d99dbb.png)
​
6. Easy build Library
    - 100種以上の3Dオブジェクトや音源が用意されています
​
![SparkAR/Easy build](https://user-images.githubusercontent.com/41946222/86516375-009fde80-be5b-11ea-841e-cfced9c61fb1.png)
​
7. Object 設定パネル
    - オブジェクトの設定をいじることができます
​
![SparkAR/Object Config](https://user-images.githubusercontent.com/41946222/86516538-5fb22300-be5c-11ea-9c4e-13c2471461d9.png)
​
8. Test
    - Testfileを送ってエフェクトを試すことができます
    
![Spark AR/Test](https://user-images.githubusercontent.com/41946222/86516583-c3d4e700-be5c-11ea-86f1-8ea9e2128ce0.png)
​
9. Upload to Spark AR Hub
    - Spark ARのハブに公開することができます
​
![Spark AR/Spark AR Hub](https://user-images.githubusercontent.com/41946222/86516617-1f06d980-be5d-11ea-8c7a-6a566fef1b17.png)
​
​
10. ducumentation/Q&A
    - 左下のボタンより説明資料やQ&Aを行えます
​
![Documentation/Q&A](https://user-images.githubusercontent.com/41946222/86516669-9fc5d580-be5d-11ea-95aa-d1ab02b7de10.png)
​
​
### 作り方
- チュートリアル後のHome画面はこんな感じ
![Spark AR home](https://user-images.githubusercontent.com/41946222/86517026-8b370c80-be60-11ea-94b3-da7b98a5fdeb.png)
​
- New Projectかテンプレートから開発を開始しましょう
- 作り方のイメージ
    - 3Dのオブジェクトを用意する
        - iPadのアプリでキャプチャする
            - display.landやQuote、pronoMeshScan辺りがお勧めです。ここは別記事に纏めます。
        - ネットで拾ってくる
            - [Free3D](https://free3d.com/ja/)
            - [Turbosquid](https://www.turbosquid.com/?referral=rymk678908)
            - [yobi3d](https://www.yobi3d.com/)
            - [Artist3D.com](http://artist-3d.com/)
            - [3D Free](http://www.3dfree.jp/)
            - [Dimensiva](https://dimensiva.com/)
            - [3D Model Free.com](http://www.3dmodelfree.com/)
            - [archive3D](https://archive3d.net/)
            - [3Delisious](https://3delicious.net/)
        - 自作する場合(CGデザイナー以外)
            - iPadで以下のアプリを使えば素人でも簡単に作れます
                - Putty3D
                - Sharp3D
    - Add ObjectからPC上のデータを選んで取り込む
        - 後はSpark AI上で編集
    - 画像をテクスチャとして表示するときも同じような感じ
​
詳細な作り方は別途まとめます。
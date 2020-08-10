---
title: '[AR.js x A-Frame] WebAR入門～マーカーベースで3Dオブジェクトを表示するAPを開発する～'
date: 2020-07-27 23:18:34
categories:
- AR
- WebAR
tags:
- AR
- WebAR
- AR.js
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/88541481-c7205480-d04f-11ea-812a-53d2765f47a6.png
---

AR機能を持つWEBアプリの作り方を解説します。今回はAR.jsとA-Frameいうフレームワークを活用して、特定のマーカーの上に3Dオブジェクトを表示するImage Tracking型のARを作ります。30m程度で楽にできました。
CGデザイナーや駆け出しエンジニアでも簡単にできると思います。

<!-- toc -->

## 基礎知識
WebARの概要と主な技術ついてです。
すぐに実装に入りたい人は飛ばしてOK

### WebARとは
![AR.js](https://ar-js-org.github.io/AR.js-Docs/intro-image.gif)

- AR(拡張現実)をWebブラウザ上で実現する技術
    - ユーザーはアプリのインストールが不要で手軽
    - 近年様々なサイトで使われ始めています
        - Ex.) ECサイトのサンプル表示、観光地の城の表示、バンドのCDジャケットの上にメンバーを表示..
    
- WebARの種類
    - マーカーベース
        - 特定の画像(マーカー)の上に3モデルを表示
    - ロケーションベース
        - 特定の場所に3Dモデルを表示

- WebARを実現する主な手段
    - A-FRAME
    - AR.js
    - model-viewer
    - WebXR Device API
    - 8th wall

- 今回利用するJavaScripライブラリ/フレームワーク
    - A-FRAME
    - AR.js

### A-FRAME
![a-frame.gif](https://j-xaas.github.io/a-frame.gif)
- Mozilla開発の3D VR 空間構築のWebVRフレームワーク
- インタラクティブな3D動きを表現可能
    - 特定のオブジェクトのタッチで動くアクション、機能まで作れる
- これ単体ではARにはならない

### AR.js
![AR.js](https://ar-js-org.github.io/AR.js-Docs/intro-image.gif)

- [AR.js Documentation](https://ar-js-org.github.io/AR.js-Docs/)
    - ARをWebで実現するライブラリ
        - A-Frameで表現した３Dの動きをこれでAR化するイメージ
    - 10行程度の記述で手軽にARの実装が可能
    - .obj形式のデータを用意すれば簡単に出せます
    - 調べた限りでは、これが一番主流
    - ベースはthree.jsとjsartoolkit5


#### ARのタイプ
- マーカーベース
    ![AR.js Marker Dog](https://user-images.githubusercontent.com/68212997/88541481-c7205480-d04f-11ea-812a-53d2765f47a6.png)
    - 特定のマーカーがカメラに映った際に、その上に表示する
    - 使用例
        - 拡張アート、学習（拡張書籍）、拡張チラシ、広告...
- 画像ベース(ImageTracking)
    ![AR.js Image Tracking T-REX](https://user-images.githubusercontent.com/68212997/88548257-d3111400-d059-11ea-9c55-7f8b58ba5503.png)
    - 特定の画像の上に表示
        - マーカーベースとほぼ仕組みは同じ
    - 安定性はマーカーの方が高い
- ロケーションベース

    ![AR.js Location Based AR](https://user-images.githubusercontent.com/68212997/88549099-dce74700-d05a-11ea-8d98-770b5a3d123e.png)

    - マーカー無しで特定の場所に表示（ポケモンGOとかのイメージ）
    - 最近のアップデートで対応しました
    - 使用例
        - 3Dマップ、ゲーム、観光...
    - こちらは別記事で解説します

### その他
- model-viewer
    - モバイルのブラウザからWebARを閲覧可能にするWebコンポーネント
    - インタラクティブなボタンは追加できない
    - アニメーションも表示可能
![model-viewer](https://user-images.githubusercontent.com/68212997/87849370-01d50f00-c923-11ea-926e-63434be8b837.png)

- WebXR Device API
  - インタラクティブな要素を追加可能
  - WebARを実現するJavaScriptライブラをゴリゴリ書ける人向け
  - Chrome ブラウザのバージョン79以降で利用可能
![WebXR](https://user-images.githubusercontent.com/68212997/87849322-a6a31c80-c922-11ea-8554-a72c3d840037.png)

- 8th Wall
    - ARのプラットフォームサービス
![8thwall](https://user-images.githubusercontent.com/68212997/87849265-2381c680-c922-11ea-808f-0a92dd45376f.png)

- その他モバイルのARに使われるもの
    - ARKit
    - ARCore

----------------------------------------

## 実装手順
簡素なサンプルAPを作って公開するまで

### AR.jsのサンプルコードを実装
- 適当なディレクトリを作成
    - ar-test

- 上記のディレクトリの中にファイルを作成
    - index.html

- index.htmlを編集
```
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width,intial-scale=1">
        <title>WebAR</title>
    </head>
    <body style="margin:0px; overflow:hidden;">
        <script src="https://aframe.io/releases/0.8.0/aframe.min.js"></script>
        <script src="https://jeromeetienne.github.io/AR.js/aframe/build/aframe-ar.js"></script>

        <a-scene embedded arjs="debugUIEnabled:false; sourceType: webcam;">
            <a-marker preset="custom" type='pattern' url="my-icon-marker.patt">
              <a-text value="My name is soeyu!\n Nice to meet you!" position=" 0 0 1" align="center" rotation="-90 0 0" color="#7993ff">
              </a-text>
            </a-marker>
            <a-entity camera></a-entity>
        </a-scene>
    </body>
</html>
```


### 3Dデータを用意する
- .obj形式３Dデータを用意して、上記のhtmlから参照させます
    - ※mtl(マテリアルデータ)までセットで必要

- 今回はネットで無料のデータを拾います
    - その他の手法については後述
    - 以下のサイトから拾います
    - Free3D
- 表示したいデータは


### APを公開
作成したAPを公開して、ブラウザから確認してみましょう。
先ほど作成したindex.htmlとそこから参照する3Dモデルのデータをまとめてアップします。
公開先は以下のどれでもOK

#### Netlifyで公開
- [Netlify Drop](https://app.netlify.com/)
   - 手軽な無料のホスティングサービス
   - githubのアカウントがあれば、ソースのディレクトリをドラッグ＆ドロップするだけ

- アカウントを作成
    - GithubのアカウントやEmailで登録できます

![Netlify　Make Account](https://user-images.githubusercontent.com/68212997/88545763-5e88a600-d056-11ea-9674-a852e770a23c.png)

- ログインするとこんな感じ
![Netlify Login](https://user-images.githubusercontent.com/68212997/88545941-a14a7e00-d056-11ea-96e5-58b18779102e.png)

先ほど作成したディレクトリ毎ここにドラッグするだけでAPを公開できます
- 


#### その他の手法で公開
それぞれ別記事で解説しているので今回は割愛します。
Netlifyは簡易的なものなので、ちゃんと公開するのであれば、何れかがいいと思います。

- Github Pages
    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- AWS S3
    - [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
- Firebase Hosting
    - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)

### 動作確認/作成したAPの使い方
今回はマーカーの上に3モデルを表示する形式なので、マーカーは事前に印刷しておきましょうディスプレイ表示できる機器がスマホと別であればそれでもOK

- 公開したURLにアクセスすると、APが起動してカメラへのアクセス許可を求められます
- Agree
-  APが起動した状態でマーカーをカメラに写すと、3Dモデルが表示されます
   - マーカー全体がカメラに収まらないと表示されません



-------------------------------

## カスタマイズしてみる

### マーカーを変更
以下のサイトで好きなマーカーを作成できます

- [AR.js marker generater](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)

- "Upload"から好きな画像を取り込めます
    - ここで例えば、CDのジャケットを画像とし取り込めば

### 3Dデータを変更する
#### 必要なデータ
- .obj形式３Dデータ
    - ※mtl(マテリアルデータ)までセットで必要です

#### 3Dデータを用意する手段
以下の手段があります

1. ネットで無料のデータを拾う
- 以下の様なサイトに沢山落ちてます
    - Free3D

2. iPadでキャプチャーする
- 別記事で解説しています
- お勧めのアプリは以下
    - display.land
    - 3D Scanner App
    - Capture

3. 自作する
- CGデザイナー向けの手法
- 私はデザイナーではありませんが、素人でもiPadのお絵描きアプリやCADで簡単なものを作れました。



## 参考

### 関連記事
#### AR
- [[AR.js Studio] NoCodeでWebARをGithub Pagesに公開する](/AR-js-Studio-NoCodeでWebARをGithub-Pagesに公開する/)
- [[Spark AR] ARフィルターの作り方](/Spark-AR-ARフィルターの作り方/)
- [iPadで3Dデータをキャプチャする手法 [display.land]](/iPadで3Dデータをキャプチャする手法-display-land/)
#### Hosting
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)

- [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)

- [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)

### その他
- [AR.js Documentation](https://ar-js-org.github.io/AR.js-Docs/)
- [AR.js](https://github.com/jeromeetienne/AR.js/blob/master/README.md)
- [A-Frame](https://aframe.io/)
- [poly](https://poly.google.com/)
- [Netrify](https://app.netlify.com/)
- [Three.js](https://jeromeetienne.github.io/AR.js-docs/docs/)
- [WebARの概要と実装方法（AR.js）](https://medium.com/@yonemoto/webar%E3%81%AE%E6%A6%82%E8%A6%81%E3%81%A8%E5%AE%9F%E8%A3%85%E6%96%B9%E6%B3%95-ar-js-b5739540feb7)
- [マーカーレス WebARの現状をまとめました　～アプリレスのブラウザAR～](https://ux.daishinsha-cd.jp/blog/webar)
- [簡単爆速AR！(webAR)](https://qiita.com/poccariswet/items/5f24b23341626f9e0f5d)
- [A-FrameとAR.jsで超簡単AR（PC・スマホ・マルチマーカー対応](https://dsp.logly.co.jp/click?ad=4AG4hpsDG-Sx8bGR7Gbnf1q72w5E0dNW-4iWmmC-vn4FyShFeZz5103M_KPcFfbO2-aGcMj5H_o9UkGzQpUg65iC8vqhLJECLoLIJ62duGtfW3Kl4w5umE88WvZPJChDn6H1CzL74PboXlDHtQaaOi2B9NchjR8qOlSBVvqn4b5V3dUp_-YlB36U0v3lkx5cEtXQ9o_ubMB-dq6MqFwYyJAK0oPEQUhBOo1SCtimfecoFfB3SZqHcTVQ_O9xKtCwxzaU_8hDIfN51N36UeFaOVQ9f2NwQmQ4bgB-uKFeS3M0tfU2YKUsryvvsEcpmAkqVKTNClYz_SoBJ7klhdHimnnywfeSyld53kS84GIf2E5SCBNT1MtYH_KhbvyDDoQrmfXzV7z55zcKKx79PwKRbXu78FeJjzW7oDo8n5C0k2kxxX7h-NJWl2Dxohf32UVu_fu8ZoWbSu0Cdvyc5cPNirWnZXImyf73X4-SjZeqM228JU69FeKLc-QRjaZ8CgWN5aHjUAgeE5fiM6HHP5sbDS_j6D-D8Nyzu3yQsGUnndvGi0FshDnsCg)



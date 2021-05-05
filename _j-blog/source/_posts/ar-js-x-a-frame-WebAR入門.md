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
        - 特定の`画像(マーカー)の上`に3Dモデルを表示
    - ロケーションベース
        - 特定の`場所`に3Dモデルを表示

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
![a-frame](https://user-images.githubusercontent.com/68212997/116892860-4429cb80-ac6b-11eb-8b57-4a53a6ef3c57.png)

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
  - WebARを実現するJavaScriptライブラリをゴリゴリ書ける人向け
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

- 適当なディレクトリを作成
    - ar-test

### 3Dデータを用意する
- .obj形式３Dデータを用意して、上記のhtmlから参照させます
    - ※mtl(マテリアルデータ)までセットで必要

- 今回はネットで無料のデータを拾います
    - 以下のサイトから拾います(他の方法は後述)
    - [Free3D](https://free3d.com/ja/3d-models/%E7%8C%AB)

- 今回は猫を表示します
    - 無料のデータと有料のデータがあります

<img width="1038" alt="Free3D cat" src="https://user-images.githubusercontent.com/68212997/90363412-ac189180-e09d-11ea-819d-7cb125c0383d.png">

- Freeの茶虎を選択
    - AR.jsで利用可能なデータ形式か注意
        - .objはAR表示可能です
    - ダウンロード

<img width="989" alt="Free3D Cat茶とら" src="https://user-images.githubusercontent.com/68212997/90363868-5d1f2c00-e09e-11ea-923b-adfe03674297.png">

- ダウンロードしたZipファイルを展開
    - .objファイルと.mtlファイルの両方が必要です

<img width="482" alt="AR js Free3D Data" src="https://user-images.githubusercontent.com/68212997/90364104-d454c000-e09e-11ea-840c-83caedf0e891.png">

ARで表示するデータの準備は以上でOK

### AR.jsをhtmlで利用
- 適当なディレクトリを作成
    - ar-test

- 上記のディレクトリの中にファイルを作成
    - index.html

- index.htmlを編集
```
<!doctype HTML>
<html>
<!-- A-Frame ライブラリの読み込み -->
<script src="https://aframe.io/releases/1.0.0/aframe.min.js"></script>
<!-- AR.js ライブラリの読み込み -->
<script src="https://cdn.rawgit.com/jeromeetienne/AR.js/2.2.0/aframe/build/aframe-ar.js"></script>
<body style='margin:0px; overflow:hidden;'>
<!-- こっからA-Frame -->
<a-scene embedded arjs><!--="debugUIEnabled:false;trackingMethod:best;" vr-mode-ui="enabled: false"--->

    <!-- こっからARの世界。マーカー上になにを展開するかを書く -->
    <a-marker preset="hiro">
        <!--objファイル: 形　mtlファイル: 表面　mtlなしでは透明になってしまう--->
        <a-entity 
            obj-model="obj: url(obj/12221_Cat_v1_l3.obj);
        mtl: url(obj/12221_Cat_v1_l3.mtl)"
            scale="0.02 0.02 0.02"
            rotation="-90 0 0"
        >
        </a-entity>
        <!--scale:サイズ　rotation: 角度 -90??-->
             <a-text value="Cat AR by J" position="0 0.8 0" align="center"></a-text>
    </a-marker>
    <!-- arなのでカメラが必要 -->
    <a-entity camera></a-entity>
</a-scene>
<!-- ここまででA-Frameおわり -->

</body>
</html>
```

- このhtmlファイルでは、以下のようなWEBページを定義している
    - アクセスするとカメラが起動
    - a-markerで指定したマーカーをカメラに写った際にAR.jsの機能が起動
        - a-entityで指定したオブジェクトをARとして表示
    - a-textで空中に文字を表示

### 各ファイルを同一ディレクトリにまとめる
.objファイルと.mtlファイルをまとめる

- AR.jsはhtmlで参照する3Dデータの相対パスを指定してARを表示します

- ディレクトリを作成
    - 手動でもOK
```
mkdir "ar-test"
```

- ディレクトリにindex.htmlと３Dデータをコピー
    - 以下のようにまとめる

<img width="743" alt="ar-test directory" src="https://user-images.githubusercontent.com/68212997/90369760-44ffda80-e0a7-11ea-921e-b0b417a840bf.png">


- 3Dデータはobjというフォルダにまとめる
<img width="549" alt="ar js directory" src="https://user-images.githubusercontent.com/68212997/90370009-a1fb9080-e0a7-11ea-93f0-2eaade28caa6.png">



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

先ほど作成したディレクトリ（ar-test）をここにドラッグするだけでAPを公開できます


#### その他の手法で公開
それぞれ別記事で解説しているので今回は割愛します。
Netlifyは簡易的なものなので、ちゃんと公開するのであれば、↓の何れかがいいと思います。

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


- 実際に公開したAPがこちら
    - https://xenodochial-cray-e209cf.netlify.app/
    - アクセスするとカメラが起動します

- マーカーを画角に収めると猫が現れます



-------------------------------

## カスタマイズしてみる

### マーカーを変更
自力でマーカーをいじろうとするとかなり専門的な知識を要求されますが、以下のジェネレーターで簡単に作成できました

- [AR.js marker generater](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)

- "Upload"から好きな画像を取り込めます
    - ここで例えば、CDのジャケットを画像として取り込めば、その上にARを表示できる。広告への応用もできそう

---
### 3Dデータを変更する
#### 必要なデータ
- .obj形式の3Dデータ
    - ※mtl(マテリアルデータ)までセットで必要です

#### 3Dデータを用意する手段
以下の手段があります

1. ネットで無料のデータを拾う
- 以下の様なサイトに沢山落ちてます
    - Free3D

2. iPadでキャプチャーする
- 以下の記事で解説しています
    - [iPadで3Dデータをキャプチャする手法 [display.land]](/iPadで3Dデータをキャプチャする手法-display-land/)

- お勧めのアプリは以下
    - display.land
    - 3D Scanner App
    - Capture

3. 自作する
- CGデザイナー向けの手法
- 私はCGデザイナーではありませんが、素人でもiPadのお絵描きアプリやCADで簡単なものは作れました。
    - AR.jsが対応可能な形式でデータを出力できるアプリであればどれでも使えます

---
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



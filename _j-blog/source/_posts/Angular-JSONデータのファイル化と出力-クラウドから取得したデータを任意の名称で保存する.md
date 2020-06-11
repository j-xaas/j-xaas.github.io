---
title: '[Angular JavaScript] JSONデータのファイル化と出力 ~クラウドから取得したデータを任意の名称で保存するロジック~ (TypeScript)'
date: 2020-05-31 22:55:36
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
- JSON
- JavaScript
- TypeScript
- ファイル操作
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/83424662-abda0400-a467-11ea-831a-2b146d29a1c5.png
---

Angular APでローカル端末にファイルを出力する機能をWEB APに実装する手順を解説します。外部API(ex. AWS)からService経由でGETしたデータをLocal端末に保存するケースを想定しています。色々試したものの、WEB APはセキュリティ制約が多いので、やり方が限られました。

<!-- toc -->

## 1. 基礎知識

### 1.1. 今回の手法(JacaScriptのdownloadを使用)

詳しくは手順を参考にしてください

- リンクタグのDOMを取得してそこからdownload
  - 保存するJSONファイルの名前を一意に定義
  - レスポンスデータをJSON形式に変換
    - JSON.stringify()を使用
  - HTMLのリンク要素を生成
    - link = document.createElement()
  - リンク先にJSON形式の文字列データを置いておく
    - link.hrel = 'data:text/plain,' + encodeURIComponent(data);
  - 保存するJSONファイルの名前をリンクに設定
    - link.download = fileName
  - ファイルを保存
    - link.click()で自動で疑似的にリンクをクリックさせる

- ポイント
  - プラグインの導入や、ブラウザの制約無しという条件であれば、これ以外の手法は現状ありませんでした
  - ファイルの保存先は指定不可
    - 端末の”ダウンロード”に入ります
  - セキュリティの都合上、ブラウザから直接端末のドライブに読み書きできないようになっているようです

### 1.2. その他の手法
- Device Storage API
- Native File System API
  - 保存先の指定が可能
  ```
  このAPIはユーザーのデバイス上にあるファイルを読み込んだり、任意のディレクトリにファイルを書き込むことができるAPIです。途中段階のAPIですが、このAPIが提供されることでスマホアプリやデスクトップアプリのような機能をWebアプリでも提供できるようになります。
  ```
  - 今回は採用しません
    - ブラウザによっては動かず、APの利用に前提条件ができてしまう為
  - 採用する場合は以下を参考にどうぞ
    - [ブラウザからドライブにファイルの書き込みができるNative File System APIとは？](https://www.mitsue.co.jp/knowledge/blog/frontend/201909/30_1002.html)

## 2. 単一ファイルを取得
以下の構成を想定
- get-file.component
  - get-file.component.html
  - get-file.component.ts
- service
  - sample.service

### 2.1. get-file.component.html
- (click)イベントでメソッドを起動するボタンを作成
  - TS側に外部からデータを取得して、ファイルとして保存するロジックを書きます

- get-data.component.html
  ```
  <button mat-raised-button (click)="submit()">Get Data</button>
  ```

- その他：フォームやチェックリスト等を作成
  - 問い合わせ用のパラメータ設定をユーザが自由に弄れるようにするため
  - 別記事で解説 ＆ 今回の趣旨と異なるため省略

### 2.2. get-file.component.ts
(click)イベントで発火するメソッドをこちらで作りこみます

- submit()
  - 機能
    - 外部からJSONデータを取得
      - 外部APIへの問い合わせを実行するServiceを呼び出す
    - ファイル名を自動生成
      - 時刻を自動取得＆フォームの入力値と合わせる
    - 取得データを加工してJSONファイル化

### 2.3. 実装例
```
  submit() {

    // 1. 各種パラメータの取得
    // ----1.1. ファイル名のprefixに付ける日時データを取得---
    // Dateオブジェクトの作成
    const now = new Date();
    // 各日時要素を取得
    const year = String(now.getFullYear()); // 年
    // 1月=0と出るため+
    const month = String(now.getMonth() + 1); // 月
    const date = String(now.getDate()); // 日
    const hour = String(now.getHours()); // 時
    const min = String(now.getMinutes()); // 分
    const sec = String(now.getSeconds()); // 秒
    // YYYYMMDDHHMMSの形式で変数timeにまとめる 
    const time =  year + month + date + hour + min + sec;

    // 1.2. AWSへの問い合わせ用のパラメータを取得
    // 今回はAWSから情報を取得するパターンを想定
    // （認証とregionの指定、その他パラメータが必要）
    // 1.2.1. Credential認証
    AWS.config.credentials = new AWS.Credentials(AWS_CONFIG.accessKeyId, AWS_CONFIG.secretAccessKey);

    // 1.2.2. チェックされたリージョンリストを取得
    const REGION = this.makeRegionList();

    // 1.2.3. 問い合わせ用のパラメータ(ex. sampleParameter)の取得
    const parameter = this.makeSampleParameterList();

    // 2. 外部APIへの問い合わせとファイル化----    
    // Service経由でデータを取得
    // forkjoinを利用する為に返り値をobservablesに格納
    let observables = REGION.map(region => { // 引数としてRegion情報を渡す
      return this.sampleService.getData(region, resourceType).pipe(catchError(e => observableOf({"error": e})))
    });

    // 3. ファイル化
    // リクエストの終了を待って返り値に処理を加える
    // 非同期処理の待ち合わせの為にforkJoinを利用
    forkJoin(observables).subscribe( response => {
      // Observableをsubscribeして、中の値を取り出し、ファイルとして出力

      // レスポンスを加工してjsonファイルとURLを作る

      // 3.1. 保存するJSONファイルの名前: yyyymmddhhmmss-region_name.json とする
      // 拡張子をfiletypeで指定
      const filetype = '.json';
      // file名を設定　
      const fileName = time + '_' + REGION + filetype;

      // 3.2. データをJSON形式の文字列に変換する。
      const data = JSON.stringify(response);

      // 出力:リンクタグのDOMを取得してそこから行う

      // 3.3. HTMLのリンク要素を生成する
      const link = document.createElement('a');

      // 3.4. リンク先にJSON形式の文字列データを置いておく。
      link.href = 'data:text/plain,' + encodeURIComponent(data);

      // 3.5. 保存するJSONファイルの名前をリンクに設定
      link.download = fileName;

      // 3.6. ファイルを保存
      link.click();

    });

```

## 3. 複数ファイルをダウンロード

処理を複数回実行する必要があります。問い合わせ処理とファイル化処理を特定のパラメータに応じてfor文でループさせます。

- 想定するシチュエーション
  - AWSからデータを取得
    - 指定した各region毎の情報をファイル化します
- 機能連携イメージ
  - ボタンを押下
    - submit()メソッドが発火
  - 各種パラメータを取得
    - ファイルの命名と問い合わせに必要な情報を集める
  - 問い合わせを実行（データを持ってくる）
    - 特定のパラメータ数回、外部からデータを取得 ⇒ 配列に格納
      - 外部APIに問い合わせを実行するサービス(sampleService)を呼び出して処理はそちらに任せる
  - ファイル化処理
    - ファイルを分けたい単位（特定のパラメータより規定）

```
  // データを取得 ⇒ ローカルにJSONファイルを保存するメソッド
  submit() {

    // 1. 各種パラメータの取得
    // ----1.1. ファイル名のprefixに付ける日時データを取得---
    // Dateオブジェクトの作成
    const now = new Date();
    // 各日時要素を取得
    const year = String(now.getFullYear()); // 年
    // 1月=0と出るため+
    const month = String(now.getMonth() + 1); // 月
    const date = String(now.getDate()); // 日
    const hour = String(now.getHours()); // 時
    const min = String(now.getMinutes()); // 分
    const sec = String(now.getSeconds()); // 秒
    // YYYYMMDDHHMMSの形式で変数timeにまとめる 
    const time =  year + month + date + hour + min + sec;

    // 1.2. AWSへの問い合わせ用のパラメータを取得
    // 今回はAWSから情報を取得するパターンを想定
    // （認証とregionの指定、その他パラメータが必要）
    // 1.2.1. Credential認証
    AWS.config.credentials = new AWS.Credentials(AWS_CONFIG.accessKeyId, AWS_CONFIG.secretAccessKey);

    // 1.2.2. チェックされたリージョンリストを取得
    const REGIONS = this.makeRegionList();

    // 1.2.3. 問い合わせ用のパラメータ(ex. sampleParameter)の取得
    const parameter = this.makeSampleParameterList();

    // 2. 外部APIへの問い合わせとファイル化----
    // 以降の処理をsampleParameter毎にループさせる

    for (let a = 0, numSampleParameter = sampleParameter.length ; a < numSampleParameter; a++) {

      // ------2.1. region数回　AWSへの問い合わせを実行---------
      // forkjoinを利用する為に返り値をobservablesに格納

      const observables = REGIONS.map(region => { // 引数としてRegionを渡す
        return this.sampleService.getData(region).pipe(catchError(e => observableOf({"error": e})))
      });

      // ------2.2. ファイル化処理をregion数回ループさせる-------

      for (let i = 0, numRegions = REGIONS.length ; i < numRegions; i++) {
        // リクエスト(非同期処理)の終了をforkjoinで待ち合わせる
        forkJoin(observables[i]).subscribe( response => { 
          // Observableをsubscribeして、中の値を取り出し、変数データの内容を変数responseとして扱う

          // 2.2.1. レスポンスを加工してjsonファイルとURLを作る
          // 保存するJSONファイルの名前: yyyymmddhhmmss-region_name.json とする

          // 拡張子をfiletypeに指定
          const filetype = '.json';
          // file名を設定　
          const fileName = time + '_' + REGIONS[i] + filetype;

          // 2.2.2. データをJSON形式の文字列に変換
          const data = JSON.stringify(response);

          // 出力:リンクタグのDOMを取得してそこから行う
          // 2.2.3. HTMLのリンク要素を生成する。
          const link = document.createElement('a');

          // 2.2.4. リンク先にJSON形式の文字列データを置いておく
          link.href = 'data:text/plain,' + encodeURIComponent(data);

          // 2.2.5. 保存するJSONファイルの名前をリンクに設定
          link.download = fileName;

          // 2.2.6. ファイルを保存
          link.click();
        });
      }  // -------ファイル生成ループ終了--------
    }
```

- service側の処理や受け渡す変数、ファイル名はシチュエーションに合わせて補完してください


## 4. 参考
### ファイルのダウンロード
- [JSでダウンロードを実装した話](https://qiita.com/Ohtak/items/b7fb05c4e3dee13c0d1f)
- [JavaScriptでローカルにファイルを保存する方法（その1）](JavaScriptでローカルにファイルを保存する方法（その1）)
- [JavaScriptでダウンロードされるファイルの保存場所を指定する](https://ja.stackoverflow.com/questions/14530/javascript%E3%81%A7%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89%E3%81%95%E3%82%8C%E3%82%8B%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E4%BF%9D%E5%AD%98%E5%A0%B4%E6%89%80%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B)
- [[Angular] CSV ファイルを出力したときにやったこと](https://qiita.com/ksh-fthr/items/29db7c5c7268ee1802c5)
- [JavaScriptでJSONファイルを保存する/開く方法](https://slash-mochi.net/?p=3073)
- [JavaScriptでFile APIをファイル操作方法ついて解説](https://www.e-loop.jp/knowledges/35/)
- [web.dev JavaScriptでファイルを読み込む](https://web.dev/read-files/)

- [MDN ウェブアプリケーションからのファイルの使用](https://developer.mozilla.org/ja/docs/Web/API/File/Using_files_from_web_applications)

### その他
- [JavaScriptで現在の日付、時刻を取得する - JavaScript プログラミング](https://www.ipentec.com/document/javascript-get-current-datetime)
- [Dateオブジェクト](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [ブラウザからドライブにファイルの書き込みができるNative File System APIとは？](https://www.mitsue.co.jp/knowledge/blog/frontend/201909/30_1002.html)

### jQuery or Pythonを使用
- [jsonファイルの読み込み纏め](https://qiita.com/yoshida3/items/36083f7959771b62e9af)

- [How To Read Local JSON Files In Angular](https://www.angularjswiki.com/angular/how-to-read-local-json-files-in-angular/)

### JSON JavaScriptでの扱い
- [JSONの扱い方を解説！PythonやJavaScript・Swiftでの基本的な使用法とは。配列取得や出力方法も紹介](https://agency-star.co.jp/column/json-parse/)

### MDNドキュメント
- [MDN File](https://developer.mozilla.org/ja/docs/Web/API/File)
- [MDN FileList](https://developer.mozilla.org/ja/docs/Web/API/File)
- [MDN FileReader](https://developer.mozilla.org/ja/docs/Web/API/FileReader)
- [MDN Blob](https://developer.mozilla.org/ja/docs/Web/API/Blob)



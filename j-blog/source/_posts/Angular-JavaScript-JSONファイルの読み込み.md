---
title: '[Angular JavaScript] JSONファイル(複数)の読み込み'
date: 2020-05-31 23:16:54
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

- Angular APにFile APIでローカル端末からファイルを読み込む機能を実装する手法を解説します
- WEB APはセキュリティの関係で制約が多いので、やり方が限られます

<div style="text-align:center;">
<img src="https://miro.medium.com/max/480/1*VKY-Ldkt-iHobItql7G_5w.png" height="90px" width="300px" alt="">
</div>

<!-- toc -->

## 1. 基礎知識

### 1.1. File API
- [File API](https://developer.mozilla.org/ja/docs/Web/API/File)
  - HTML5で定義された、ファイル操作のためのAPI
  - MDN説明
  ```
  File インターフェイスは、ファイルについての情報を提供したり、ウェブページ内の JavaScript でその内容にアクセスできるようにしたりします。
  File オブジェクトは一般的に <input> 要素を使用してユーザがファイルを選択した結果として返された FileList オブジェクトや、ドラッグアンドドロップ操作の DataTransfer オブジェクト、 HTMLCanvasElement の mozGetAsFile() API から情報を取得します。 
  ```
  - 参考
    - [MDN ウェブアプリケーションからのファイルの使用](https://developer.mozilla.org/ja/docs/Web/API/File/Using_files_from_web_applications)

- できないこと
  - ディレクトリの読み込み
    - ファイル単位の読み込みしかできません
  - ユーザーの許可無しの読み込み
    - 自動的にローカルのファイル一覧をWEB AP上に表示するような仕様は実現できませんでした

### 1.2. Native File System API
- Native File System API
    - こちらを導入することで、ディレクトリの読み込みを実現できそうです
    - 今回は採用しませんでした
        - ブラウザによっては動かず、APの利用に前提条件ができてしまう為です
- こちらを利用する場合は以下を参考にどうぞ
    - [ブラウザからドライブにファイルの書き込みができるNative File System APIとは？](https://www.mitsue.co.jp/knowledge/blog/frontend/201909/30_1002.html)

## 2. 実装手順

- 以下を構成を想定
  - sample.component.html
  - sample.component.ts

### 2.1. HTML Fileのinputを用意
htmlにinputボタンを用意します
- sample.component.html
```
<input type = "file" [accept]="'.json'" (change)="selectFile($event)" #fl multiple id ='file'>
```
- ファイルタイプを限定
  - acceptで定義
  ```
  [accept]=""
  ```
- 複数ファイルを取得
  - multiple
- (change)でファイル読み込み時に実行するメソッドを定義
  - $eventに読み込んだファイルのデータが入ります

- 読み込んだファイルを一覧表示
  - 読み込んだファイル名をfileNamesに格納してngForで表示する
```
<div>
  <ng-container *ngFor="let fileName of fileNames; let i = index" >
    <li> No. {{i+1}}　|　Name {{ fileName }} </li>
  </ng-container> 
</div>
```


### 2.2. Fileのデータの取得

- ロジック
  - ファイル読み込み時に(change)イベントでメソッドが発火
  - 引数$eventとしてファイルのデータをメソッドに渡す
  - .target.filesでFileListオブジェクトを取得
  - ファイル数回のループを実行
    - ファイルを読み込むためにFileReaderオブジェクトを生成
    - .readAsText()でFileオブジェクトの読み込みを実行
      - .onloadで読み込み成功時の処理を定義
        - 読み込んだデータ(.result)を配列に格納
      - ..onerrorで読み込み失敗時の処理を定義
        - Errorメッセージを表示

- sample.component.ts
```
export class SampleComponent implements OnInit {

  // 選択したファイルの中身のデータを受け取る変数を宣言
  public filesData = [];

  // File読み込み時に発火するメソッド
  selectFile(evt) {

    // 変数データの初期化
    this.filesData = []; // ファイルの中身のデータ

    // inputしたファイルをFilelistオブジェクトとして取得
    // 複数のfileオブジェクトがselectfilesに入る
    let selectFiles = evt.target.files;

    // --------------fileを読み込む------------------------

    // ファイル数 = selectFiles.length回 ループさせる
    for (let i = 0, num = selectFiles.length; i < num; i++) {

      // 1. ファイルを読み込むためにFileReaderオブジェクトを生成
      // １ファイルしか読み込めない為、ここもループが必要
      const rdr = new FileReader();

      // 2. ファイルをテキストとして読み込む
      rdr.readAsText(selectFiles[i]);

      // 3. 読み込みが成功した際のイベントを定義 loadイベントのハンドラー
      // TSの場合アロー関数でないと動かない
      rdr.onload = (e) => {
        // reader JSONファイルの中身のデータ

        // 4. 読み込んだデータを配列に格納
        this.filesData = this.filesData.concat(rdr.result);
        // console.log(this.filesData); // 配列にファイルの中身が追加されている

      }

      // ファイルの読みこみエラー時の処理 errorイベントのハンドラー
      rdr.onerror = (e) => {
        // console.log('ファイルを読み込めません');
      }
    }
}
```

### 2.3. FileのMetaDataの取得
- 仮定
  - ファイル名を取得して、選択ファイル一覧を表示したい
  - ファイル名はprefixに時刻データがついたものを想定
  ```
  202051820143_XXXXXXX.json
  ```
- sample.component.ts
```
export class SampleComponent implements OnInit {

  // MeataDataを受け取る変数を宣言
  public time = []; // 取得データのタイムスタンプ
  public filesNames = []; // 選択したファイルの情報表示用に名前を格納する

  // 選択したファイルの中身のデータを受け取る変数を宣言
  public filesData = [];

  // File読み込み時に発火するメソッド
  selectFile(evt) {

    // 変数データの初期化
    this.time = []; // 取得データのタイムスタンプ
    this.filesName = []; // ファイル名
    this.filesData = []; // ファイルの中身のデータ

    // inputしたファイルをFilelistオブジェクトとして取得
    // FileListオブジェクトの中に複数のfileオブジェクトが含まれる
    let selectFiles = evt.target.files;

    // データ取得ロジック
    // -----略-------

    // MetaDataを取得するロジック
    for (let i = 0, numFiles = selectFiles.length; i < numFiles; i++) {

      // 1. FileListから単一のfileオブジェクトを抽出
      const f = selectFiles[i]; // fileオブジェクトを格納

      // 2. ファイル名から拡張子を除いた値を取得
      const filename = f.name.match(/(.*)\.json$/)[1];

      // 3. ファイル名を分解して要素情報を取得
      const tmp = filename.split('_');
      // console.log(tmp); // ["202051816143", "XXXXXXX"]

      // 4. 各要素の値を抽出

      // 4.1. タイムスタンプを配列として複数取得

      if (this.time !== tmp[0]) {
        this.time = this.time.concat(tmp[0]);
      }

      // 4.2. XXXXX部分
      this.filesName = this.selectedRegions.concat(tmp[1]);
    }
    console.log(this.filesName);
    // HTMLにfilesNameを双方向バインディング指定していれば、画面に表示される
```
  
### 参考
### 関連記事
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)
- [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)

### MDNドキュメント
- [MDN File](https://developer.mozilla.org/ja/docs/Web/API/File)
- [MDN FileList](https://developer.mozilla.org/ja/docs/Web/API/File)
- [MDN FileReader](https://developer.mozilla.org/ja/docs/Web/API/FileReader)
- [MDN Blob](https://developer.mozilla.org/ja/docs/Web/API/Blob)

### File APIを使用
- [JavaScriptでFile APIをファイル操作方法ついて解説](https://www.e-loop.jp/knowledges/35/)
- [web.dev JavaScriptでファイルを読み込む](https://web.dev/read-files/)

- [MDN ウェブアプリケーションからのファイルの使用](https://developer.mozilla.org/ja/docs/Web/API/File/Using_files_from_web_applications)

- [JavaScriptでJSONファイルを保存する/開く方法](https://slash-mochi.net/?p=3073)
- [Angular FileAPIのonloadではアロー関数を使う](https://mi12cp.hatenablog.com/entry/2018/08/10/002344)

### jQuery or Pythonを使用
- [jsonファイルの読み込み纏め](https://qiita.com/yoshida3/items/36083f7959771b62e9af)

- [How To Read Local JSON Files In Angular](https://www.angularjswiki.com/angular/how-to-read-local-json-files-in-angular/)
  - Angular 4,5 
    - @angular/common/httpのHttpClientとrxjs/ObservableのObservableを使う
    - angularのassetsにファイルを置く前提であり今回とは異なった

### その他
- [ブラウザからドライブにファイルの書き込みができるNative File System APIとは？](https://www.mitsue.co.jp/knowledge/blog/frontend/201909/30_1002.html)


### JSON JavaScriptでの扱い
- [JSONの扱い方を解説！PythonやJavaScript・Swiftでの基本的な使用法とは。配列取得や出力方法も紹介](https://agency-star.co.jp/column/json-parse/)

- [Javascript|JSONでの、レコードの表現方法（配列、連想配列）→{}と[]の違い](https://blog.goo.ne.jp/xmldtp/e/20ee95b2f8c620be2b3a211cc8dd7358)
---
title: '[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける'
date: 2020-09-16 21:15:50
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- 'Angular Material'
- 'ReactiveForm'
- 'mat-input'
thumbnail: https://user-images.githubusercontent.com/68212997/93335972-1f125500-f862-11ea-9519-599094e766bb.png
toc: true
---

- やりたいこと
    - バリデーションで引っかかった内容毎にエラーメッセージを出し分けて、何を直すべきか？をユーザにリアルタイムで教えたい
    - 入力値に不備がある状態で、ユーザにリクエストをさせない、できない仕組みを作る
- 同一のフォームで複数のバリデーションルール毎にエラーメッセージを切り替えるには、テンプレート駆動フォームではなく”リアクティブフォーム”の利用が必要
​
<!--toc-->

## Angularのフォーム　テンプレート駆動/Reactive（モデル駆動）
- フォームの種類
  - テンプレート駆動フォーム
    - フォームの検証ルールをテンプレートとなるhtmlファイルに記載
    - 入力コントロール(Ex. `<input>`)に属性を付けると、それに対応したFormControlオブジェクトやFormGroupオブジェクトが生成される
    - バリデーション
      - 属性で指定(Ex. required)することで、該当するオブジェクトに適応される
    - 仕組み
      - コンポ―ネントから、オブジェクトにアクセス
  - Reactive（モデル駆動）フォーム
    - テンプレート側で
    - ts側に記載
    - メリット
      - 柔軟なバリデーション、入力されたデータの複雑な制御を実現可能
    - デメリット
      - コードが冗長になりがち
    - 使いどころ
      - 同一のフォーム内で複数のバリデーションルール毎にエラーメッセージを切り替えたいケース
        - Ex. 未入力 or 文字数の不足 or 禁止された入力値の型(大文字禁止なのか等)をユーザにリアルタイムで伝えることでUXの向上を図れる
    - 仕組み
      - コンポーネントにあらかじめFormControlオブジェクトやFormGroupオブジェクトを作っておく
      - テンプレートの入力コントロール(Ex. `<input> 要素`)からそれらのオブジェクトを参照

​
## 実装手順
- sampleというコンポーネントがあるものとする​
    - sample.component.html
    - sample.component.ts
​
### モジュールの有効化
​
1. app.component.module.tsでReactiveFormsモジュールをimport
```
// Reactive form
import { ReactiveFormsModule } from '@angular/forms'
// テンプレート駆動型と同様にFormsModuleも必要
​
~略~
​
  importts：[
​
    FormsModule,
    ReactiveFormsModule // 追加
  ]
```
​
2. 対象のコンポーネントでも必要なモジュールをimport
- sample.component.ts
```
import { FormGroup, FormControl, FormBuilder, Validators } from '@angular/forms'
```
- [FormBuilder](https://angular.jp/guide/reactive-forms#formbuilder%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%9F%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD%E3%83%BC%E3%83%AB%E3%81%AE%E4%BD%9C%E6%88%90)
  - 必須ではないが、これを利用すると短く楽に記述できる
​
### 実装例
- Formbuilderを利用する事で完結に記述した例
- ID x Password型のシンプルなフォームは基本以下でよさそう
    - 時間を作れればSchematicsで自動生成できるようにしておきます
​
####　sample.component.ts
- Formbuilderをconstructorで注入
```
// 1. import module
import { FormGroup, FormControl, FormBuilder, Validators } from '@angular/forms'
​
export class  SampleComponent implements OnInit{
​
    // 2.1. FormGroup参照用の変数を宣言
    credentialForm:FormGroup;
​
    // 2.2. FormControl参照用の変数を宣言
    id = new FormControl("", [
        Validators.required,
        Validators.minLength(20)
    ]);
    pass = new FormControl("", [
        Validators.required,
        Validators.minLength(40)
    ]);
​
  // 3. Formbuilderの注入 FormBuilderオブジェクトを生成
  constructor(private fb: FormBuilder) {
    // 4. FormBuilderオブジェクトを使ってFormGroupとFormControlを作成
    this.credentialForm = this.fb.group({
      // formControlと初期値、バリデート条件を列挙
      id: this.id,
      pass: this.pass
    })
  }

    // リクエスト実行時に入力値を参照するサンプル
  submit(){
    // 入力値の参照
    id = this.id.value.id;
    pass = this.pass.value.pass;
    this.sampleService(id, pass); // 認証機能を持つServiceに送る
  }
}
```
​
- 入力値の参照
    - this.formControlname.valueで取得可能になっている
```
this.id.value;
this.pass.value;
```
​
#### sample.component.html
- 入力コントロールとfonrControlオブジェクトの連携、バリデーションに応じたエラーメッセージの表示
    - maxlengthはhtml側に設定すべき
        - ts側でも検知はできたが、規定した時数以上入力を不可にすることはできなかった為
```
<!-- 1. formGroupの設定 -->
<form [FormGroup] = "credentialForm">
​
    <mat-form-field>
    <mat-label>accessKeyId</mat-label>
​
    <!-- 2. FormControlの結びつけ -->
    <input matInput
        formControlName="id"
        placeholder="Ex. XXXXX..."
        maxlength="20">
​
    <!--&&条件を付けないとエラー解消後もメッセージが残ってしまう為注意-->
    <!-- 3.1. エラーメッセージ 空欄,未入力時 -->
    <mat-error *ngIf="id.invalid && id.invalid.required">入力必須</mat-error>
    <!-- 3.2. エラーメッセージ 文字数不足時 -->
    <materror *ngIf="id.invalid && id.errors.minlength">字数不足</mat-error>
​
    <!-- 3. FormControlの結びつけ -->
    <mat-form-field>
        <mat-label>Password</mat-label>
        <input matInput
          formControlName="pass"
          placeholder="Ex. FdOI0..."
          maxlength="40"
          [type]="hide ? 'password' : 'text'"
        >
        <!---pattern="[a-zA-Z0-9!-/:-@¥[-`{-~]*"--->
        <!--目のアイコンの押下で入力値の表示を切り替え--->
        <button mat-icon-button
          matSuffix
          (click)="hide = !hide"
          [attr.aria-label]="'Hide password'"
          [attr.aria-pressed]="hide"
        >
            <mat-icon>{{hide ? 'visibility_off' : 'visibility'}}</mat-icon>
        </button>
        <mat-error *ngIf="pass.errors.required">入力必須</mat-error>
        <mat-error *ngIf="pass.errors.minlength">字数不足</mat-error>
    </mat-form-field>
​
</form>
​
```
​
- 送信ボタンの無効化
    - 一つでもバリデーションエラーがあればdisabledで無効化
    - formGroup名.invalid
        - フォームグループ内で一つでもバリデーションエラーがあればtrue
```
<button 
    (click)="submit()"
    [disabled]="credentialForm.invalid"
>送信</button>
```
- 他のフォームのバリデーションチェックも条件に加える場合
    - ||でor条件を利用する
```
      <button mat-raised-button
        (click)="submit()"
        [disabled]="credentialForm.invalid || checkValidation"
      >Get Data</button>
```

### 備考
- より厳しくバリデーションを設定する場合は、patternと正規表現で大文字のみ、小文字英数、記号を一つ以上含めるといった指定も可能
    - [正規表現を可視化してまとめたチートシート](https://qiita.com/grrrr/items/0b35b5c1c98eebfa5128)

- [フォームコントロールのデータモデルの特定部分を更新](https://angular.jp/guide/reactive-forms#updating-parts-of-the-data-model)
  - setValue()メソッド
    - 値を更新
  - patchValue()メソッド
    - 値を置き換える
​​
​
- 振り返り：詳細なバリデーションチェックを実装することで、以下の効果がある
    - UXの向上
        - 送信ボタンを押して画面遷移を行う前に、リアルタイムで入力値の誤りに気づける
    - エラーハンドリングの実装の簡略化
        - 誤った入力値によるリクエストを未然に防ぐことで、エラーのパターンが減る
    - セキュリティの向上
        - 不正な入力を防げる
​
​
## 参考
### 関連記事
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)

### その他
- [★Angular日本語ドキュメント/リアクティブフォーム](https://angular.jp/guide/reactive-forms)
- [正規表現を可視化してまとめたチートシート](https://qiita.com/grrrr/items/0b35b5c1c98eebfa5128)
---
title: '[Angular] データをファイルで切り出しimportする'
date: 2020-09-25 01:45:11
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
thumbnail: https://user-images.githubusercontent.com/68212997/94175960-5c19bf80-fed2-11ea-978c-a4a15c823186.png
toc: true
---

Angular公式のCoding Guideを読む中で知ったのですが、データをComponentのtsファイル内で宣言するのはアンチパターンで、極力ファイルとして外だしするのがベストプラクティスだそうです。データファイルをComponent側から参照するまでのメモです

<!--toc-->

### 想定する状況

- 元データ
  - componentのtsファイル内でハッシュを宣言しているが、他のcomponentからも参照できるようにファイルとして外だしする
  - 以下はcheckbox一覧として使用するデータの例

```
  regionsData = [ // 個々のselectedプロパティでチェック状態を保持
    { value: 'us-east-1', selected: false},
    { value: 'us-east-2', selected: false},
    { value: 'us-west-1', selected: false},
    { value: 'us-west-2', selected: false},
    { value: 'ca-central-1', selected: false},
    { value: 'eu-central-1', selected: false},
    { value: 'eu-west-1', selected: false},
    { value: 'eu-west-2', selected: false},
    { value: 'eu-west-3', selected: false},
    { value: 'eu-north-1', selected: false},
    { value: 'ap-northeast-1', selected: false},
    { value: 'ap-northeast-2', selected: false},
    { value: 'ap-northeast-3', selected: false},
    { value: 'ap-southeast-1', selected: false},
    { value: 'ap-southeast-2', selected: false},
    { value: 'ap-south-1', selected: false},
    { value: 'me-south-1', selected: false},
    { value: 'sa-east-1', selected: false},
  ];
```

### ファイルにデータを切り出す
- ファイルを作成
  - componentと同様のディレクトリに格納
- regions-data.ts
  - exportで他のcomponentからもアクセス可能な変数として宣言
```
export const regionsData = [
  { value: 'us-east-1', selected: false},
  { value: 'us-east-2', selected: false},
  { value: 'us-west-1', selected: false},
  { value: 'us-west-2', selected: false},
  { value: 'ca-central-1', selected: false},
  { value: 'eu-central-1', selected: false},
  { value: 'eu-west-1', selected: false},
  { value: 'eu-west-2', selected: false},
  { value: 'eu-west-3', selected: false},
  { value: 'eu-north-1', selected: false},
  { value: 'ap-northeast-1', selected: false},
  { value: 'ap-northeast-2', selected: false},
  { value: 'ap-northeast-3', selected: false},
  { value: 'ap-southeast-1', selected: false},
  { value: 'ap-southeast-2', selected: false},
  { value: 'ap-south-1', selected: false},
  { value: 'me-south-1', selected: false},
  { value: 'sa-east-1', selected: false},
];
```

### Componentからファイルをimport

- sample.component.ts
  - ファイルからimport
```
// ファイルをimport
import { regionsData } from './regions-data';
export class SampleComponent implements OnInit {
  // component内で扱う変数にimportしたデータを格納
  regionsData = regionsData;
```


## 参考
### 関連記事
- [[Angular Service入門] ロジックを切り出し、複数Componentで再利用可能にする](/Angular-Service入門-ロジックを切り出し、複数Componentで再利用可能にする/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)


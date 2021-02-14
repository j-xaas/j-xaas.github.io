---
title: AWS SDK for JavaScriptでRegionリストを取得
date: 2021-01-11 18:25:56
categories:
- Serverless Application Dev
- AWS
tags: 
- Angular
- AWS SDK for JavaScript
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84601524-bf4f8b00-aebb-11ea-9bf6-33f5f99da852.png
---

## やりたいこと
  - フロントエンド(SPA)からAWS SDK for JSで任意のアカウントのリージョンリストを取得する
  - SDKの導入については以下を参照
    - [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

- 公式のディベロッパーガイドを参考に実装する
  - [リージョンおよびアベイラビリティーゾーンに関する情報の取得](https://docs.aws.amazon.com/ja_jp/sdk-for-ruby/v3/developer-guide/ec2-regions-availability-zones.html)

## describeRegionsメソッド
- 使用可能なリージョンに関する情報を取得するには、describeRegions メソッドを呼び出せばよい
  - 戻り値
    - Aws::EC2::Types::DescribeRegionsResult オブジェクト
      - DescribeRegionsResult.Regionsに配列が格納されている
      - AWS.EC2.RegionList
      ```
      console.log(data.Regions); // dataに戻り値が格納される
      [object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object]
      console.log(data.Regions[0]);
      [object Object]
      ```
      - EC2.Region.RegionName
      ```
      console.log(data.Regions[0].RegionName);
      eu-north-1
      ```

## 実装例
```
// aws-sdkを変数AWSとしてimport
import * as AWS from 'aws-sdk';

// RegionListを取得するメソッド
  getRegions() {
    // AWS SDKのEC2オブジェクトを宣言
    let ec2 = new AWS.EC2({region: ap-northeast-1}); // Default Regionの指定が必須
    // 問合せ(describeRegions)を実行
    ec2.describeRegions({}, (err, data ) => {
      if (err) {
        let errorMessage = String(err);
        alert(errorMessage);
      }
      else { // データ取得時
        // Aws::EC2::Types::DescribeRegionsResult オブジェクトが戻り値としてdataに格納される
        let regions = data.Regions.map((d) => d.RegionName); // ループでリージョン名を抽出した配列を生成
        return(regions);
      }
    });
  }
```


## 参考
### 関連記事
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

- [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)


### その他
- [リージョンおよびアベイラビリティーゾーンに関する情報の取得](https://docs.aws.amazon.com/ja_jp/sdk-for-ruby/v3/developer-guide/ec2-regions-availability-zones.html)
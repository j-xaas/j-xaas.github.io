---
title: '[Hexo] URLのリダイレクト (hexo-generator-alias/hexo-generator-redirect])'
date: 2020-06-14 21:50:01
category:
- WEB Page Dev
- Hexo
tags:
- Hexo
toc: true
thumbnail: https://media.github-enterprise-9911a.paas1.nec-cloud.com/user/11/files/dd1dd280-ad8b-11ea-8807-e89852f3b1cc
---

<!-- toc -->

## リダイレクトが必要となる理由
Hexoで生成したWEBページのURLはファイル名や設定を変更するだけで簡単に弄れますが、
以下の理由でケアが必要になります

- Googleから重複記事とみなされる
    - せっかく以前のURLでSEO一位でもそのままでは引き継げません
- 古いURLのリンク元から辿ると404 Not Foundになってしまう
![image](https://user-images.githubusercontent.com/41946222/84590736-116cbe00-ae74-11ea-88e8-ced291e619c8.png)


そこで、Googleに元のページのURLを変更したことを伝える（リダイレクト設定をする）必要があります

## リダイレクト設定方法

手動で頑張るのもありですが、Hexoの場合は以下のプラグインを入れれば解決できます

- Plugin
    - [hexo-generator-alias](https://github.com/hexojs/hexo-generator-alias)
        - 直接リダイレクト
    - [hexo-generator-redirect](https://www.npmjs.com/package/hexo-generator-redirect)
        - リダイレクトページを独自に用意

両方試したので解説します。


## リダイレクト設定手順 hexo-generator-alias
### プラグインをinstall
```
npm install hexo-generator-alias --save-dev
```

### リダイレクトの設定方法
- 各記事のymlファイルに宣言
- 設定ファイル(_config.yml)に宣言

### 各記事のファイルでリダイレクトを宣言
- Hexoの各記事のymlファイルの頭についている---で囲まれたFront-matterに alias: を足します
```
---
alias: 旧url/
---
```


### 設定例
- 以下の記事が前URLでSEO一位になっていたので、リダイレクトしてみます

```
---
title: '[オンライン飲み会向け]　Netflix Partyでリモート鑑賞会をする方法'
date: 2020-04-08 22:36:09
category: 
- Tool Tips
- Chrome 拡張機能
tag: 
- Google Chrome
- Netflix Party
toc: true
alias: /2020/04/08/オンライン飲み向け-Netflix-Partyでリモート鑑賞会をする方法/
---
```


- 本番環境にデプロイして、実際に検索欄から見てみます
    - 以下のように前のURL(日付入り)が出てきますが、開くと新しいURLにリダイレクトされました
    - 一瞬、リダイレクト用のページで”移動しました”と表示してからリダイレクトされる仕様はちょっと微妙な気もします

![image](https://user-images.githubusercontent.com/41946222/84591031-3b26e480-ae76-11ea-9a54-ba417af6b85f.png)
  

- 新しいURLにRedirectされました

![image](https://user-images.githubusercontent.com/41946222/84593871-8d253580-ae89-11ea-9630-b7c52a28baec.png)




## リダイレクト設定手順 hexo-generator-redirect
### プラグインをinstall
- hexo-generator-redirectをinstall
```
your-blog> npm install hexo-generator-redirect --save-dev
```

### 各記事のファイルで宣言
- Hexoの各記事のymlファイルの頭についている---で囲まれたFront-matter（設定欄）にredirect_fromを足すだけです
    - 注意点
        - 後ろに/は不要
        - 頭の https://you-blog.github.ioは不要

```
---
redirect_from:
- /old-url1
- /old-url2
---
```
### 例
- 以下の記事が前URLでSEO一位になっていたので、リダイレクトしてみます
```
---
title: '[オンライン飲み会向け]　Netflix Partyでリモート鑑賞会をする方法'
date: 2020-04-08 22:36:09
category: 
- Tool Tips
- Chrome 拡張機能
tag: 
- Google Chrome
- Netflix Party
toc: true
redirect_from:
- /2020/04/08/オンライン飲み向け-Netflix-Partyでリモート鑑賞会をする方法
---
```

### テンプレートの作成
- テーマのlayoutの中に redirect.ejs を作成します
    - パス
    ```
    your-blog\themes\icarus\layout>
    ```
- redirect.ejsを編集
    - 公式の説明に合った例
```
<% const newUrl = full_url_for(page.target.path) %>
 
<h1>Page address was changed</h1>
<p>The new page address is <a href="<%= newUrl %>"><%= newUrl %></a></p>
 
<script type="text/javascript">
  setTimeout(function(){ document.location.href = '<%= newUrl %>'; }, 5000);
</script> 
```

- Hexo gの実行時にリダイレクト様のHTMLが生成されていることを確認可能
    
```
hexo g

（略）
INFO  Generated: 2020/04/08/オンライン飲み向け-Netflix-Partyでリモート鑑賞会をする方法/index.html
```

- 本番環境にデプロイして、実際に検索欄から見てみます
    - 以下のように前のURL(日付入り)が出てきますが、開くと新しいURLにリダイレクトされました
    - 一瞬、リダイレクト用のページで”移動しました”と表示してからリダイレクトされる仕様はちょっと微妙な気もします

![image](https://user-images.githubusercontent.com/41946222/84591031-3b26e480-ae76-11ea-9a54-ba417af6b85f.png)

- 新しいURLにRedirectされました

![image](https://user-images.githubusercontent.com/41946222/84593871-8d253580-ae89-11ea-9630-b7c52a28baec.png)



## 参考
- [hexo-generator-redirect](https://www.npmjs.com/package/hexo-generator-redirect)
- [Hexo の 投稿記事 URL を 変更する](https://azriton.github.io/2017/01/09/Hexo%E3%81%AE%E6%8A%95%E7%A8%BF%E8%A8%98%E4%BA%8BURL%E3%82%92%E5%A4%89%E6%9B%B4%E3%81%99%E3%82%8B/)


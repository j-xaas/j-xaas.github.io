---
title: Hexo 共有機能のPlugin Add Thisで動的なシェアボタンを追加
date: 2020-06-16 22:49:53
category:
- WEB Page Dev
- Hexo
tags: 
- Hexo
- AddThis
- icarus
- NoCode
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84784157-f55c4e80-b024-11ea-98c8-b4500d84cf91.png
---

　Hexoで生成したWEBサイトに共有機能を追加する手法を解説します。今回はAdd ThisというPluginで動的なシェアボタンを追加します。WEBアプリにも適用できるので覚えておくと便利です。HexoとGithub PagesでWEBサイトを作る方法は[こちら](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)

<!-- toc -->

## Add This
- [Add This](https://www.addthis.com/) 
    - WEBページにコンテンツを追加するフリーのサービス
        - ex. シェアボタン, 
    - サイトでポチポチ押しながらデザインを決めると、自動でソースを作ってくれるNo Codeっぽいサービスです

![Add This](https://user-images.githubusercontent.com/41946222/84783273-06f12680-b024-11ea-99ea-a6f71841ea66.png)

- 注意点
    - 上記のサービスで生成した共有ボタンは、Adブロックサービスでブロックされる可能性があります

## 手順
### Account設定

- AddThisにアクセス
- Make it Shareableより”Get Started”を押下
![image](https://user-images.githubusercontent.com/41946222/84784245-102ec300-b025-11ea-8b65-c3a558c9c06e.png)

- Sign In
    - 今回はGoogleアカウントを利用します

![Sign In](https://user-images.githubusercontent.com/41946222/84784570-70be0000-b025-11ea-82f0-f7560a7bd8e2.png)

- Create Account
    - Email AddressとCountryを設定
    - Registerを押下

![Create Account](https://user-images.githubusercontent.com/41946222/84784965-e1fdb300-b025-11ea-958e-621840414a54.png)

- アカウントの設定画面が開きます
![image](https://user-images.githubusercontent.com/41946222/84785193-2be69900-b026-11ea-8b79-3301dea74cde.png)


### シェアボタンを作成
- 上部タブのTools⇒ADD NEW TOOLを選択
    - Share Buttonsを押下

![Select a Tool](https://user-images.githubusercontent.com/41946222/84785585-9dbee280-b026-11ea-8655-935974398883.png)

- 用意されたデザインから追加したいシェアボタンをタイプを選択
    - PCとMobileでそれぞれどのように表示されるか確認可能です

-  Inline
![Inline](https://user-images.githubusercontent.com/41946222/84787565-1aeb5700-b029-11ea-96fc-2857ffb3fcef.png)

- Floating
![Floating](https://user-images.githubusercontent.com/41946222/84787847-6c93e180-b029-11ea-8852-7b8d87e2d07b.png)

- Expanding
    - カーソルを合わせるとにゅっと出てくる

![Expanding](https://user-images.githubusercontent.com/41946222/84788246-e4faa280-b029-11ea-8d96-589db54fca4e.png)


今回はExpandingを使ってみます  

- Continueを押下
- カスタマイズを行いActivateToolを押下

-  TutorialとCodeが表示されます
![Get The Code](https://user-images.githubusercontent.com/41946222/84789303-0c05a400-b02b-11ea-8b76-0ccd3ef42d15.png)


- 各記事のHTMLに以下を貼れば反映されるとのこと
    - Hexoの場合は設定ファイルに以下のsrc=以降のURLを規定するだけで全ページに反映してくれました
```
<!-- Go to www.addthis.com/dashboard to customize your tools -->
<script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-5ee8d1fc16b460fb"></script>
```

- 設定ファイルを変更
    - Hexoディレクトリ直下の_config.ymlではなく、テーマディレクトリの_config.ymlに設定します
    ```
    \your-blog\themes\your-theme\_config.yml
    ```
- _config.ymlのshare欄に設定
```
# Share plugin settings
# https://ppoffice.github.io/hexo-theme-icarus/categories/Plugins/Share
share:
    # Share plugin name
    type: addthis
    install_url: //s7.addthis.com/js/300/addthis_widget.js#pubid=ra-XXXXXXXXXXXXXX
```

以上で設定は完了


## 関連記事
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- [Hexoテーマ(theme)変更: icarus](/Hexoテーマ-theme-変更-icarus/)
- [Hexoにコメント欄を追加(Disqus)](/Hexoにコメント欄を追加-Disqus/)
- [icarus(Hexo Theme) Tips Menuの編集](/icarus-Hexo-Theme-Tips-Menuの編集/)
- [[Hexo] 記事のURLを変更](/Hexo-記事のURLを変更/)
- [Hexo URLのリダイレクトhexo-generator-alias/hexo-generator-redirect](/Hexo-URLのリダイレクト-hexo-generator-alias-hexo-generator-redirect/)
- [Hexoでサイトマップを自動生成 ~ Google Search Consoleへ登録](/Hexoでサイトマップを自動生成-Google-Search-Consoleへ登録/)
- [HexoブログのAMP化【完全版】](/HexoブログのAMP化/)

## 参考
- [Hexo icarus plugins](https://ppoffice.github.io/hexo-theme-icarus/categories/Plugins/Share)
- [Add This](https://www.addthis.com/) 
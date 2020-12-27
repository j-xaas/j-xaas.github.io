---
title: '[C言語入門] コンパイル〜文字列の出力まで'
date: 2020-09-28 20:46:04
category:
- Others
- C
tags:
- C
thumbnail]: https://user-images.githubusercontent.com/68212997/94435240-d483c780-01d5-11eb-8b31-496780188c8e.png
toc: true
---

<!--toc-->

- これからC言語を学び始めたい方向け
- C言語のコンパイラを入れて、"Hello World"を表示する簡単なプログラムを作るまで

## 環境構築: コンパイラ(gcc)のインストール
- C言語はコンパイラのインストールのみでOK

### Cent OSの場合
```
$ yum install gcc
```

### macOSの場合
- 初めからgccコマンドが利用できました

## 実装
- HelloWorldを表示させるまで

### Cプログラムのコンパイル
- C言語で書いたプログラムをコンピュータが理解できる言語に変換することで実行可能になる
    - この変換をコンパイルという

- [gcc( GNU Compiler Collection（gcc)](](https://webkaru.net/clang/linux-compiler-gcc-install/))
    - フリーのコンパイラ
    - 主なオプション
        - -g (コンパイル，リンク時)
            - コンパイル，リンク時にDEBUG情報を付加する．dbx,gdbなどのデバッガ を使用するときに必要．
        - -c (コンパイル時)
            - コンパイルのみ行う(オブジェクトファイルを生成する)
            ex: gcc -c test.c　　　→ test.oを生成
        - -o test.o (コンパイル時)(または-o test(リンク時)
            - コンパイルして，その結果をtest.oとする．または，リンクしてその 結果をtestとする．省略時は，-cがついていればtest.o (オブジェクトファイル)，-cがついていなければa.out(実行ファイル)．
            - ex: gcc -c test.c -o test2.o　　→test2.oという オブジェクトファイルを生成

### 文字列の出力
- C言語における文字列の出力方法
    - [printf 書式](https://www.sejuku.net/blog/24934)
    ```
    printf(“書式文字列”, 変数1, 変数2, ・・・)
    ```
    - 変数から文字列 x 3を表示
    ```
    printf("%s%s%s", str1, str2, str3);
    ```
    - 改行コード \n
    - printf関数を用いるには`<stdio.h>`っというヘッダファイルを取り込む必要がある

### Cプログラムの頭につける”#include <studio.h>”について 
- `#include <studio.h>`
    - [#include <stdio.h>はおまじないじゃないぞ。](https://qiita.com/HirosuguTakeshita/items/8893b2b8a7f05c67482a)
    - `<stdio.h>`というヘッダーファイル
        - 標準Cライブラリと呼ばれる”ライブラリ”
        - 汎用性の高い複数のプログラムを再利用可能な形でひとまとまりにしたもの
            - 簡単にいうと説明書をたくさん保管してある図書館
    ![<stdio.h>とは](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F333871%2F693f304d-d370-32ab-ae9c-34340479ba4b.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&s=ca4a8f86f7e4b2bed4ed0c414817df44)
    - 説明引用
    ```
    基本的に、関数はプログラム内で定義しないと使えません。もちろん、printf()だって関数ですから、本来であればプログラム中で定義しないと使えないわけですが、頻繁に使う関数は前もって定義してまとめておいていつでも引っ張ってこれるようにした方が効率がいいわけです。
    それを実現したのが”ライブラリ”と呼ばれるファイルです。
    ```

### C言語における関数の作り方
- 関数名の前には`戻り値の型`を指定
- 返す値がなければ型の指定の代わりに「void」句を記述。
- 戻り値ありの場合
    ```
    型名 関数名(型名 引数1, 型名 引数2, ・・・) {
        処理
        return オブジェクト
    }
    ```
- 戻り値なしの場合
    ```
    void 関数名(型名 引数1, 型名 引数2, ・・・) {
        処理
    }
    ```
- 引数には変数やポインタ、構造体の実体のように処理で使用するオブジェクトの型と引数名を記述


### プログラムを開発
- ファイルを生成
```
touch sample.c
```

- sample.cを編集
    - 今回は返す戻り値が無いのでvoid型
```
#include<stdio.h>
void main() {
  printf("Hello World\n");
}
```
### コンパイル
- gccコマンドを実行
    ```
    gcc -o sample sample.c
    ```
    - 指定した名称のファイルが生成された
    ```
     ls
    sample.c  sample
    ```

- 実行
```
# ./sample
Hello World
```

#### 参考
- [LinuxでC言語 - コンパイラ（gcc）のインストール](https://webkaru.net/clang/linux-compiler-gcc-install/)
- [コンパイラ(gccコマンド)の使い方](http://www.ysr.net.it-chiba.ac.jp/data/cc.html)
- [#include <stdio.h>はおまじないじゃないぞ。](https://qiita.com/HirosuguTakeshita/items/8893b2b8a7f05c67482a)


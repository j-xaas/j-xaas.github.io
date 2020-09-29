---
title: '[vscode macbook(zsh)] codeコマンドでCLIからファイルを開閉可能にする'
date: 2020-09-16 20:43:44
categories:
- Tool Tips
- vscode
tags: vscode
thumbnail: https://user-images.githubusercontent.com/68212997/93333648-95ad5380-f85e-11ea-9cce-364ed97e35f7.png
---

vscodeを入れたら初めにする設定のメモ。CLIからcodeコマンドでファイルを開けるようにします。

## 環境
- zsh 
- macOS
- MacBook Pro(2020)

## 設定
- 設定ファイルを開く
```
vi ~/.zshrc
```
- 以下を追記
```
function code {
    if [[ $# = 0 ]]
    then
        open -a "Visual Studio Code"
    else
        local argPath="$1"
        [[ $1 = /* ]] && argPath="$1" || argPath="$PWD/${1#./}"
        open -a "Visual Studio Code" "$argPath"
    fi
}
```
- 以下を実行
```
source ~/.zshrc
```

- 以上でcodeコマンドによるファイルの開閉が可能になった
```
code <filepass>
```

- 参考
    - [zshを使っていてVSCodeのcodeコマンドを使用する方法](https://qiita.com/sayama0402/items/453595d0d8f54b645753)
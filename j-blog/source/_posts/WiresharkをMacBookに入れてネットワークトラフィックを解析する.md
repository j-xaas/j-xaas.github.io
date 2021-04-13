---
title: WiresharkをMacBookに入れてネットワークトラフィックを解析する
date: 2021-01-11 15:48:59
categories:
- Tool Tips
- Wireshark
tags: 
- Wireshark
- Network
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/104151714-118c6680-5421-11eb-844a-3077a1662830.png
---

WiresharkのCLI＆GUI版をMacBookに入れて、解析&シーケンス図生成するまでのメモ

<!--toc-->

## CLI版（Tshark）
CLI版はGUI版より高速

- 以下からインストーラーをダウンロードして実行
    - [Wireshark download](https://wireshark.jp.uptodown.com/mac/download)
- chomodBPFインストーラを実行

- 以下のコマンドを実行
```
brew install wireshark
```

- 動作チェック
```
MacBook-Pro ~ % tshark -D
1. en0 (Wi-Fi)
2. p2p0
3. awdl0
4. llw0
5. utun0
6. utun1
7. utun2
8. utun3
9. en5
10. lo0 (Loopback)
11. bridge0 (Thunderbolt Bridge)
12. en1 (Thunderbolt 1)
13. en2 (Thunderbolt 2)
14. en3 (Thunderbolt 3)
15. en4 (Thunderbolt 4)
16. gif0
17. stf0
18. ap1
19. ciscodump (Cisco remote capture)
20. randpkt (Random packet generator)
21. sshdump (SSH remote capture)
22. udpdump (UDP Listener remote capture)
```

## GUI版(Wireshark.app)
- 以下を実行すればOK
```
brew cask install wireshark
```

## 活用例
- まずはネットワークトラフィックを取得する
    - tcpdumpでトラフィックをdumpファイルとして出力する例
        - .pcap拡張子で保存することでWiresharkでそのまま開くことができる
        - 以下の実行後に解析した通信を実行する
    ```
    tcpdump -i ens192 -s 1024 -w tcpdump.pcap
    ```

### Wiresharkでトラフィックを解析
- 取得したファイルを開く
    - 以下の様にトラフィックを確認できる

![Wireshark](https://user-images.githubusercontent.com/68212997/104151714-118c6680-5421-11eb-844a-3077a1662830.png)

### pcapファイルからシーケンス図を作成
- メニューバーの”統計” → ”フローグラフ”から以下を表示できる。PDF保存も可能
![Wireshark シーケンス図](https://user-images.githubusercontent.com/68212997/104152827-06870580-5424-11eb-8f5c-c4350671b5e8.png)


- 詳細は以下を見て使いながら覚えていけばOK
[Wiresharkチートシート](https://dementium2.com/page-11/wireshark-2-3/)


### 参考
- [Wiresharkチートシート](https://dementium2.com/page-11/wireshark-2-3/)
- [Wiresharkで見れるTCPシーケンス図](http://sweeksky.blog.jp/archives/1073893109.html)
- [MacでWiresharkをインストールする方法(GUI & CLI)](https://qiita.com/y-vectorfield/items/3aaaa98eb2aeca223ecf)
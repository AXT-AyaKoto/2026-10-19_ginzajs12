---
theme: dark
hue: 280
width: 1920
height: 1080
fontSize: 30
break: soft
footer: "JavaScriptで競技プログラミングに挑戦する、ということ ― Ayasaka-Koto (AyaExpTech) @Ginza.js#12"
pageTransition:
    type: fade
    duration: 0.15
animation:
    duration: 0.15
rawHTML: true
---

<!-- presen-it! slide-break (center-x=true) -->

# JavaScriptで競技プログラミングに挑戦する、ということ

2026-10-19 ― Ayasaka-Koto (AyaExpTech) @Ginza.js#12

<!-- presen-it! slide-break -->

## 諸注意

- タイトルでJavaScriptって言ってますが、実際私が書いてるのはTypeScriptです
    - 「Ginza.**js**」なのでJavaScriptって書きました
    - JavaScriptって言ったりTypeScriptって言ったりしますが、適宜読み替えてください

<!-- presen-it! slide-break -->

## JS/TSは色んな場所で使える

- Webフロントエンド
    - もともとの役割はこれですよね
- アプリ開発
    - デスクトップ: Electron, Tauri, ...
    - モバイル: React Native, Capacitor, ...
- バックエンド (Node.jsとかで……)
    - エッジだけどCoudflare Workersとかもありますよね

<!-- presen-it! slide-break -->

## 競技プログラミング

ところで、競技プログラミングというものがあります

> 決められた条件のもとで与えられた問題、課題をプログラミングを用いて解決し、その過程や結果を競うもの ……(後略)
> (https://info.atcoder.jp/overview/about/competitive)

- 問題が出て、それを解くプログラムを書く速さとか正確さを競う頭脳スポーツ
- コンテストサイトの1つに「AtCoder」がある
- 色んな言語が使える
    - [AtCoderで使用出来る言語とライブラリの一覧 - 2025/10 (img.atcoder.jp)](https://img.atcoder.jp/file/language-update/2025-10/language-list.html)
- 私は2年ちょっと前にこれを知って「面白そう」と思いました

<!-- presen-it! slide-break -->

## TypeScript × AtCoder ?

- いろんな言語使えるなら「自分のメイン言語」でやりたいですよね
    - 私はTypeScript(当時はJavaScript)がメイン言語だったので、そうしたいと考えました
    - 競プロで主流の言語はC++ ≫ Python > Java > Rustって感じだが、どれも書けない or 書きたくない
- ジャッジシステムとして対応はしていそう
    - Node.js 22.19.0, Deno 2.4.5, Bun 1.2.21が使える
    - JavaScriptもTypeScriptも使える<!-- presen-it! reveal (at=1) -->
- **やろう！**
    - この判断で私は茨の道を歩むことに……
    - 今日はその“茨の道を歩んだ歴史”を話します

<!-- presen-it! slide-break (center-y=true) -->

<!-- presen-it! column-break -->

## TypeScript競プロerの受難

<!-- presen-it! slide-break -->

## そんなにでかくない受難

- 環境構築がめんどくさーい！
    - 問題ページにエディタ埋め込む拡張機能を自作しました
        - [AtCoder In-Browser Playground (AIBP)](https://github.com/AXT-Studio/AtCoder-InBrowser-Playground)
        - 右の画像はAIBPのスクショです
    - たぶん今ならCLI経由での提出もそんなに難しくないはず
        - [Nanashi.さんの`cpcli`](https://sevenc7c.com/competitive-programming-cli/)とか
        - 当面の間はAIBPをDogfoodingするつもり
- 解説にくっついてるコードがだいたいC++かPython
    - AIに聞けば丁寧に解説してくれる 良い時代だ
    - "答え言うなヒントだけ出せ！"っていう指示を用意した

このへんは、まあ……

<!-- presen-it! column-break -->

![](https://addons.mozilla.org/user-media/previews/full/387/387941.png?modified=1783799252)

<!-- presen-it! slide-break (center-x=true) -->

## 圧倒的くそでか受難

<!-- presen-it! reveal (at=1) -->

<b style="font-size:3em">ライブラリの整備不足</b>

<!-- presen-it! slide-break -->

## ライブラリの整備不足、とは……？ (1)

- “AtCoder Library”というAtCoder側でいくつかのアルゴリズムを実装したライブラリがある
    - C++ではそれが使える
    - 他のいくつかの言語にも移植されている
- JS/TS → 移植はある……が、メンテは滞っているし機能も不足している
    - [ac-library-js](https://github.com/e6nlaq/ac-library-js)というのがあり、AtCoderのジャッジでも使える
    - 最終更新: 2025年1月
    - ちゃんとしたドキュメントがない
    - いくつか「AtCoder Libraryにはあるけど`ac-library-js`にはない」ものがある

<!-- presen-it! slide-break -->

## ライブラリの整備不足、とは……？ (2)

というかそれ以前に、「他の多くの言語にはあるけどJS/TS標準にはない」が結構ある

- Queue, Deque
    - Arrayでいいじゃんと思った方へ: それだと役不足です あとで話します
    - (実は`data-structure-typed`が使えるんですが、英語ドキュメントは個人的に読むのがだるくて……)
- 平衡二分探索木
    - C++の`std::set`, `std::map`、Javaの`TreeSet`など
    - Pythonは標準にはないが、代替として[tatyam set](https://qiita.com/tatyam/items/492c70ac4c955c055602)が有名
- Priority Queue
    - C++の`std::priority_queue`、Javaの`PriorityQueue`、Pythonの`heapq`など
- 二分探索
    - C++の`lower_bound`、Javaの`Arrays.binarySearch`、Pythonの`bisect`など

<!-- presen-it! slide-break -->

## ないなら作ろう

- 使いやすくて、かつ実用的なものを作りたい！
    - 既存ライブラリのドキュメント整備すればそれで済む話な気もするが……
- きっと自分の勉強にもなる
    - ネタバレ: この判断は正しかった

<!-- presen-it! slide-break -->

## AyaExpTech Arcane

コード見たいよ〜！って方のために先に共有

- 読み: /éɪéksték ɑːkéin/
    - "axt arcane"で調べれば出てくる
    - `axtech.dev`(私の個人サイト)にリンク載ってる

<!-- presen-it! column-break (center-x=true) -->

![](https://api.qrserver.com/v1/create-qr-code/?size=320x320&qzone=4&data=https://jsr.io/@ayaexptech/arcane)

https://jsr.io/@ayaexptech/arcane

<!-- presen-it! slide-break -->

## 前提知識: 各問題の難易度感

AtCoder Beginner Contestは現在7問(100分)セット

- A: 入出力とifとfor書ければプログラミング初心者でもいける
- B: 競プロ未経験でもプログラミングある程度やってればいけると思う<!-- presen-it! reveal (at=1) --> **←ここまではいける**<!-- presen-it! reveal -->
- C: ここからアルゴリズムとかの勉強が必要。プログラミング経験者にとって最初の壁<!-- presen-it! reveal (at=1) --> **←まずここを突破しないといけない**<!-- presen-it! reveal -->
- D: むずい。今の私はだいたい解ける
- E: もっとむずい。今の私はたまに解けることがある
- F: うーーん解法の見当がつかない
- G: 私が解けるようになる日は来るのでしょうか

<!-- presen-it! slide-break -->

## C問題頻出 ∩ JS/TS標準にない

他にもあるかもしれないが、とくにつらいのはこのへん

- Queue, Deque
    - 当然あるものとして出題されている ないんですけど！
    - 説明しやすそうなのでこのあとこれを題材にちょっと喋りますね
- Priority Queue
    - 当然あるものとして出題されている ないんですけど！ - コピー
- 順列全列挙
    - たまに出てきて散る
- ソート済み配列上の二分探索(`lower_bound`, `upper_bound`)
    - 二分探索くらい書けよって話ではあるんけど、ここでライブラリ勢と数分差付くの嫌だよね〜
- 平衡二分探索木(`std::set`)
    - 回避できることがほとんどだが、その回避先がQueueだったりして悲しい

<!-- presen-it! slide-break -->

## Queue, Dequeって何？

<!-- presen-it! slide-break -->

## memo: Directives

```md
<!-- presen-it! slide-break (center-x=true&center-y=false) -->
<!-- presen-it! column-break (center-x=true&center-y=false) -->
<!-- presen-it! reveal (at=1) -->
```

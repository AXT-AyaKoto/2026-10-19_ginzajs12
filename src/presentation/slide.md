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

## 自己紹介

綾坂こと (Ayasaka-Koto)

- 所属: ZEN大学 2年生
- AtCoder (Algo) Highest Rate <b style="color:#00C0C0">1238</b><!-- presen-it! reveal (at=1) --> **←今日はこれの話**<!-- presen-it! reveal -->
- 趣味: 音ゲー, Vtuber(hololive)
- Website: https://axtech.dev/

<!-- presen-it! column-break (center-x=true) -->

![](https://axtech.dev/icons/Ayasaka-Koto/real.webp)

<!-- presen-it! slide-break -->

## 競技プログラミング

突然ですが、この世界には「競技プログラミング」というものがあります
(名前だけ知ってるけどやったことない、みたいな人もいそう)

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
- JS/TS → 移植はある……が、不安要素もある
    - [ac-library-js](https://github.com/e6nlaq/ac-library-js)というのがあり、AtCoderのジャッジでも使える
    - メンテが滞っていそう……？(最終更新: 2025年1月)
    - ちゃんとしたドキュメントがない
        - DeepWikiのインデックス登録はされているが、2025年10月のルール変更でAI使用はほぼ禁止になったのでNG
    - いくつか「AtCoder Libraryにはあるけど`ac-library-js`にはない」ものがある
        - `ac-library-js`にあるけど性能(処理速度)に難あり、みたいなものもある

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

## よく出る ∩ JS/TS標準にない

他にもあるが、とくにつらいのはこのへん

- Queue, Deque
    - 当然あるものとして出題されている ないんですけど！
    - 説明しやすそうなのでこのあとこれを題材にちょっと喋りますね
- Priority Queue
    - 当然あるものとして出題されている ないんですけど！ - コピー
- 順列全列挙
    - たまに出てきて散る
- Segment Tree (配列の1点更新と区間クエリを高速に処理する)
- Disjoint Set Union (Union-Find) ("グループの関係"を高速に扱う)

<!-- presen-it! slide-break -->

## Queue, Dequeって何？ (1)

キューって義務教育だと思ってるんですけどそんなことない？

|       | 先頭挿入 | 先頭取り出し | 末尾挿入 | 末尾取り出し |
| :---: | :------: | :----------: | :------: | :----------: |
| Stack |    ❌️    |      ❌️      |    ✅️    |      ✅️      |
| Queue |    ✅️    |      ❌️      |    ❌️    |      ✅️      |
| Deque |    ✅️    |      ✅️      |    ✅️    |      ✅️      |

DequeはQueueとStackの上位互換なので、Dequeだけ持っとけばOK

<!-- presen-it! column-break (center-x=true) -->

![](./assets/what-is-queue-deque.webp)

<!-- presen-it! slide-break -->

## Arrayでよくない？

👉️結論から言うね。それ、**\*\*鋭い\*\***。

- JSのArrayは「非負整数文字列をキーとして値を持っているObject」
- それに対して先頭取り出し(`shift`)を実装するには、大まかになにをすればいい？<!-- presen-it! reveal (at=1) -->
    1. `length`を1減らす<!-- presen-it! reveal (at=2) -->
    2. `0`番目を捨てる<!-- presen-it! reveal (at=3) -->
    3. `1`番目以降を1個ずらす<!-- presen-it! reveal (at=4) --> **←ここめっちゃ時間かかってない？**<!-- presen-it! reveal (at=5) -->
- 配列の長さにおおよそ比例する時間がかかる
    - 厳密には上下から配列の長さの定数倍で抑えれるだけだけど、だいたい比例だと思っていい
    - 競プロでは長さ20万の配列に20万回操作しろとか言われる
        - 操作ごとにずらす操作やってたらおおよそ何百億回の操作が必要($(2 \times 10^5)^2 = 4 \times 10^{10}$)
        - だいたいの問題は実行時間制限(2秒が多い)がついてるので、間に合わない
- 余談: Stackは別にArrayを使っていい
    - 末尾いじるだけならずらす処理は発生しないので

<!-- presen-it! slide-break -->

## どうやって作る？

### 双方向連結リスト

- 前後への参照を持ったノードを繋いで管理する
    - `Node<T> = {curr: T, prev?: Node<T>, next?: Node<T>}`
    - `List<T> = {head?: Node<T>, tail?: Node<T>}`
- C++の`std::list`がこれ

<!-- presen-it! reveal (at=1) -->

一回採用したけどちょっと重かったのでやめた
<!-- presen-it! reveal -->

<!-- presen-it! column-break -->

### Two-stack Deque

- 2つのStackを内部で持つ
    1. 先頭側を**逆順で**保持するStack
    2. 末尾側を**正順で**保持するStack
- 取り出し時に要素がなければ、もう一方から半分移動する

<!-- presen-it! reveal (at=1) -->

こっちを採用！

<!-- presen-it! slide-break -->

## 余談: Two-stack Dequeは本当に$O(1)$なんですか？

時間余ってたら話す

- 最悪$O(1)$ではない
    - 移す処理をするときに、最大でDequeの長さの半分に概ね比例する時間がかかる(最悪$O(N)$)
- 償却解析をすると、ならし$O(1)$とは言える
    - 移す処理の発動には取り出し操作が必要で、そこで事前にコストを払ったことにする(均す)
    - コストを均すと一定以下に収まるので、償却$O(1)$<!-- presen-it! reveal (at=1) -->
- 償却$O(1)$になってない状態でしばらく使ってた
    - あっぶね〜〜〜！！
    - 当時は「半分移動」じゃなくて「全部移動」としていた
    - Queueの代替として使ってると償却$O(1)$だったのもあって気づかなかった
    - AIが気づいてくれて、現在は修正済み

<!-- presen-it! slide-break -->

## いっぱい作る

こんな感じの「本質的に速くするための実装」をいっぱいやる

<!-- presen-it! reveal (at=1) -->

- **Priority Queue** → `BinaryHeap`, `BinaryHeapLite`
- **配列上の二分探索** → `BinarySearch`
- **二項係数(組み合わせ) mod p** → `Combination`
- **畳み込み** → `Convolution`
- Deque → `Deque`
- グラフ → `DirectedGraph`, `UndirectedGraph`など
    - **強連結成分分解**: `DirectedGraph.getSCC()`
- **Disjoint Set Union (Union-Find)** → `DisjointSet`
- 数学関連 → `ExtendedMath`
    - 最大公約数、素数判定、**中国剰余定理**、BigInt関連……
- "区間をSetで管理するやつ" → `IntervalSet`
- 累積和 → `Iteration.accumulate()`
- 順列全列挙 → `Iteration.next_permutation()`
- bit全探索 → `Iteration.next_product()`

<!-- presen-it! column-break -->

(太字: AtCoder Libraryにあるやつ)

- Implicit Treap → `ImplicitTreap`
- **遅延セグメント木** → `LazySegmentTree`
- 線形篩(素数列挙) → `LinearSieve`
- **最大流問題ソルバー** → `MaxFlow`
- **最小費用流問題ソルバー** → `MinCostFlow`
- **積極的にmodを取る演算** → `ModOps`
- ローリングハッシュ → `RollingHash`
- **セグメント木** → `SegmentTree`
- スパーステーブル → `SparseTable`
- 文字列関連 → `StringOperations`
    - ランレングス圧縮、**Suffix Array**、**LCP Array**、**Z Array**
- Treap → `StringOperations.getSuffixArray()`
- **2-SATソルバー** → `TwoSAT`
- 2次元ベクトル → `Vector2DFloat`, `Vector2DInt`

<!-- presen-it! slide-break -->

## ver2.0.0 公開

そして先日(10月XX日)、ついにver2.0.0を公開！

- ver2.0.0をもって、**AtCoder Libraryが持つライブラリと同等の機能をすべて持つ**ライブラリになった
    - 最後まで残っていた`Convolution`を実装しました
    - それ以外のものもいっぱい入ってるけど、準公式ライブラリをカバーしたことは大きなマイルストーンでしょう
- **JS/TS標準 + AXT Arcaneだけで戦える**ようになったと言っていい
    - というか私はそれで戦ってる

<!-- presen-it! slide-break -->

## memo: Directives

```md
<!-- presen-it! slide-break (center-x=true&center-y=false) -->
<!-- presen-it! column-break (center-x=true&center-y=false) -->
<!-- presen-it! reveal (at=1) -->
```

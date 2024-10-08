# 基本２

| アルゴリズム | 最悪計算量 | 平均計算量 | 最良計算量 |最悪空間計算量 | インプレース | 安定性 | Pros | Cons |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| バケットソート | $$O(n^2)$$ | $$O(n + k)$$ | $$O(n)$$ | $$O(nk)$$| No | Yes | 線形時間でソートできる | バケット数が固定または要素数が固定でないと使いにくい |
| カウントソート | $$O(n + k)$$ | $$O(n + k)$$ | $$O(n + k)$$ |  | No | Yes | 線形時間でソートできる | 要素の取りうる値が制限されている必要がある |
| 基数ソート | $$O(nk)$$ | - | $$O(n)$$ | $$O(nk)$$ | No | Yes | 線形時間でソートできる |  |

## 擬似ソート（比較以外の方法によるソート）

前述したように値の比較を基本操作とするソートでは最速でも$$O(n \log n)$$の最悪時間計算量を要するが、比較操作を使わないソートであればより高速（$$O(n)$$）にソートすることができる。そこで必要になるのが、値同士の相対的関係ではなく、値そのものに基づいてアルゴリズムの動作を決めることである。具体的には、値（またはそれに一定の変換を加えたもの）を添字として配列の参照を行う。

比較を用いるソートではソート対象のデータが全順序集合（順序を定めることができる値の集合）であればそれ以上の制約を必要としないが、比較を用いないソートではそれ以上の制約が必要になるため、使い所が限られるのが欠点である。

### バケットソート

バケットソート（bucket sort、バケツソート、ビンソート）では、ソート対象のデータの取りうる値が$$m$$種類であるとき、$$m$$個のバケツを用意し、データ列を走査して、各データを対応するバケツに入れていく。この処理が終わった後、ソートしたい順序に従ってバケツから値を取り出して結合することで、データをソートすることができる。安定ソートを実現するためには、同じバケツに入っているデータは入れたときと同じ順序で取り出す必要がある。

バケツ数を$$k$$個とすると、バケットソートの時間計算量は$$O(n + k)$$であり（$$n$$個の要素を走査して、次に$$k$$個のバケツを結合するため）、要素数が固定またはバケツ数が固定であるなら線形時間（$$O(n)$$）でソートできることになる。ただし、要素の取りうる値が$$k$$種類であるという入力に対する強い制約を持ち、ソート対象の値のモデルに合わせてプログラムを書く必要が生じてしまうため、一般的なソートアルゴリズムとしては使いにくい。

本来はバケツ内でもソートが必要ない場合バケツ内でソートをする必要があるとき、データが非均一で特定のバケツにのみデータが集中し、そのバケツ内でさらに効率の悪いソート（$$O(n^2)$$）を行った場合（例えばクイックソートを使っていて最悪のケースが発生したときなど）、バケットソートの最悪計算量は$$O(n^2)$$になる。

[バケットソート](https://ja.wikipedia.org/wiki/バケットソート)

[Top K Frequent Elements - LeetCode](https://github.com/rihib/leetcode/pull/20)

### カウントソート

カウントソート（counting sort）はバケットソートの一種でもあり、ソート対象のデータに特定の範囲の値しか含まれていないことがわかっている場合、それぞれの値が何回現れるかをカウントすることによってソートするアルゴリズムである。

### 基数ソート

基数ソート（radix sort）は、各桁に対してソートを行い、桁ごとに整列を繰り返すことで全体をソートする。桁ごとのソートにはバケットソートやカウントソートを用いる。

このとき、最小桁から順にソートしていく必要がある。各桁ごとのソートでは他の桁の情報を知ることができないので、最大桁からソートをすると、より小さい桁のソート結果が優先して使われることになってしまう。例えば、$$45, 17, 72$$というデータ列があるとき、最大桁からソートする場合、$$45, 17, 72$$→$$17, 45, 72$$→$$72, 45, 17$$となってしまう。最小桁からソートする場合は、$$45, 17, 72$$→$$72, 45, 17$$→$$17, 45, 72$$となり、正しいソート結果が得られる。

またソートに用いるバケットソートやカウントソートは安定ソートである必要がある。不安定ソートだと一度ソートされた順序が後のステップで維持されない。例えば$$45, 17, 42$$というデータ列があるとき、不安定ソートを使った場合、$$45, 17, 42$$→$$42, 45, 17$$→$$17, 45, 42$$のようになる可能性があり、より小さい桁でソートした順序が維持されることを保証できない。

もちろん桁数の異なる数字同士であっても基数ソートを適用することができる。例えば$$170, 45, 75$$というデータ列があるとき、$$170, 45, 75$$→$$170, 45, 75$$→$$45, 170, 75$$→$$45, 75, 170$$となる。他にも文字列を辞書式順序にソートする場合にも基数ソートを使うことができる。例えば$$"dog", "cat", "ape"$$というデータ列があるとき、$$"dog", "cat", "ape"$$→$$"ape", "dog", "cat"$$→$$"cat", "dog", "ape"$$→$$"ape", "cat", "dog"$$となる。文字列をソートする場合は、文字数が異なる場合には短い方の文字列の後ろに空白文字を追加して文字数を揃えるなどの追加の処理を行う必要がある（しかし、数列をソートする場合も、結局は各桁ごとに処理するために一旦は文字列に変換して処理することに注意）。

$$k$$を桁数（文字数）とすると、各桁ごとに行うバケットソートやカウントソートの時間計算量はバケツ数が固定であることから$$O(n)$$であり、それを$$k$$回繰り返すため、基数ソートの時間計算量は$$O(nk)$$になるが、$$k$$は固定値であるため、実際は$$O(n)$$であると言える。そのため、実用的なアルゴリズムとしては最高速である。ただし、例えばクイックソートと比べてどちらが速いかは慎重な検討を要する。$$k$$の大きさや個々の操作に要する時間によっては、クイックソートの方が速いこともありうる。

[このスライド](https://www.docswell.com/s/kumagi/KYWWPE-fear-l1-cache)も面白いので見ておくと良い。最近のCPUではTAGEという分岐予測技術が使われている（[参考](https://pc.watch.impress.co.jp/docs/column/kaigai/1192296.html)）。

[基数ソート](https://ja.wikipedia.org/wiki/基数ソート)<br>
[GPU最速ソート! Radix Sort その①](https://qiita.com/tommyecguitar/items/3c1897bceda4a06beef2)

## マッチング

テキストエディタにおける探索コマンド（Cmd + Fなどで文字列検索する場合など）やテキストデータベースにおける検索などに使われる。テキストと呼ばれる文字列の中にパターンと呼ばれる文字列に一致する部分があるかを調べる。テキストはパターンよりも遥かに大きいことが多い。テキストは100万文字以上になることも珍しくなく、対してパターンは10文字程度であることも多い。

計算量を気にしなければ、最もシンプルな方法では、テキストにパターンを重ねて一致するかどうかを調べ、一致しなければ重ねる位置を一つずつずらしていけばいい。この場合、テキスト（$$n$$）に比べてパターン（$$m$$）が十分に小さいとすれば、最悪時間計算量は$$O(nm)$$になる。ただし、実際はパターンの１文字目か２文字目で不一致が判明する場合がほとんどであるため、実用的に見てそれほど問題にならないことが多い。ただ、最悪計算量が$$m$$に比例する点に改良の余地が残っている。

最悪計算量が$$O(nm)$$になるのは、ある位置にパターンを置いて比較した際に得られる情報を十分活用していないためである。例えば、`aaaaa`というテキストと`aaab`というパターンがあるとき、最初の`a`を比較した時点で2文字目と３文字目も`a`であることがわかっているのに、この情報を活用せずに、次のステップでまたパターンの１文字目とテキストの２文字目を比較することになる。マッチングの計算速度をあげるためには、一致するまたは一致しないことがわかっている無駄な比較をできるだけ避けることが重要である。

### 効率的な文字列マッチング（KMP、BP）

#### KMP法

KMP（Knuth-Morris-Pratt）法の比較の回数は最大$$2n$$回であることから、計算量は$$m$$によらずに$$O(n)$$になり、シンプルなマッチング方法の$$O(nm)$$の最悪計算量を回避することができる。実際の文字列処理では両者の計算時間にオーダーで見られるほどの差が出ることは少ないが、やはりKMP法の方が若干速いと言われている。ただ、KMP法は後述するBM法に比べると実用上の性能で劣る。

シンプルなマッチング方法では、途中まで一致して不一致となった場合に、テキスト状を少し引き返してから再度比較を始める必要があるが、KMP法の長所はテキストの上を逆戻りする必要がないため、ファイルからテキストを入力しながら探索を行う場合などにこの性質によって処理が簡単になることがある。

#### BM法

BM（Boyer-Moore）法は、KMP法よりもさらに高速なマッチングアルゴリズムであり、パターンがある程度以上（普通５以上くらい）の長さを持つ場合には、最も速い文字列マッチングアルゴリズムだと言われている。KMP法と同様に最悪計算量は$$O(n)$$である。

シンプルなマッチング方法やKMP法ではテキストの中の文字をそれぞれ１回は調べなければならず、最低でも$$n$$回の比較を行う必要があるのに対し、BM法は$$n/m$$個の文字とだけ比較すればよいことがあり、これによって特にパターンの長さが長い時に計算時間の大幅な短縮が期待できる。この特徴から、（$$O(n^2)$$を$$O(n \log n)$$にするような劇的な高速化とは違って計算時間を数分の１にするだけだが）現実世界ではテキスト探索の使用頻度が高いということもあり、BM法は実用上極めて重要なアルゴリズムとみなされている。

BM法では、シンプルなマッチング方法と同様にテキストを左から順に調べていくが、一旦テキスト上の位置が決まったら、パターンについては逆に右から左に向かって調べる。もちろんマッチングを進めるにあたってはKMP法と同様にそれ以前に行った比較の結果として得られた情報をできるだけ活用して比較の回数を減らすように努める。

BM法では、比較回数を減らすために、不一致が見つかった時に一致しないことがわかっている場所を飛ばして、できるだけ大きくパターンをずらすことを目標とする。まず、不一致となった点のテキストの文字によってパターンをずらす量を決める。これは最初の文字で不一致となった時に最も有効になる。次に、何文字かが一致した後で不一致になった時に一致した部分の情報を利用してずらす量を決める。これは逆に一致した文字数が多いほど効果が大きい。普通はこの２つを組み合わせて使う。

### 正規表現マッチング

KMP法やBM法による文字列マッチングの一種の拡張であり、KMP法やBM法はパターンとして単なる文字列を与えた時のマッチングアルゴリズムであるのに対し、正規表現マッチングはパターンとしてより一般的な形のものを許した場合のマッチングアルゴリズムである。一般にいくつかの状態とそれらの間の移動の規則によって計算の方法を規定できる場合、オートマトンの考え方が有効である。

正規表現マッチングは、典型的な例としてエディタで文字列を探したり置き換えたりするコマンドでパターンとして正規表現を用いたり、コンパイラでプログラムを読み込んで字句単位に切り分ける処理を正規表現マッチングとして定式化できる。コンパイラを作成する労力を軽減するために、字句の認識を行うプログラムを自動生成するツール（Unixのlexなど）があるが、これらは正規表現マッチングを行うアルゴリズムを内部で使っている。

正規表現はさまざまなパターンを表すことができるが、その表現能力には一定の限界があり、例えば左右の括弧の対応が正しく取れているという条件を表現することはできない。コンパイラの例で言えば、正規表現で記述できるのは字句を取り出す段階までで、その先のプログラムの構文を表現することはできない。

#### オートマトン

オートマトンは決められた規則に従って幾つかの状態を遷移しながら計算を進める機構である。次の状態を決める規則の書き方やメモリの使い方にどのようなものが許されるかによって、計算の記述能力に差が生じる。これに応じてオートマトンはいくつかの種類に分けられており、有限オートマトンはその中でも最も単純なものである。有限オートマトンは有限状態機械（FSM：Finite State Machine）とも呼ばれ、有限個の状態とそれらの間の遷移を持つ。正規表現マッチングでは有限オートマトンの考え方を利用する。どの状態からでも次の遷移先候補が１つに決まるようなオートマトンを決定性有限オートマトン（DFA：Deterministic Finite Automaton）のと呼ぶ。一方で次の遷移先候補が複数ある場合は非決定性有限オートマトン（NFA：Nondeterministic Finite Automaton）と呼ぶ。

一般に有限オートマトンは有向グラフで表現でき、グラフの頂点が有限オートマトンの状態に対応し、辺が遷移を表す。このようなグラフを状態遷移図と呼ぶ。有限オートマトンは動作中、いずれかの状態に位置し、動作開始時の状態を初期状態と呼ぶ。その他に最終状態と呼ばれる状態が１つ以上存在し、各状態で有限オートマトンが入力装置から１文字読み込んで状態遷移することを繰り返し、入力文字列が尽きた時点で有限オートマトンは停止するが、そのときに最終状態に到達していれば、有限オートマトンはその入力文字列を受理する。それ以外の場合は拒否する。一般に正規表現が与えられたとき、その正規表現に合致する文字列だけを受理する有限オートマトンを構成することが可能である。

正規表現マッチングはバックトラック法を使えば比較的容易に実現できるが後述するReDoSのリスクがあるため、バックトラック法を使わずに有限オートマトンの動作を追跡（シミュレート）することではるかに効率の良いマッチングが可能になる。シミュレートする場合、NFAを使う場合は、状態の数を$$n$$、遷移先候補の数を$$m$$としたとき、マッチングの最悪計算量は$$O(nm)$$になるが、DFA場合は、遷移先候補は常に１つなので、線形時間（$$O(n)$$）でマッチングが完了する。実際はNFAを使う場合でも$$O(nm)$$というのは多めの評価であり、大抵のパターンのマッチングは$$O(n)$$程度の計算量で処理できるが、集合を管理するのに手間がかかるので、DFAに比べて遅くなるのは避け難い。

与えられた正規表現をNFAに変換することは容易であるが、DFAを直接作るのは難しい。そのためDFAを用いる場合は正規表現からNFAに翻訳した上で、NFAをDFAに変換することが一般的である。NFAとDFAは等価（DFAで受理できる集合は全て何らかのNFAで受理できるし、NFAで受理できる集合は全て何らかのDFAで受理できる）なので、NFAをDFAに変換することは可能である。DFAに変換することで、より効率的に正規表現マッチングを行うことができるが、変換処理には最悪の場合、正規表現の長さに対して指数時間（正規表現の長さを$$n$$とすると、$$O(2^n)$$）の計算量がかかることがある。しかし、実際にはこのような最悪の場合が発生することは稀である。

このことから、大量の入力とのマッチングを行う場合にはDFAを使うべきだが、小規模の処理の場合は前処理に要する時間や実装の容易さを考えるとNFAを使うことも選択肢に入る。

[計算の理論I NFAとDFAの等価性](https://www.cs.is.saga-u.ac.jp/lecture/automaton/03F/Lect04.pdf)

#### ReDoS

ReDoS（Regular expression Denial of Service）とはDoSの一種である。ある文字の遷移先が$$n$$個存在する場合、その文字が$$m$$個入力されると、遷移先候補は$$n^m$$個になる。正規表現とそれに対応する入力によっては、遷移先候補が数万個になる場合もあり、このような場合は遷移先候補を１つずつ辿って受理か否かを確認するのに膨大な時間がかかってしまう。この事象のことを破壊的なバックトラッキング（catastrophic backtracking）と呼び、プログラムのサービス停止状態（DoS）を引き起こす。

例えば、`a*`を`a`の0回以上の繰り返しを表す正規表現だとするとき、`aaaaa`という文字列に対して`(a*)*b`という正規表現を適用する場合を考える。`a`という文字が来たとき、`a*`によって`a`自身を繰り返すか、`(a*)*`によって`a*`（`a`の繰り返し）を繰り返すかの２つの遷移先候補があるため、$$n$$個の`a`が来たときには全部で$$2^n$$個の遷移先候補が生まれることになる。

2019年には、Webアプリケーションファイアウォールに記述されていた正規表現で定義された検知ルールにReDoSの脆弱性があり、それを突かれたことにより、Cloudflare社が提供するCDNサービスが約30分間ダウンし、その間は世界中のWebサイトが閲覧できなくなるという世界規模の障害につながったことがある。

ReDoS以外にも[正規表現インジェクション](https://blog.ohgaki.net/regular-expression-injection)という攻撃もあり、そもそも正規表現エンジンは脆弱性になりやすいので、実際に開発する際はユーザーに正規表現パターンを直接入力させないようにするべきである。プログラミング言語によっては標準で、正規表現を用いた文字列マッチングに対し、タイムアウト機能を提供しているものもある。

[正規表現をフリーズさせないために](https://qiita.com/Tatamo/items/68a10c6274953e695354)
[Pythonの脆弱性～ReDOS～](https://gihyo.jp/article/2022/11/prevent-vulnerability-0002)<br>
[破壊的なバックトラック(Catastrophic backtracking)](https://ja.javascript.info/regexp-catastrophic-backtracking)<br>
[はじめての正規表現とベストプラクティス10: 危険な「Catastrophic Backtracking」前編](https://techracho.bpsinc.jp/hachi8833/2021_06_11/108100)

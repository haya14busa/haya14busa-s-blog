---
title: 数学ガールフェルマーの最終定理読んだメモ
author: haya14busa
layout: post
permalink: /mathematical-girls-fermas-last-theorem-memo/
categories:
  - Memo
tags:
  - math-girl
  - mathematics
---
[Web連載『数学ガールの秘密ノート』][1]

## プロローグ

> 「整数は神の作ったものだが、他は人間の作ったものである」(Die ganzen Zahlen hat der liebe Gott gemacht, alles andere ist Menschenwerk.)
> 
> &#8211; <cite><a href="http://ja.wikipedia.org/wiki/レオポルト・クロネッカー">レオポルト・クロネッカー &#8211; Wikipedia</a></cite>

*   [Leopold Kronecker &#8211; Wikipedia, the free encyclopedia][2]
*   [Diophantus &#8211; Wikipedia, the free encyclopedia][3] 
    *   [アレクサンドリアのディオファントス &#8211; Wikipedia][4]
*   [Pythagoras &#8211; Wikipedia, the free encyclopedia][5] 
    *   [ピタゴラス &#8211; Wikipedia][6]
*   [Pierre de Fermat &#8211; Wikipedia, the free encyclopedia][7] 
    *   [ピエール・ド・フェルマー &#8211; Wikipedia][8]

## 無限の宇宙を手に乗せて

### 時計の巡回

*   [Number theory &#8211; Wikipedia, the free encyclopedia][9] 
    *   [数論 &#8211; Wikipedia][10]
*   [数論初歩][11] 
    *   PDFでまとまっている
*   <<特殊から一般へ>>

> 「ではみなさんは、そういうふうに川だと云われたり、乳の流れたあとだと云われたりしていたこのぼんやりと白いものがほんとうは何かご承知ですか。」
> 
> &#8211; <cite><a href="http://www.aozora.gr.jp/cards/000081/files/456_15050.html">宮沢賢治 銀河鉄道の夜</a></cite>

## ピタゴラスの定理

![pytagorean_proof_by_gif][12]

*   [Pythagorean theorem &#8211; Wikipedia, the free encyclopedia][13] 
    *   [ピタゴラスの定理 &#8211; Wikipedia][14]
*   Card Tetra: 原始ピタゴラス数は無数に存在するか
*   Card Milka: 原点中心の単位円周上に、有理点は無数に存在するか 
    *   e.g. (1,0),(0,1),(-1,0),(0,01)
*   <<例示は理解の試金石>>
*   <<整数の構造は、素因数が示す>>
*   <<有理数の構造は、整数の比が示す>>

### Card: Tetra

1.  <<例示は理解の試金石>> 
    *   具体的に探してみる
2.  偶奇を調べる(パリティチェック) 
    1.  a,bが偶数の原始ピタゴラス数は存在するか
    2.  a,bが奇数は？
    *   c^2は4の倍数
    *   背理法
3.  <<和の形>>から<<積の形>>へ 
    1.   <<整数の構造は、素因数が示す>>
    2.  b^2 = (c-a)(c+a)とa,b,cの偶奇を組み合わせる
4.  互いに素 
    1.  <<わかりきっていることでも、きちんとまとめるのがいいんだよ>>
    2.  a,c,b、A,B,Cについて互いに素であるか？
    *   aを奇数として
    *   2A = c &#8211; a
    *   2B = b
    *   2C = c + a
    *   Pattern: 互いに素の証明は、背理法で同じ倍数になるという矛盾を導く
5.  素因数分解 
    *   素因数分解の一意性 -> A,Cは平方数
6.  原子ピタゴラス数の一般形 
    *   無数にある素数から無数の原子ピタゴラス数を作り上げる

*   <<偶奇を調べる>>
*   <<整数の構造は、素因数が示す>>
*   <<積の形>>
*   <<互いに素>>

### Card: Milka

1.  <<有理点が無数にある>>という命題は、方程式`x^2 + y^2 = 1`が<<無数の有理数解を持つ>>という命題と同値
2.  P(-1,0)を通り、傾きがtの直線`l: y = tx + t`と単位円`x^2+y^2=1`のもう一つの交点Qをtで表す
3.  **点Tがy軸上の有理点ならば、点Qも有理点になる**

#### 原始ピタゴラス数の問題との関連

ピタゴラスの定理を変形

    a^2 + b^2 = c^2
    (a/c)^2 + (b/c)^2 = 1 -> 単位円
    

<<有理数の構造は、整数の比が示す>>

**<<原始ピタゴラス数が無数にある>>と<<単位円周上に有理点が無数にある>>は同値**

### Link

*   [原始ピタゴラス数について少し勉強してみるなど &#8211; 自明でない日記][15]   &#8211; 流れが書いてある
*   [学習ノート・数学ガール・フェルマーの最終定理][16]   &#8211; 補足もある<<単位円周上に有理点が無数にある>>ならば<<原始ピタゴラス数が無数にある>>

## 互いに素

*   [Coprime integers &#8211; Wikipedia, the free encyclopedia][17] 
    *   relatively prime
*   [互いに素 &#8211; Wikipedia][18]
*   [「互いに素」という概念][19]
*   [互いに素][20][PDF]

自然数a,bの最大公約数をM、最小公倍数をLで表すと、このとき

    a \* b = M \* L
    

が成り立つ

*   a &#42; b : <<aの素因数すべて>>と<<\bの素因数すべて>>
*   M &#42; L :   &#8211; M : <<aとbでだぶっている素因数すべて>>   &#8211; L : <<aとbでだぶりを除いた素因数すべて>>

### 素数指数表現

    n = 2^n_2 \* 3^n_3 \* 5^n_5 ....
      = <n_2, n_3, n_5, ....>
    

*   無限次元のベクトルとみなして考えることができる 
    *   互いに素のとき、ベクトルが直交する
*   mとnが互いに素のとき m⊥nと表す

## 背理法

*   問題を読む<<深さ>>
*   [Propositional calculus &#8211; Wikipedia, the free encyclopedia][21] 
    *   [命題 &#8211; Wikipedia][22]
*   [Theorem &#8211; Wikipedia, the free encyclopedia][23]

矛盾
:   Pを命題とする。 矛盾とは、<<\Pである>>と<<\Pでない>>の両方が成り立つこと

√2が有理数でないことを、背理法以外に素因数の個数のパリティで証明できる

<<素因数分解の一意性により、素因数の個数は、素因数ごとに両辺で一致する>>

## 砕ける素数

*   [Imaginary number &#8211; Wikipedia, the free encyclopedia][24] 
    *   [虚数 &#8211; Wikipedia][25]
*   虚数iを不自然に感じるのは負数を不自然に感じるのと変わらない 
    *   `x - 1 = 0`をx ≥0の世界で考える
*   **方程式の解として<<定義>>した。満たすべき<<公理>>を方程式の形で示した**

### 1次元から2次元への飛躍

*   [Complex plane &#8211; Wikipedia, the free encyclopedia][26] 
    *   [複素平面 &#8211; Wikipedia][27]
*   [Polar coordinate system &#8211; Wikipedia, the free encyclopedia][28] 
    *   [極座標系 &#8211; Wikipedia][29]

<<二乗して-1になる数>>を幾何の目で見るなら、  
**二回行うと-1になる拡大・回転は何か?**

`(-1) * (-1) = 1`は-1の偏角180度を二倍することに相当する -> 360度の回転

回れ右を二回行えば元の向きに戻る

代数 <-> 幾何 複素数全体の集合 <-> 複素平面 複素数`a+bi` <-> 複素平面上の点(a,b) 複素数の集合 <-> 複素平面上の図形 複素数の数 <-> 平行四辺形の対角線 複素数の積 <-> 絶対値の積、偏角の和(拡大・回転)

### 格子点

*   [Pigeonhole principle &#8211; Wikipedia, the free encyclopedia][30] 
    *   [鳩の巣原理 &#8211; Wikipedia][31]

**鳩の巣原理**
:   n個の鳩ノ巣にn+1羽の鳩が入ったら、  
    少なくとも1個の巣には、2羽の鳩がいる。  
    ただし、nは自然数とする。

### 砕ける素数

*   [単数 &#8211; Wikipedia][32]
*   [Gaussian integer &#8211; Wikipedia, the free encyclopedia][33] 
    *   [ガウス整数 &#8211; Wikipedia][34]
*   a + biを整数の一種と考えると、素因数分解の一意性が砕ける

    整数Ζ----ゼロ        (0)
           |--単数        (±1)
           |--合成数      (±4,±6,±8,...)
           `--素数----<<砕ける素数>>  Ζ[i]で積に分解可能
                   `-<<砕けない素数>> Ζ[i]で積に分解不可能
    

砕ける素数を4で割った余りは3にならない。

    p = (a + bi)(a - bi)
    

pを奇数の素数にすると、

    p = (a + bi)(a - bi) <-> pを4で割ると余りは1
    

## アーベル郡の涙

*   [数学記号の表 &#8211; Wikipedia][35]

> <td rowspan="2">
>   <a href="/wiki/%E7%B4%A0%E6%95%B0" title="素数" class="broken_link">素数</a> (Prime number)の全体、<a href="/wiki/%E5%B0%84%E5%BD%B1%E7%A9%BA%E9%96%93" title="射影空間" class="broken_link">射影空間</a>など
> </td>
> 
> <td rowspan="2">
>   <a href="/wiki/%E8%87%AA%E7%84%B6%E6%95%B0" title="自然数" class="broken_link">自然数</a> (Natural number)の全体
> </td>
> 
> <td rowspan="2">
>   <a href="/wiki/%E6%95%B4%E6%95%B0" title="整数" class="broken_link">整数</a> (独: Zahlen)の全体
> </td>
> 
> <td rowspan="2">
>   <a href="/wiki/%E6%9C%89%E7%90%86%E6%95%B0" title="有理数" class="broken_link">有理数</a> (Quotient)の全体
> </td>
> 
> <td rowspan="2">
>   <a href="/wiki/%E5%AE%9F%E6%95%B0" title="実数" class="broken_link">実数</a> (Real number)の全体
> </td>
> 
> <td rowspan="2">
>   <a href="/wiki/%E8%A4%87%E7%B4%A0%E6%95%B0" title="複素数" class="broken_link">複素数</a> (Complex number)の全体
> </td>
> 
> <td rowspan="2">
>   <a href="/wiki/%E5%9B%9B%E5%85%83%E6%95%B0" title="四元数" class="broken_link">四元数</a> (Hamilton number)の全体
> </td>
> 
> &#8211; <cite><a href="http://ja.wikipedia.org/wiki/%E6%95%B0%E5%AD%A6%E8%A8%98%E5%8F%B7#.E4.BB.A3.E6.95.B0.E5.AD.A6.E3.81.AE.E8.A8.98.E5.8F.B7">数学記号の表 &#8211; Wikipedia</a></cite>

*   [Set (mathematics) &#8211; Wikipedia, the free encyclopedia][41] 
    *   [集合 &#8211; Wikipedia][42]
*   [Group (mathematics) &#8211; Wikipedia, the free encyclopedia][43] 
    *   [群 (数学) &#8211; Wikipedia][44]
    *   [Binary operation &#8211; Wikipedia, the free encyclopedia][45]
    *   [Element (mathematics) &#8211; Wikipedia, the free encyclopedia][46]

### 集合に対して演算を入れる

集合Gに対して、演算☆が定義されるとは、集合Gのどんな要素a,bに対しても、

    a ☆ b ∈ G
    

が成り立つということ。これを<<演算☆に関して集合Gは**閉じている**>>と呼ぶ

*   a ∈ G 
    *   aはGの要素である
    *   a is an element of G
    *   a belongs to G
    *   a is in G
*   G -> Group

結合法則

    (a ☆ b) ☆ c = a ☆ (b ☆ c)
    

単位元の定義(単位元eの公理)
:   集合Gの任意の要素aに対して、以下の式を満たす集合Gの要素eを、演算☆における**単位元**と呼ぶ。

    a ☆ e = e ☆ a = a
    

<<公理が定義を生み出している>>
:   最も厳密な言葉である数式を使い、公理という名の命題を使って定義する

逆元の定義(逆元の公理)
:   aを集合Gの要素とし、eを単位元とする。aに対して、以下の式を満たすb∈Gを、演算☆に関するaの**逆元**と呼ぶ。 ~~~ a ☆ b = b ☆ a = e ~~~

### 郡の定義(郡の公理)

以下の公理を満たす集合Gを**郡**と呼ぶ。

*   **演算☆**に関して閉じている
*   任意の元に対して、**結合法則**が成り立つ。
*   **単位元**が存在する。
*   任意の元に対して、その元に対する**逆元**が存在する。

*かくのごとき集合を**郡**と呼べ*

### <<例示は理解の試金石>>

*   <<整数全体の集合Ζは、演算+に関して郡をなすか?>>
*   <<奇数全体の集合Ζは、演算+に関して郡をなすか?>>
*   <<偶数全体の集合Ζは、演算+に関して郡をなすか?>>
*   <<整数全体の集合Ζは、演算×に関して郡をなすか?>>

例

*   最小の郡 
    *   {e}
*   要素が2個の群 
    *   {e,a}

演算表

    |☆ | e a|
    |---+----|
    |e  | e a|
    |a  | a e|
    

*   <<同じ>>郡のことを<<同型>>な郡と呼ぶ
*   <<要素が二つの郡は本質的に一つ>> 
    *   公理によって与えられる*制約が構造を生み出している*

### アーベル群

*   [Abelian group &#8211; Wikipedia, the free encyclopedia][47] 
    *   [アーベル群 &#8211; Wikipedia][48]
*   任意の元について**交換法則**を満たす郡を**アーベル郡**という

交換法則

    a ☆ b = b ☆ a
    

<<無矛盾性は存在の礎>>

## ヘアスタイルを法として

*   [Division (mathematics) &#8211; Wikipedia, the free encyclopedia][49] 
    *   [除法 &#8211; Wikipedia][50]
*   [Modular arithmetic &#8211; Wikipedia, the free encyclopedia][51] 
    *   [合同式 &#8211; Wikipedia][52]
*   法 -> modulus
*   商 -> quotient
*   剰余 -> residue

modの定義(整数)
:   a,b,q,rは整数で、b≠0とする

    a mod b = r ⇔ a = bq + r (0 ≤r < |b|)
    

合同(congruent modulo)

    a ≡ b (mod m) ⇔ a mod m = b mod m ⇔ (a-b) mod m = 0
    

### 郡・環・体

*   [剰余類環 &#8211; Wikipedia][53]
*   [Ring (mathematics) &#8211; Wikipedia, the free encyclopedia][54] 
    *   [環 (数学) &#8211; Wikipedia][55]
    *   郡では演算が1つだが、環では2種類の演算を入れる
*   [Commutative ring &#8211; Wikipedia, the free encyclopedia][56] 
    *   [可換環 &#8211; Wikipedia][57]

環の定義(環の公理)
:   以下の公理を満たす集合を**環**とよぶ

*   演算+(加法)に関して 
    *   閉じている
    *   単位元が存在する(0と呼ぶ)
    *   すべての要素について結合法則が成り立つ
    *   すべての要素について交換法則が成り立つ
    *   すべての要素について逆元が存在する
*   演算×(乗法)に関して 
    *   閉じている
    *   単位元が存在する(1と呼ぶ)
    *   すべての要素について結合法則が成り立つ
    *   すべての要素について交換法則が成り立つ
*   演算+と×に関して 
    *   すべての要素について分配法則が成り立つ

厳密に言えば乗法の単位元が存在する可換環を述べている

*   環と郡 
    *   環は、加法に関してアーベル群である。
    *   環は、乗法に関してアーベル郡とは限らない。

整数環と剰余環

体の定義(体の公理)
:   環の定義プラス、演算×に関して**0以外のすべての要素について逆元が存在する**

*   [Field (mathematics) &#8211; Wikipedia, the free encyclopedia][58] 
    *   [体 (数学) &#8211; Wikipedia][59]

**剰余環Ζ/mΖは、mが素数のとき体になる**

pを素数とし、剰余環Ζ/mΖを体と見なすとき、それを**有限体**F_pと呼ぶ

    F_p = Ζ/pΖ
    

合同を使って<<無限を折りたたむ>>

## 無限降下法

*   [Fermat&#8217;s Last Theorem &#8211; Wikipedia, the free encyclopedia][60] 
    *   [フェルマーの最終定理 &#8211; Wikipedia][61]
*   [Andrew Wiles &#8211; Wikipedia, the free encyclopedia][62] 
    *   [アンドリュー・ワイルズ &#8211; Wikipedia][63]

次の方程式は、n ≥3で自然数解を持たない。

    x^n + y^n = z^n
    

私は驚くべき証明を見つけたが、  
それを書き記すには、この余白は狭すぎる。

FLT -> **F**ermat&#8217;s **L**ast **T**heorem

### Card: Tetra

三辺が自然数で面積が平方数である直角三角形は存在するか。

*   [Proof by infinite descent &#8211; Wikipedia, the free encyclopedia][64]  
    *   [無限降下法 &#8211; Wikipedia][65]

無限降下法
:   1.  自然数に関する数式を作る。
    2.  その数式を操作して、*同じ形をした別の数式を作る* 
        *   このとき、小さくなる自然数を含む
    3.  同じ操作を繰り返すと自然数が無限に小さくなる -> 矛盾

### Card: Milka

FLT(4)の証明

Card: Tetraの解答を使って矛盾を示す

## 最も美しい数式

*   [Euler&#8217;s identity &#8211; Wikipedia, the free encyclopedia][66] 
    *   [オイラーの等式 &#8211; Wikipedia][67]

Euler&#8217;s identity

    e^iπ = -1
    

*   [Euler&#8217;s formula &#8211; Wikipedia, the free encyclopedia][68] 
    *   [オイラーの公式 &#8211; Wikipedia][69]

Euler&#8217;s formula

    e^iθ = cosθ + isinθ
    

*   [Closure (mathematics) &#8211; Wikipedia, the free encyclopedia][70] 
    *   [生成 (数学) &#8211; Wikipedia][71]

## 最も美しい数式

&#8220;Is the term &#8216;well-defines&#8217; well defined?&#8221;

1.  指数を指数法則を満たすものと定義 
    *   自然数以外への拡張
2.  e^xを冪級数で表す
3.  テイラー展開
4.  テイラー展開の結果をe^xと定義 
    *   複素数への拡張
5.  e^iθを<<大胆な代入>>
6.  cosθとsinθのテイラー展開の結果と比較する

## フェルマーの最終定理

*   [Ideal (ring theory) &#8211; Wikipedia, the free encyclopedia][72] 
    *   [イデアル &#8211; Wikipedia][73]
*   [Ernst Kummer &#8211; Wikipedia, the free encyclopedia][74] 
    *   [エルンスト・クンマー &#8211; Wikipedia][75]
*   [Modularity theorem &#8211; Wikipedia, the free encyclopedia][76] 
    *   [谷山・志村の定理 &#8211; Wikipedia][77]
    *   [Goro Shimura &#8211; Wikipedia, the free encyclopedia][78]
    *   [志村五郎 &#8211; Wikipedia][79]
    *   [Yutaka Taniyama &#8211; Wikipedia, the free encyclopedia][80]
    *   [谷山豊 &#8211; Wikipedia][81]
*   [Gerhard Frey &#8211; Wikipedia, the free encyclopedia][82] 
    *   [ゲルハルト・フライ &#8211; Wikipedia][83]
*   [Wiles&#8217; proof of Fermat&#8217;s Last Theorem &#8211; Wikipedia, the free encyclopedia][84]
*   [The Way to the Proof of Fermat’s Last Theorem][85]
*   [Modular elliptic curves and Fermat&#8217;s last theorem &#8211; Stanford University][86]

 [1]: http://www.hyuki.com/girl/note.html
 [2]: http://en.wikipedia.org/wiki/Leopold_Kronecker
 [3]: http://en.wikipedia.org/wiki/Diophantus
 [4]: http://ja.wikipedia.org/wiki/アレクサンドリアのディオファントス
 [5]: http://en.wikipedia.org/wiki/Pythagoras
 [6]: http://ja.wikipedia.org/wiki/ピタゴラス
 [7]: http://en.wikipedia.org/wiki/Pierre_de_Fermat
 [8]: http://ja.wikipedia.org/wiki/ピエール・ド・フェルマー
 [9]: http://en.wikipedia.org/wiki/Number_theory
 [10]: http://ja.wikipedia.org/wiki/数論
 [11]: http://aozoragakuen.sakura.ne.jp/suuron/suuron.html
 [12]: http://livedoor.4.blogimg.jp/ayacnews/imgs/9/2/92af2ab0.gif
 [13]: http://en.wikipedia.org/wiki/Pythagorean_theorem
 [14]: http://ja.wikipedia.org/wiki/ピタゴラスの定理
 [15]: http://d.hatena.ne.jp/tt4cs/20111230/1325218297
 [16]: http://www.bonten-yutori.net/mathgirls_F/
 [17]: http://en.wikipedia.org/wiki/Coprime_integers
 [18]: http://ja.wikipedia.org/wiki/%E4%BA%92%E3%81%84%E3%81%AB%E7%B4%A0
 [19]: http://www.hyuki.com/dig/relprime.html
 [20]: http://sigma2011auemath.web.fc2.com/pdf/03kyo1.pdf
 [21]: http://en.wikipedia.org/wiki/Propositional_calculus
 [22]: http://ja.wikipedia.org/wiki/%E5%91%BD%E9%A1%8C
 [23]: http://en.wikipedia.org/wiki/Theorem#Terminology
 [24]: http://en.wikipedia.org/wiki/Imaginary_number
 [25]: http://ja.wikipedia.org/wiki/%E8%99%9A%E6%95%B0
 [26]: http://en.wikipedia.org/wiki/Complex_plane
 [27]: http://ja.wikipedia.org/wiki/%E8%A4%87%E7%B4%A0%E5%B9%B3%E9%9D%A2
 [28]: http://en.wikipedia.org/wiki/Polar_coordinate_system
 [29]: http://ja.wikipedia.org/wiki/%E6%A5%B5%E5%BA%A7%E6%A8%99
 [30]: http://en.wikipedia.org/wiki/Pigeonhole_principle
 [31]: http://ja.wikipedia.org/wiki/%E9%B3%A9%E3%81%AE%E5%B7%A3%E5%8E%9F%E7%90%86
 [32]: http://ja.wikipedia.org/wiki/%E5%8D%98%E6%95%B0
 [33]: http://en.wikipedia.org/wiki/Gaussian_integer
 [34]: http://ja.wikipedia.org/wiki/%E3%82%AC%E3%82%A6%E3%82%B9%E6%95%B4%E6%95%B0
 [35]: http://ja.wikipedia.org/wiki/数学記号の表
 [36]: /wiki/P "P"
 [37]: /wiki/Z "Z"
 [38]: /wiki/R "R"
 [39]: /wiki/C "C"
 [40]: /wiki/H "H"
 [41]: http://en.wikipedia.org/wiki/Set_(mathematics)
 [42]: http://ja.wikipedia.org/wiki/%E9%9B%86%E5%90%88
 [43]: http://en.wikipedia.org/wiki/Group_(mathematics)
 [44]: http://ja.wikipedia.org/wiki/%E7%BE%A4_(%E6%95%B0%E5%AD%A6)
 [45]: http://en.wikipedia.org/wiki/Binary_operation
 [46]: http://en.wikipedia.org/wiki/Element_(mathematics)
 [47]: http://en.wikipedia.org/wiki/Abelian_group
 [48]: http://ja.wikipedia.org/wiki/%E3%82%A2%E3%83%BC%E3%83%99%E3%83%AB%E7%BE%A4
 [49]: http://en.wikipedia.org/wiki/Division_(mathematics)
 [50]: http://ja.wikipedia.org/wiki/%E5%89%B0%E4%BD%99
 [51]: http://en.wikipedia.org/wiki/Modular_arithmetic
 [52]: http://ja.wikipedia.org/wiki/%E5%90%88%E5%90%8C%E5%BC%8F
 [53]: http://ja.wikipedia.org/wiki/%E5%89%B0%E4%BD%99%E9%A1%9E%E7%92%B0
 [54]: http://en.wikipedia.org/wiki/Ring_(mathematics)
 [55]: http://ja.wikipedia.org/wiki/%E7%92%B0_(%E6%95%B0%E5%AD%A6)
 [56]: http://en.wikipedia.org/wiki/Commutative_ring
 [57]: http://ja.wikipedia.org/wiki/%E5%8F%AF%E6%8F%9B%E7%92%B0
 [58]: http://en.wikipedia.org/wiki/Field_(mathematics)
 [59]: http://ja.wikipedia.org/wiki/%E4%BD%93_(%E6%95%B0%E5%AD%A6)
 [60]: http://en.wikipedia.org/wiki/Fermat%27s_Last_Theorem
 [61]: http://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A7%E3%83%AB%E3%83%9E%E3%83%BC%E3%81%AE%E6%9C%80%E7%B5%82%E5%AE%9A%E7%90%86
 [62]: http://en.wikipedia.org/wiki/Andrew_Wiles
 [63]: http://ja.wikipedia.org/wiki/%E3%82%A2%E3%83%B3%E3%83%89%E3%83%AA%E3%83%A5%E3%83%BC%E3%83%BB%E3%83%AF%E3%82%A4%E3%83%AB%E3%82%BA
 [64]: http://en.wikipedia.org/wiki/Proof_by_infinite_descent
 [65]: http://ja.wikipedia.org/wiki/%E7%84%A1%E9%99%90%E9%99%8D%E4%B8%8B%E6%B3%95
 [66]: http://en.wikipedia.org/wiki/Euler%27s_identity
 [67]: http://ja.wikipedia.org/wiki/%E3%82%AA%E3%82%A4%E3%83%A9%E3%83%BC%E3%81%AE%E7%AD%89%E5%BC%8F
 [68]: http://en.wikipedia.org/wiki/Euler%27s_formula
 [69]: http://ja.wikipedia.org/wiki/%E3%82%AA%E3%82%A4%E3%83%A9%E3%83%BC%E3%81%AE%E5%85%AC%E5%BC%8F
 [70]: http://en.wikipedia.org/wiki/Closure_(mathematics)
 [71]: http://ja.wikipedia.org/wiki/生成_(数学)
 [72]: http://en.wikipedia.org/wiki/Ideal_(ring_theory)
 [73]: http://ja.wikipedia.org/wiki/イデアル
 [74]: http://en.wikipedia.org/wiki/Ernst_Kummer
 [75]: http://ja.wikipedia.org/wiki/エルンスト・クンマー
 [76]: http://en.wikipedia.org/wiki/Modularity_theorem
 [77]: http://ja.wikipedia.org/wiki/谷山・志村の定理
 [78]: http://en.wikipedia.org/wiki/Goro_Shimura
 [79]: http://ja.wikipedia.org/wiki/志村五郎
 [80]: http://en.wikipedia.org/wiki/Yutaka_Taniyama
 [81]: http://ja.wikipedia.org/wiki/谷山豊
 [82]: http://en.wikipedia.org/wiki/Gerhard_Frey
 [83]: http://ja.wikipedia.org/wiki/ゲルハルト・フライ
 [84]: http://en.wikipedia.org/wiki/Wiles'_proof_of_Fermat's_Last_Theorem
 [85]: http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.27.6567&rep=rep1&type=pdf
 [86]: http://math.stanford.edu/~lekheng/flt/wiles.pdf
# 物理的描像の3分類
ここでは**オイラー形式**と**ラグランジュ形式**、並びに**セミラグランジュ形式**という3つの物理的描像について学びます。前から2つの名前は連続体力学でも聞いたことがあるでしょう。一方のセミラグランジュ形式は数値解析の文脈で発展した形式です。執筆にあたっては[計算流体力学](https://www.coronasha.co.jp/np/isbn/9784339045970/)を参考にしました。

## 支配方程式の一般的記法
ある物理量$\phi$と流れ場$\boldsymbol{v}$を考えます。$\phi$の保存則を考えると以下の支配方程式が得られます。
```{math}
\partial_t\phi+\boldsymbol{v}\cdot \nabla \phi + \phi\nabla \cdot \boldsymbol{v} =\chi
```
ここで$\chi$は移流項以外を纏めたものになります。更に左辺第3項も$\chi$に含めてしまい、
```{math}
\partial_t\phi+\boldsymbol{v}\cdot \nabla \phi =\chi
```
として考えることにしましょう(2つの式で$\chi$の定義が変わっていることに注意してください)。そして空間と時間について$(\boldsymbol{x_1}, t_1)$と$(\boldsymbol{x_2}, t_2)$の2点を考えます。ここで$t_1 < t_2$です。Stokesの定理を用いることで以下の式が得られます。
```{math}
:label: eq-cont
\phi(\boldsymbol{x_2}, t_2)=\phi(\boldsymbol{x_1}, t_1)+\int_C(d\boldsymbol{x}-\boldsymbol{v}dt)\cdot \nabla \phi+\int_C{\chi}dt
```
ここで$C$は時空間の2点を結ぶ任意の経路です。さてSim.の目的は物理量の時間発展です。したがって任意の$\boldsymbol{x_1}$における$\phi(\boldsymbol{x_1}, t_1)$の値が与えられているとし、そこから任意の$\boldsymbol{x_2}$における$\phi(\boldsymbol{x_2}, t_2)$を求めようとします。その数式は上式の右辺であり、具体的な計算方法によって形式が異なるという訳です。

## オイラー形式
オイラー形式では$\boldsymbol{x_1}=\boldsymbol{x_2}$とします。つまり$\phi(\boldsymbol{x_1}, t_1)$から同じ位置の$\phi(\boldsymbol{x_1}, t_2)$を計算します("から"と言ってしまうとそれ以外の位置における物理量に依存しない印象を受けるかもしれませんが、そういう訳ではございません。式中に勾配などが登場するのでやはり近傍の値にも依存します)。このとき式{eq}`eq-cont`は以下のように書き直されます。
```{math}
\phi(\boldsymbol{x_1}, t_2)=\phi(\boldsymbol{x_1}, t_1)+\int_{t_1}^{t_2}(-\boldsymbol{v} \cdot \nabla \phi)dt+\int_{t_1}^{t_2}{\chi}dt
```
$d\boldsymbol{x}=0$であることに注意してください。また時空間の2点を結ぶ経路$C$も位置を固定された時間発展で考えた方が楽なので、上式のようになった次第です。ここから更に数値計算可能な形へ持っていく方法は、例えば[数値流体力学](https://www.morikita.co.jp/books/mid/091972)で解説されています(本ブログでもいつか議論します)。

## ラグランジュ形式

## セミラグランジュ形式
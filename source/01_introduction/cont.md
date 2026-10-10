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
として考えることにしましょう(2つの式で$\chi$の定義が変わっていることに注意してください)。そして空間と時間について$(\boldsymbol{x_1}, t_1)$と$(\boldsymbol{x_2}, t_2)$の2点を考えます。ここで$t_1 < t_2$です。ストークスの定理を用いることで以下の式が得られます。
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
$d\boldsymbol{x}=0$であることに注意してください。また時空間の2点を結ぶ経路$C$も位置を固定された時間発展で考えた方が楽なので、上式のようになった次第です。位置座標$\boldsymbol{x}$は実数値ベクトルであるため、厳密には更新すべき$\phi(\boldsymbol{x}, t_2)$が無限に存在することになります。ただしそれだと計算機で扱えないので、数値計算では空間を有限の間隔で離散化し、各節点(もしくはセルや要素)の値を更新させます。当然ながら計算精度は離散化の解像度に依存します。以下はオイラー形式の概念図です。節点の無い領域の$\phi$の値は、近傍の節点からの補間により求めたりします(厳密には有限体積法や有限差分法、並びに有限要素法によって異なります。一旦は粗っぽいイメージを持つだけで十分です)。

```{figure} ./pic/euler.png
:name: fig-euler
:width: 40%
:align: center

オイラー形式の概念図
```

## ラグランジュ形式
ラグランジュ法では式{eq}`eq-cont`の時空間経路$C$に対して$d\boldsymbol{x}=\boldsymbol{v}dt$となるようなものを考えます。したがって$\boldsymbol{x_2}=\boldsymbol{x_1}+\int_{t_1}^{t_2}\boldsymbol{v}dt$です。よくラグランジュ法は「粒子のような部分系を動かして全体の力学的挙動を表現する」と言われますが、正に$d\boldsymbol{x}=\boldsymbol{v}dt$の関係を指します。「時刻$t_1$において位置$\boldsymbol{x_1}$にいた仮想粒子を追いかけて、その道中で受ける作用によって状態が$\phi(\boldsymbol{x_1}, t_1)$から$\phi(\boldsymbol{x_2}, t_2)$に変化した」と考える訳です。式{eq}`eq-cont`に$d\boldsymbol{x}=\boldsymbol{v}dt$を代入した結果は以下の通りです。道中で受けた変化という描像がよく伝わると思います。
```{math}
:label: eq-lag
\phi(\boldsymbol{x_2}, t_2)=\phi(\boldsymbol{x_1}, t_1)+\int_C{\chi}dt
```

オイラー形式と同様にラグランジュ形式でも離散化を行います。ただしオイラー形式は「空間を有限個の節点で離散化し、節点の位置座標は時間発展の間固定する」であったのに対し、ラグランジュ形式では「有限個の仮想粒子を用意し、物理法則に従って粒子の位置を更新させる」と考えます。従って粗っぽく言うと、各時刻の粒子は任意の位置に存在しえます。以下はラグランジュ形式の概念図です。

```{figure} ./pic/lagrangian.png
:name: fig-lag
:width: 40%
:align: center

ラグランジュ形式の概念図
```

## セミラグランジュ形式
セミラグランジュ形式はラグランジュ形式とオイラー形式の中間のような存在です。まず、オイラー形式と同様に空間を固定した節点で離散化し、並びに各節点の物理量(たとえば$\phi(\boldsymbol{x_2}, t_2)$)を更新します。一方でラグランジュ形式のように$d\boldsymbol{x}=\boldsymbol{v}dt$を考えます。

重要な特徴は「ラグランジュ形式のように仮想粒子の軌跡を考えるにも拘わらず、物理量を更新する節点は固定されている」という点です。ラグランジュ形式の場合は$\boldsymbol{x_1}$や$\boldsymbol{x_2}$の値を前もって用意する必要は無く、$\boldsymbol{x_2}=\boldsymbol{x_1}+\int_{t_1}^{t_2}\boldsymbol{v}dt$とすることができました。一方でセミラグランジュ形式の場合、節点座標間でこの関係が成立することは期待できません。ある位置座標$\boldsymbol{x}$が$\boldsymbol{x_2}=\boldsymbol{x}+\int_{t_1}^{t_2}\boldsymbol{v}dt$を満たす場合、以下の式が成り立ちます。
```{math}
\phi(\boldsymbol{x_2}, t_2)=\phi(\boldsymbol{x}, t_1)+\int_C{\chi}dt
```
ただしその$\boldsymbol{x}$が節点座標とは限らないのが特徴です。セミラグランジュ形式の概念図を以下に示します。$\phi(\boldsymbol{x}, t_1)$の値は近傍の節点での値から補間して求めます。オイラー形式のときにさりげなく補間について触れましたが、セミラグランジュ形式の数値計算精度において、「如何にして補間するか？」はとても重要になってきます。
```{figure} ./pic/semi.png
:name: fig-semi
:width: 40%
:align: center

セミラグランジュ形式の概念図
```
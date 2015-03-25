!SLIDE

# Scalaでテストライブラリを作ってる話

<br /><br />

<a style="font-size: 10%" rel="license" href="http://creativecommons.org/licenses/by/2.1/jp/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="http://i.creativecommons.org/l/by/2.1/jp/88x31.png" /></a>

!SLIDE

<img src="https://pbs.twimg.com/profile_images/1931553270/xuwei.gif" width="100" height="100" />

- twitter [@xuwei_k](https://twitter.com/xuwei_k)
- github [@xuwei-k](https://github.com/xuwei-k)
- blog <http://d.hatena.ne.jp/xuwei/>


!SLIDE

# どういうテストライブラリ？

<br />
↓
<br />
<br />

## Scalacheckのようなやつ

!SLIDE

- <https://github.com/rickynils/scalacheck>
- <http://scalacheck.org>


!SLIDE

# Scalacheckってなに？

<br />
↓
<br />
<br />

## HaskellのQuickcheckというライブラリの移植
## 自動でテストのための値をランダムに生成


!SLIDE

# Scalacheckじゃダメなの？

<br />
↓
<br />
<br />

## かなり使ってるので、色々細かい不満がでてきた

!SLIDE

# 新しく作るのではなくて、Scalacheckにpull reqすればいいんじゃないの？

!SLIDE


- あとでするかもしれないけど、とりあえず勉強のためにも一から作りたかった
- Scalacheckの内部のコードはそこまで綺麗ではないので、機能追加難しい(と少なくとも今は感じた)


!SLIDE

# Scalacheckの不満点

!SLIDE

(だいたい上から重要な順)

<br />

- CoArbirary
- 巨大な値を生成してテストが終わらないときの対策
- タイムアウト
- パラメータの細かい指定が不可能
- seedを指定して、ランダムだけど予測可能(?)なテスト方法の提供
- GenとArbitraryがあまり意味なく2つあって面倒
- ScalazのTypeclassの階層と重複テストの問題
- Gen Monad Transformer

!SLIDE

# Coarbitrary

- これが一番大きな理由
- 「関数の値をランダムに生成するためのもの」
- そんなことできるの？
- できるらしいですよ
- (うまく説明できる気がしないので、原理は省略)

!SLIDE

## 巨大な値を生成してテストが終わらないときの対策 

<br />

よくある例

<br />


```scala
case class A(value: List[B])
case class B(value: List[Int])
```

<br />


!SLIDE

## 巨大な値を生成してテストが終わらないときの対策 

<br />

- デフォルトではListは100要素まで
- 100 × 100 で 10000 要素


!SLIDE

- 間違って巨大な値生成してテストが終わらなくなったらタイムアウトやうまくキャンセルさせたい
- maxSizeなどを細かく設定したい

↓

Scalacheckでは不可能？


!SLIDE

残念な妥協

<https://github.com/scalaz/scalaz/blob/v7.1.1/project/build.scala#L87>

!SLIDE

## seedを指定して、ランダムだけど予測可能(?)なテスト方法の提供

<br />

本当にランダムだと、再現性が難しいので<br />
(まぁ引数を保持しておくでもいいけど)


!SLIDE

# GenとArbitraryがあまり意味なく2つあって面倒

<https://twitter.com/xuwei_k/status/541898455698776065>

<blockquote class="twitter-tweet" lang="en"><p>ScalaCheckでArbitraryはGenを保持してるだけでそれ以外何もしてない？でQuickCheckだとGenとShrinkを保持するものになってる？で、まぁそれは設計判断としていいんだけど、ならArbitraryかGenのどちらかは不要だと思うけどなぜこうなってるのか</p>&mdash; Kenji Yoshida (@xuwei_k) <a href="https://twitter.com/xuwei_k/status/541898455698776065">December 8, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


!SLIDE

## ScalazのTypeclassの階層と重複テストの問題

<br />


- <http://xuwei-k.github.io/scalaz7.1.1.svg>
- <https://github.com/scalaz/scalaz/blob/v7.1.1/scalacheck-binding/src/main/scala/scalaz/scalacheck/ScalazProperties.scala>


!SLIDE

## 先駆者

<br />

- <https://github.com/typelevel/discipline>
- <http://typelevel.org/blog/2013/11/17/discipline.html>

!SLIDE

## disciplineの不満点

<br />

- Scalacheck前提
- 識別子がString固定？
- メンテ不安？

!SLIDE

# 実装方針


!SLIDE

# Functional Javaをパクる

<https://github.com/functionaljava/functionaljava/tree/v4.3/core/src/main/java/fj/test>

!SLIDE

# ScalacheckよりFunctional Javaのほうがとてもコード綺麗でわかりやすい!?

!SLIDE

## CoArbitraryはそのまま移植すれば解決

!SLIDE

## タイムアウトは少し改造すればできた

!SLIDE


## パラメータの細かい指定

<br />

基本的にかなり純粋関数型で作られてるので、あまりいじる必要ない


!SLIDE

## seedを指定して、ランダムだけど予測可能なテスト方法の提供


<br />


少し頑張ればできた

!SLIDE

## ScalazのTypeclassの階層と重複テストの問題


<br />

- 完全に独自に作った
- 試行錯誤した途中のなにか <https://gist.github.com/xuwei-k/be463dc93355e3f9c1c2>
- 有向非巡回グラフとして考えて、重複ノードを削除する


!SLIDE

## Gen Monad Transformer

<br />
<br />

気が向いたら、これから作る

!SLIDE

## コードまだ未公開

<br />

最初のリリースと同時に公開？


!SLIDE

## いいライブラリ名募集

<br />

- 特になければ ScalazCheck という当て付けのような名前に・・・
- もしくは最初からScalaz組み込み？


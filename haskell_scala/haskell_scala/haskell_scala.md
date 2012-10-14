!SLIDE

# Haskell と Scala

<p>
start haskell 2 第5回
<a style="font-size: 10%" rel="license" href="http://creativecommons.org/licenses/by/2.1/jp/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="http://i.creativecommons.org/l/by/2.1/jp/88x31.png" /></a>
</p>

!SLIDE

## 自己紹介

<img src="https://twimg0-a.akamaihd.net/profile_images/1931553270/xuwei.gif" width=200 height=200 />

* twitter [@xuwei_k](https://twitter.com/xuwei_k)
* github [xuwei-k](http://github.com/xuwei-k)
* blog [http://d.hatena.ne.jp/xuwei](http://d.hatena.ne.jp/xuwei)
* いま無職 ＼(^o^)／
* [Scalaを使って仕事をやっていた](http://www.slideshare.net/hitoasa/mongodb-13561725) 時期があった

!SLIDE

<span style="font-size:200%;">
<blockquote class="twitter-tweet" data-in-reply-to="249142255095525376"><p>@<a href="https://twitter.com/xuwei_k">xuwei_k</a> 需要全然あります！よければぜひとも！</p>&mdash; Seizan Shimazaki (@seizans) <a href="https://twitter.com/seizans/status/249147059276492800" data-datetime="2012-09-21T14:04:22+00:00">September 21, 2012</a></blockquote>
</span>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

* Scalaは詳しいけど、Haskellはあまり詳しくありません(>_<)
* なので、HaskellよりScalaの話が多いです(ぇ
* Scalaをdisりに来ました(ぇ

!SLIDE

<p><span style="font-size:50%;">すいません、宣伝</span><br /></p>

<p><span style="font-size:200%;">Scala Conference in Japan</span> というのをやります！</p><br />

* 海外から、Scala作ってる中の人やAkkaという有名なライブラリを作ってる人が来ます！
* [http://scalaconf.jp/](http://scalaconf.jp/)
* 2013年3月2日(土)
* 東京工業大学(大岡山キャンパス)

!SLIDE

* Haskell 使ってる人から見て、Scalaのイメージとは？
* そもそもScala使ったことあるのか？
* Scalaについてどれくらい知ってるのか？
* Scalaは関数型言語なのか？

!SLIDE

## Haskellと比べてScalaのだめなところ 1

* 型推論弱い <span style="font-size: 50%;"> (一応overloadの関係などある程度理由はあるらしい) </span>
* `Option` を使うことにより減りますが `null` からは完全に逃れられない・・・
* 標準で、 `Monad` とか `Applicative` とかがない・・・？
* 型クラス・・・？
* Haskellよりコンパイル遅い
* デフォルト遅延評価ではない <span style="font-size: 50%;"> (これは、メリットもあるけど) </span>

!SLIDE

## Haskellと比べてScalaのだめなところ 2

* 代数的データ型をあらわすのがちょっと冗長
* newtypeがサポートされてない <span style="font-size: 50%;"><a href="http://etorreborre.blogspot.jp/2011/11/practical-uses-for-unboxed-tagged-types.html">黒魔術的な(?)方法でエミュレートする方法が最近発見された]</a></span>
* 値が多相になれない <span style="font-size: 50%;"><a href="http://d.hatena.ne.jp/leque/20111226/p1">かなり冗長になるけどエミュレートする方法はある</a></span>
* rank2typeもない・・・<span style="font-size: 50%;"><a href="https://gist.github.com/3300312">これも冗長になるけどエミュレートする方法ある</a></span>

!SLIDE

<p>今日とりあえず一番言いたいこと</p>

<p>↓</p>

<span style="font-size: 200%;"> Scalaでの型クラスについて！ </span>

!SLIDE

* Scalaには型クラス専用の構文はない(?)

* けど、型クラスはそれなりに使われている

!SLIDE

<pre style="font-size: 200%;">
＿人人人人人人＿
＞　implicit　＜
￣^Y^Y^Y^Y^￣
</pre>

!SLIDE

* `implicit` という謎の予約語がある
* 暗黙・・・？
* なにそれこわい
* Scalaが複雑とか言われてしまう原因の一つ・・・？

!SLIDE

<span style="font-size: 200%">
<span style="font-family: Consolas, Menlo, 'Liberation Mono', Courier, monospace;">implicit</span> は、型クラスのための構文です(ｷﾘｯ
</span>

!SLIDE

* `implicit conversion` と `implicit parameter` という2種類がある
* `implicit conversion` は型クラスには、ほとんど関係ないので忘れてください

!SLIDE

### 型クラスの定義

<hr /><br />

```scala
trait Functor[F[_]]{
  def fmap[A, B](r: F[A], f: A => B): F[B]
}

```

<br /><span style="font-size:50%;"><a href="https://github.com/scalaz/scalaz/blob/v6.0.4/core/src/main/scala/scalaz/Functor.scala#L14">https://github.com/scalaz/scalaz/blob/v6.0.4/core/src/main/scala/scalaz/Functor.scala#L14</a></span>

<hr /><br />

```haskell
class  Functor f  where
    fmap        :: (a -> b) -> f a -> f b
```

<br /><span style="font-size:50%;"><a href="https://github.com/ghc/packages-base/blob/ghc-7.4.2-release/GHC/Base.lhs#L179">https://github.com/ghc/packages-base/blob/ghc-7.4.2-release/GHC/Base.lhs#L179</a></span>

!SLIDE

### 型クラスのインスタンスの定義

<hr /><br />

```scala
implicit def OptionFunctor: Functor[Option] =
  new Functor[Option] {
    def fmap[A, B](r: Option[A], f: A => B) = r map f
  }
```

<br /><span style="font-size:50%;"><a href="https://github.com/scalaz/scalaz/blob/v6.0.4/core/src/main/scala/scalaz/Functor.scala#L112">https://github.com/scalaz/scalaz/blob/v6.0.4/core/src/main/scala/scalaz/Functor.scala#L112</a></span>

<hr /><br />

```haskell
instance  Functor Maybe  where
    fmap _ Nothing       = Nothing
    fmap f (Just a)      = Just (f a)
```

<br /><span style="font-size:50%;"><a href="https://github.com/ghc/packages-base/blob/ghc-7.4.2-release/Data/Maybe.hs#L72">https://github.com/ghc/packages-base/blob/ghc-7.4.2-release/Data/Maybe.hs#L72</a></span>

!SLIDE

<p>とりあえず、Scalaで以下のものがでてきたらほとんどの場合型クラスのインスタンス定義だと思えばいい</p>
<br />

* `implicit val`
* `implicit object`
* (実引数なしで、型引数のみの) `implicit def`

!SLIDE

### 重要なこと1

<p>implicit parameterによる型クラスは
「偶然エミュレートできた」のではなく、
<span style="font-size: 200%;">最初から型クラスをエミュレートすることを意図して入れた！</span>
という事実</p><br />

詳しく知りたい人は、Scalaの作者自らが書いた、この論文とか読んでください

[Type Classes as Objects and Implicits](http://ropas.snu.ac.kr/~bruno/papers/TypeClasses.pdf)

!SLIDE

### 重要なこと2

Scalaには、for式というものがありますが、ループのための構文ではなく <span style="font-size: 200%;"> Monadのための構文 </span> です!

* 詳しいことは今日は解説しないけど、Haskellのdoと目的はだいたい同じ
* for式の一部の機能だけ使うと、なんとなくループっぽいから初級者〜中級者向けの解説では、それを説明してない
* さらに詳しいことを言えば、パターンマッチでfilterできたり、ifガードがあるので、MonadPlus相当(?)

!SLIDE

ところで、さっきのFunctorの定義は、Scala標準ライブラリにはありません

!SLIDE

<pre style="font-size: 200%;">
＿人人人人人＿
＞　Scalaz　＜
￣^Y^Y^Y^Y￣
</pre>

!SLIDE

## Scalaz とは・・・？

* Scala標準ライブラリにほとんど型クラスの定義がない
* 大量の型クラスの定義
* Scalaで型クラスを使ったプログラミングをするなら必須
* ほかにも似たようなライブラリあるけど、あまり使われてない [okomok/ken](http://github.com/okomok/ken)
* なのでデファクトスタンダード

!SLIDE

![ekmett1](http://cdn-ak.f.st-hatena.com/images/fotolife/x/xuwei/20121013/20121013095151.jpg?1350089630)

<br /><span style="font-size:50%;"><a href="https://github.com/scalaz/scalaz/blob/v6.0.4/core/src/main/scala/scalaz/Monad.scala">https://github.com/scalaz/scalaz/blob/v6.0.4/core/src/main/scala/scalaz/Monad.scala</a></span>

!SLIDE

![ekmett1](http://cdn-ak.f.st-hatena.com/images/fotolife/x/xuwei/20121013/20121013095908.jpg)

<br /><span style="font-size:50%;"><a href="https://github.com/scalaz/scalaz/graphs/contributors">https://github.com/scalaz/scalaz/graphs/contributors</a></span>

!SLIDE

* [Edward A. Kmett](https://github.com/ekmett) さんが、4番目に多くコミットしてる！？
* Haskell界隈で有名な人
* [comonad.com](http://comonad.com) の人
* [Lens](https://github.com/ekmett/lens) とかHaskellの有名なライブラリいっぱい作ってる

!SLIDE

<p>でも、Scalaをdisってたよ(>_<) </p>

<blockquote class="twitter-tweet"><p>@<a href="https://twitter.com/zedshaw">zedshaw</a> <a href="http://t.co/TIbxuWa8" title="http://codepad.org/6z1uQg3C">codepad.org/6z1uQg3C</a> shows why I'd rather just write it in Haskell. The parameters are only there because scala has crap inference.</p>&mdash; Edward Kmett (@kmett) <a href="https://twitter.com/kmett/status/240494760261996544" data-datetime="2012-08-28T17:03:13+00:00">August 28, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

### Scala

```scala
implicit def KleisliCategory[M[_]: Monad]: Category[({type λ[α, β]=Kleisli[M, α, β]})#λ]
  = new Category[({type λ[α, β]=Kleisli[M, α, β]})#λ] {
    def id[A] = ☆(_ η)
    def compose[X, Y, Z](f: Kleisli[M, Y, Z], g: Kleisli[M, X, Y]) = f <=< g
  }
```

<hr /><br />

### Haskell

```haskell
instance Monad m => Category (Kleisli m) where
  id = Kleisli return
  Kleisli f . Kleisli g = Kleisli (f <=< g)
```

!SLIDE

### Scalazにある型クラスの例

Equal(HaskellのEq)、Show、Order(HaskellのOrd)、Functor、Applicative、Monad、MonadPlus、Monoid、Arrow、Zipper、State、Writer、Reader、Traverse、IO、Kleisli、Free、Comonad、Bifunctor、Category、Cojoin、CoKleisli、Each、Endo、Cofree、Group、Length、Lens、Partial Lens、Semigroup、Zip、Zipper、Zap

!SLIDE

ちょっと話逸れます

!SLIDE

一週間くらい前

<blockquote class="twitter-tweet"><p>Freeモナドやばい、悟りを開いた気分だわ。</p>&mdash; ねこはる (@halcat0x15a) <a href="https://twitter.com/halcat0x15a/status/253984147297681408" data-datetime="2012-10-04T22:25:14+00:00">October 4, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

[Stackless Scala With Free Monads](http://days2012.scala-lang.org/sites/days2012/files/bjarnason_trampolines.pdf)

<blockquote class="twitter-tweet"><p>Freeモナドくわしく</p>&mdash; Hideyuki Tanaka (@tanakh) <a href="https://twitter.com/tanakh/status/254089546617217024" data-datetime="2012-10-05T05:24:03+00:00">October 5, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Free Monadが流行る

<blockquote class="twitter-tweet"><p>Freeモナドで悟りを開きました</p>&mdash; Hideyuki Tanaka (@tanakh) <a href="https://twitter.com/tanakh/status/254509230390910976" data-datetime="2012-10-06T09:11:43+00:00">October 6, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

<blockquote class="twitter-tweet"><p>RTされた数だけFreeモナドの解説記事書く</p>&mdash; Hideyuki Tanaka (@tanakh) <a href="https://twitter.com/tanakh/status/254512595313246208" data-datetime="2012-10-06T09:25:05+00:00">October 6, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

200RT以上！？

![tanakh free](http://cdn-ak.f.st-hatena.com/images/fotolife/x/xuwei/20121013/20121013102836.png?1350091738)

!SLIDE

* Haskellでも、まだあまり使われてないもの( Free Monad や Partial Lens など )まで、Scalaでできるらしい
* でもとにかく型をいっぱい書かなければいけなくて大変！
* Scalaで型クラスを使ったプログラミングをする利点とは・・・？

!SLIDE

1点だけ利点があるらしい

!SLIDE

<span style="font-size: 50%;">Haskellと違いScalaでは</span>

<span style="font-size: 200%;">型クラスのインスタンスを複数持てる(スコープによって自分で制御できる)</span>

!SLIDE

* それは嬉しいのか・・・？
* 型クラスのインスタンスを複数持ちたいことってあまりない・・・？
* newtypeあるし
* Haskellそれほど書いたことないから、そのあたりの感覚がよくわからない
* 将来的にGHCでも、そのあたりの制限を緩和するという話を聞いたことが(?) (もしくは既に最新版に入ってる？)
* 詳しくないのでだれか教えてください(>_<)

!SLIDE

* 最終的に (Haskellerが) Scalaをやるメリットとは・・・？
* Haskellと比べてScalaをやったほうがいい人とは？

!SLIDE

* たぶんHaskellが既にそれなりにできるなら、それほどScalaメリットない(?)
* 逆にScalaだけやってるひとがHaskellやるのはある程度メリットある気がする
* 仕事に関数型言語導入したくて、「Haskellの導入は難しそうだけど、Scalaなら導入できるのではないか？」という人はScalaやっておくといいかも・・・？

!SLIDE

* 平凡な結論だが「Javaを知っていて、関数型プログラミングやりたい」という人にとっては、HaskellよりもScalaのほうが、「手続き型から徐々に関数型なプログラミングに移行しやすい」という点では入門しやすいかもしれない
* けど、本格的に型クラスを大量に使うスタイルでプログラミングをやろうとすると、ほとんどの場合Haskellのほうが簡潔に書ける

!SLIDE

* ただ、(冗長だけれども) Haskellで現状あるほとんどの型クラスは、Scalaでもすでに存在していて型クラスを使ったプログラミングができる！という事実がそこまで知られていないので、もうちょっとその事実が広まってもいいと思う

!SLIDE

おわり。ありがとうございました

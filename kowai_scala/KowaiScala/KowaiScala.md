!SLIDE


# 怖いScala！


!SLIDE

<img src="https://pbs.twimg.com/profile_images/1931553270/xuwei.gif" width="100" height="100" />

- twitter [@xuwei_k](https://twitter.com/xuwei_k)
- github [@xuwei-k](https://github.com/xuwei-k)
- blog <http://xuwei-k.hatenablog.com/>

!SLIDE

- 無職＼(^o^)／
- もう少し無職楽しむつもりだけど、なんとなく関数型言語書いて金貰えるような仕事ないかな・・・


!SLIDE


- 今年の前半は働いてたけど、去年の今頃も無職だった
- 暇だからScalazにひたすらpull req送る
- 気づいたらコミッターになっていた((((；ﾟДﾟ))))!?
- 2013年11月現在、コミット数3位<span style="font-size: 30%;">(コミットした行数では7位くらい。つまり細かいコミットが多い)</span>


!SLIDE

<https://github.com/scalaz/scalaz/graphs/contributors>

![scalaz-graph](http://cdn-ak.f.st-hatena.com/images/fotolife/x/xuwei/20131128/20131128063417.png)


!SLIDE


## 怖いScalaとは・・・

- 怖い人がいる？
- implicit、なにそれ怖い？
- Scalaz怖い？<span style="font-size: 30%;">(実はもっと怖いライブラリが・・・)</span>
- モナド怖い？
- 関数型怖い？


!SLIDE

怖い(?)と思われてるものを真面目に話そう！


!SLIDE

怖い人達(?)の話

!SLIDE


## 最近の有名な2つの事件

- [twitter の bijection というライブラリの名前問題](http://togetter.com/li/436709)
- [scala.util.Try](http://togetter.com/li/361897)


!SLIDE


## 数々の名言(?)

- [Jodatimeはゴミ](https://twitter.com/dibblego/status/327919449325842432)
- [GoFのデザインパターンにメリットなんてひとつもない](https://twitter.com/dibblego/status/347282744692322304)
- [ScalaにIOのdata typeがないから、sbt使うの難しい](https://twitter.com/dibblego/status/225000527073775616)
- ["I believe the reader monad can't be ... used ... without ... special language options ... on" Odersky, wrong again. ](https://twitter.com/dibblego/status/405220083414204416)


!SLIDE

- [Erasure is one of the few things that Java got right. C# fell into the trap afterward.](https://twitter.com/dibblego/status/336995275149287425)
- [the #scala ZipAll made me puke.](https://twitter.com/dibblego/status/380201589698355200)
- [SIP-18 is just another bad idea serving nobody](http://tmorris.net/posts/2012-05-11-sip-18-is-just-another-bad-idea-serving-nobody.html)
- その他、昔からodersky先生と犬猿の仲らしく、数えきれないほど色々あるらしい

!SLIDE



## いい人？

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/adelbertchang">@adelbertchang</a> <a href="https://twitter.com/larsr_h">@larsr_h</a> <a href="https://twitter.com/xuwei_k">@xuwei_k</a> Kenji is rock and rolling like a champ.</p>&mdash; Tony Morris (@dibblego) <a href="https://twitter.com/dibblego/statuses/381753674864668672">September 22, 2013</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


((((；ﾟДﾟ))))ｶﾞｸｶﾞｸﾌﾞﾙﾌﾞﾙ


!SLIDE

Scalazやモナドの話

!SLIDE

- 7.0.0が2013年4月22日リリース
- 2013年11月28日現在、stableな最新版は 7.0.5
- 7.0.0から7.0.5までは(基本的に)バイナリ互換がある
- 細かいバグ修正、互換性を壊さない機能追加のリリースが5回


!SLIDE

- 現在7.1.0を同時に開発中
- 7.1.xは7.1.0-M4が最新版
- 7.1.xは現在マイルストーン版なので、バイナリ互換はない
- Scala2.11.0.finalがでる頃(2014年初め)に、7.1.0のfinalリリース予定
- 7.1.0のfinalがでたら、その後の7.1.1や7.1.2はバイナリ互換保つ予定

!SLIDE

- 7.0.xは、Scala2.9.2以上
- 7.1.xは、Scala2.9.3
- 7.2.xは、Scala2.9を切り捨てる予定？
- 7.2.xはまだ開発始まってないので、詳細は未定
- その他versionについての詳細はwikiを参照
- <https://github.com/scalaz/scalaz/wiki>


!SLIDE

<http://leifwarner.net/scalaz.svg>
<span style="font-size: 50%;">この図は7.0.x系なので、7.1.xと少し違う</span>

<iframe src="http://leifwarner.net/scalaz.svg">
</iframe>

!SLIDE

<p>モナド!</p>
<p style="font-size: 150%;">モナド!</p>
<p style="font-size: 200%;">モナド!</p>

!SLIDE

モナドを避けているから、必要以上に怖く感じるのでは？
モナドを理解してしまおう！

!SLIDE


- 世間には「モナドはコンテナ」とかそういう説明があったりします
- が、個人的にはそういう「変な比喩」を使った説明好きではない
- 比喩は理解を助けるかもしれない、が単に余計な混乱を招く可能性


!SLIDE

では、モナドとはなにか？


!SLIDE

単なる型クラスの1つ

!SLIDE

では、型クラスとは？

!SLIDE


[Scalaで型クラス入門](http://chopl.in/blog/2012/11/06/introduction-to-typeclass-with-scala.html)


!SLIDE

- Scala標準ライブラリでも使われてる
- Scalaz以外の多くのライブラリでも使われている

!SLIDE

型クラスの定義
<https://github.com/scala/scala/blob/v2.10.3/src/library/scala/math/Ordering.scala>

```scala
trait Ordering[T]{
  def compare(x: T, y: T): Int
}
```

<span style="font-size: 30%;">上記のコードは色々省略してあります</span>

!SLIDE

型クラスのインスタンスの定義

<https://github.com/scala/scala/blob/v2.10.3/src/library/scala/math/Ordering.scala#L250-L256>

```scala
trait IntOrdering extends Ordering[Int] {
  def compare(x: Int, y: Int) =
    if (x < y) -1
    else if (x == y) 0
    else 1
}
implicit object Int extends IntOrdering
```


!SLIDE

型クラスを使う側
<https://github.com/scala/scala/blob/v2.10.3/src/library/scala/collection/TraversableOnce.scala#L214>

```scala
def max[B >: A](implicit cmp: Ordering[B]): A
```


!SLIDE

- implicitは型クラスのためのもの！
- implicitは型クラス以外にも使えてしまう
- 型クラスは素晴らしいので、どんどん使いましょう
- 型クラス以外のimplicitの使いすぎに注意
- そのimplicitは、型クラスとしての使い方なのか、そうでないのか意識しよう

!SLIDE


ほとんどの型クラスには「満たすべき法則がある」


!SLIDE

法則とは？

- 例えるなら、Javaで`equals`と`hashCode`が特定の法則を満たさないといけないようなもの
- 先ほどの`Ordering`ならば、`a > b` かつ `b > c` ならば `a > c` など(推移律)

!SLIDE

モナドの型クラスの定義

```scala
trait Monad[F[_]]{
  def bind[A, B](fa: F[A])(f: A => F[B]): F[B]
  def point[A](a: => A): F[A]
}
```


!SLIDE

モナド則とは

!SLIDE


```
return a >>= k  ==  k a
 m >>= return  ==  m
 m >>= (\x -> k x >>= h)  ==  (m >>= k) >>= h
```

!SLIDE

Haskellではあれなので、Scalazにおける例


!SLIDE

モナド則その1

<https://github.com/scalaz/scalaz/blob/v7.1.0-M4/core/src/main/scala/scalaz/Monad.scala#L71>

```scala
def leftIdentity[A, B](a: A, f: A => F[B])(implicit FB: Equal[F[B]]): Boolean =
  FB.equal(bind(point(a))(f), f(a))
```


!SLIDE

モナド則その2

<https://github.com/scalaz/scalaz/blob/v7.1.0-M4/core/src/main/scala/scalaz/Monad.scala#L73>

```scala
def rightIdentity[A](a: F[A])(implicit FA: Equal[F[A]]): Boolean =
  FA.equal(bind(a)(point(_: A)), a)
```

!SLIDE

モナド則その3

<https://github.com/scalaz/scalaz/blob/v7.1.0-M4/core/src/main/scala/scalaz/Bind.scala#L40-L41>

```scala
def associativeBind[A, B, C](fa: F[A], f: A => F[B], g: B => F[C])(implicit FC: Equal[F[C]]): Boolean =
  FC.equal(bind(bind(fa)(f))(g), bind(fa)((a: A) => bind(f(a))(g)))
```

!SLIDE

もうみなさん、モナドを理解しましたね！


!SLIDE

提案:

- 「モナドはなにか？」

ということと

- 「モナドはどのように役に立つのか？」
- 「具体的にどのようなモナドがあるのか？」

を分けて考えよう！


!SLIDE

- 「モナドはなにか？」と言われたら、いま説明したもの以外のなにものでもない。
- 「具体的にモナドになるもの」は大量にあるので、それはゆっくり覚えていけばよい。

!SLIDE

- 「自分で新しいモナドを定義する」場合以外は、モナド則をそこまで気にする必要ない
- Scalazには<a href="https://github.com/scalaz/scalaz/blob/v7.1.0-M4/scalacheck-binding/src/main/scala/scalaz/scalacheck/ScalazProperties.scala#L191-L207">「モナド則が満たされているかどうかテストする仕組み」</a>があるので、それを使おう


!SLIDE


型クラスの継承関係

- 型クラスにも"継承"という概念が存在する
- ここからモナドと他の型クラスの継承関係について

!SLIDE

Monadの親の型クラス

- `InvariantFunctor` <span style="font-size: 30%;">(これはめったに使わないので忘れてもいいです)</span>
- `Functor`
- `Apply`
- `Bind`
- `Applicative`

!SLIDE

ScalazとHaskellと違い

- Haskellの標準ライブラリには `Functor`, `Applicative`, `Monad` の3つのみ
- Haskellの場合それら3つは継承関係ではない
- Haskellの標準ライブラリには存在しない `Apply`, `Bind` がScalazにはある
- Haskellの標準ライブラリには`Semigroup`はない
- その他いろいろ

!SLIDE

- `Apply` ならば、必ず `Functor` になる
- `Applicative` ならば、必ず `Apply` になる
- `Bind` ならば、必ず `Apply` になる
- `Monad` ならば、必ず `Applicative` になる
- `Monad` ならば、必ず `Bind` になる

!SLIDE

- `Bind` にはなるが `Monad` にはならない例が存在する(Mapなど)
- `Applicaive` にはなるが `Monad` にはならない例( `scalaz.Validation` など)


!SLIDE

よくある誤解

- Monadではない、むしろMonadと呼ぶには間違っている(単なるApplicativeの例に対して)Monadという言葉を使う
- Monadではなく、MonadPlusが関係することに関してMonadという言葉を使う。
- 参考 [HaskellのdoとScalaのfor式とEitherとMonadPlus](http://d.hatena.ne.jp/xuwei/20130517/1368814058)

マサカリが飛んでくるかもしれません?


!SLIDE

- 先ほど説明したように、一般的にMonadが満たすべき性質は3つ
- しかし、ScalazのMonadは多くの他の型クラスを継承している
- よって、親の型クラスの性質も満たさなければならない
- すべて合わせると**13個**になる！！！
- しかし、デフォルト実装を使っていれば必ず満たすので、普通は気にする必要ない

!SLIDE

- モナドという概念自体は、これまで説明したようなものでそれほど複雑ではない
- しかし`scalaz.Monad`を含めた様々な型クラスを使ったり、新たに定義する場合には色々な知識が必要

!SLIDE

また、`Monad` と同時に、それらと関連する型クラスである `Functor` なども同時に理解するようにしたほうがいい

!SLIDE

Scalazを使いたくてもどこから使っていいかわからない人向けの話

!SLIDE

- `scalaz.Validation`
- `Applicative#sequence`

!SLIDE

なぜ `Validation` がScalaz初心者にオススメなのか？


!SLIDE

- Monadにならないが、Applicativeになる例
- 実用的！
- ついでに、`Semigroup` も関連する
- Scalaz独自のデータ型である `NonEmptyList` の利点が生かせる

!SLIDE

`scalaz.Validation` と同じような実装

- playframework2の`JsResult`
<https://github.com/playframework/playframework/blob/2.2.1/framework/src/play-json/src/main/scala/play/api/libs/json/JsResult.scala>
- scalautilsの`Or`<span style="font-size: 30%;">(scalatestの人が作ったもの)</span>
<https://github.com/scalatest/scalatest/blob/release-2.0-for-scala-2.10/src/main/scala/org/scalautils/Or.scala#L581>

!SLIDE

- 似たようなものを再実装するくらいなら、最初から既存のもの使おう！
- 再実装するとしても、コード読むことで参考になるはず


!SLIDE

`Applicative#sequence`

<https://github.com/scalaz/scalaz/blob/v7.1.0-M4/core/src/main/scala/scalaz/Applicative.scala#L39-L40>


!SLIDE

Scala2.10から入った`Future`を使う場合

- `Option[Future[A]]` を `Future[Option[A]]` にしたい

とかよくあるはず

!SLIDE

Scalaz7.0.xの場合は、`scala.concurrent.Future`の型クラスのインスタンスは入ってないので以下の設定が必要

```scala
libraryDependencies ++= Seq(
  "org.typelevel" %% "scalaz-contrib-210" % "0.1.5",
  "org.scalaz" %% "scalaz-core" % "7.0.5"
)

scalaVersion := "2.10.3"
```

!SLIDE

```scala
import scala.concurrent.Future
import scalaz._
import Scalaz._
import scalaz.contrib.std.scalaFuture._ // 7.0.xの場合
import scalaz.std.scalaFuture._ // 7.1.0-M4の場合
import scala.concurrent.Future
import scala.concurrent.ExecutionContext.Implicits.global

```

!SLIDE

```scala
Option(Future(1)).sequence // Future(Option(1))
```

!SLIDE

`A[B[C]]`

という型のオブジェクトがあったとき、以下のどちらかであれば

- AがTraverse、BがApplicative
- AがTraverse1、BがApply

`A[B[C]]` から `B[A[C]]` に変換できる


!SLIDE

- 質問？
- `Validation` や `Applicative`の`sequence`関数の説明のライブコーディング？
- <https://gist.github.com/xuwei-k/7689965>


!SLIDE

おわり。ありがとうございました

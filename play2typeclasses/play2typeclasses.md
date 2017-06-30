!SLIDE

# Play2 の Json と型クラス

!SLIDE

<img src="https://pbs.twimg.com/profile_images/1931553270/xuwei.gif" width="100" height="100" />

- twitter [@xuwei_k](https://twitter.com/xuwei_k)
- github [@xuwei-k](https://github.com/xuwei-k)
- blog <http://xuwei-k.hatenablog.com/>
- 2014年3月からドワンゴで働いてる(まだ3ヶ月)

!SLIDE

# 社内Scala事情

- 社内のScalaのプロジェクトはほぼPlay2
- Scalaのプロジェクト6〜7個以上ある？(自分も把握できてない)
- Scalaを書いてるプログラマは30人以上?( [出典@mesoさん](https://twitter.com/meso/status/430965252444786688) )

!SLIDE

以下の様なことを話したいが、詰め込み過ぎたので、全部丁寧に話せるか謎

<br />

- 型クラスとは？
- play2にある型クラス
- Scalaにおける型クラス
- `Applicative` と `Monad`
- Play の `Reads` や `Writes` などが、なぜあのような書き方をするのか？
- Play と Scalaz の比較

!SLIDE

これから話すことは、どのversionでもそれほど大きく変わってませんが、特に言及がない場合は、[play2.2.3](https://github.com/playframework/playframework/tree/2.2.3)とします

!SLIDE

### 最終的な目標


<br />

```scala
case class Location(lat: Double, long: Double)
case class Resident(name: String, age: Int, role: Option[String])
case class Place(name: String, location: Location, residents: Seq[Resident])
```

<br />

(playの公式ドキュメントより引用)

<br />

- <http://www.playframework.com/documentation/2.2.3/ScalaJsonCombinators>



!SLIDE

これを理解できるように

<hr />
<br />

```scala
implicit val locationReads: Reads[Location] = (
  (__ \ "lat").read[Double](min(-90.0) keepAnd max(90.0)) and
  (__ \ "long").read[Double](min(-180.0) keepAnd max(180.0))
)(Location.apply _)

implicit val residentReads: Reads[Resident] = (
  (__ \ "name").read[String](minLength[String](2)) and
  (__ \ "age").read[Int](min(0) keepAnd max(150)) and
  (__ \ "role").readNullable[String]
)(Resident.apply _)

implicit val placeReads: Reads[Place] = (
  (__ \ "name").read[String](minLength[String](2)) and
  (__ \ "location").read[Location] and
  (__ \ "residents").read[Seq[Resident]]
)(Place.apply _)
```

!SLIDE

この2つの違い

<br />

```scala
implicit val placeReads: Reads[Place] = (
  (__ \ "name").read[String](minLength[String](2)) and
  (__ \ "location").read[Location] and
  (__ \ "residents").read[Seq[Resident]]
)(Place.apply _)
```

<br /><hr />

```scala
implicit val placeReads: Reads[Place] = for{
  name      <- (__ \ "name").read[String](minLength[String](2))
  location  <- (__ \ "location").read[Location]
  residents <- (__ \ "residents").read[Seq[Resident]]
} yield Place(name, location, residents)
```

!SLIDE

for式 = モナド = カッコイイ！

と思ってる人はまだScala初心者(?)


あんな気持ち悪い(?)書き方をしているのは理由がある


!SLIDE


### play2にある型クラス

<br />

[Monoid](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Monoid.scala#L3),
[Reducer](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Monoid.scala#L19),
[Functor](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Functors.scala#L7),
[InvariantFunctor](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Functors.scala#L13),
[ContravariantFunctor](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Functors.scala#L19),
[Applicative](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Applicative.scala#L5),
[Alternative](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Alternative.scala#L5),
[Reads](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Reads.scala#L17),
[Writes](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Writes.scala#L14),
[OWrites](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Writes.scala#L36),
[Format](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Format.scala#L10),
[OFormat](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Format.scala#L11),
[Writeable](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/http/Writeable.scala#L20),
[ContentTypeOf](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/http/ContentTypeOf.scala#L21),
[Formetter](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/data/format/Format.scala#L13),
[QueryStringBindable](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/mvc/Binders.scala#L65),
[PathBindable](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/mvc/Binders.scala#L150),
[JavascriptLitteral](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/mvc/Binders.scala#L190)

<br />

Play内部には、意外と型クラスがいっぱいある

!SLIDE

勝手に3種類くらいに分類


<br />


1. HaskellやScalazにもある型クラス
2. Json関連の型クラス
3. その他の型クラス


!SLIDE


### HaskellやScalazにもある型クラス


<br />


[Monoid](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Monoid.scala#L3),
[Reducer](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Monoid.scala#L19),
[Functor](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Functors.scala#L7),
[InvariantFunctor](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Functors.scala#L13),
[ContravariantFunctor](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Functors.scala#L19),
[Applicative](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Applicative.scala#L5),
[Alternative](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-functional/src/main/scala/play/api/libs/functional/Alternative.scala#L5)


<br />


- 実際それほど汎用的に使われてるわけではない？
- Jsonのために導入された感じ


!SLIDE

Json関連の型クラス


<br />


[Reads](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Reads.scala#L17),
[Writes](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Writes.scala#L14),
[OWrites](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Writes.scala#L36),
[Format](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Format.scala#L10),
[OFormat](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Format.scala#L11)


<br />


- これらが重要なので、詳しく話したいが

!SLIDE

その他


<br />


[Writeable](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/http/Writeable.scala#L20),
[ContentTypeOf](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/http/ContentTypeOf.scala#L21),
[Formetter](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/data/format/Format.scala#L13),
[QueryStringBindable](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/mvc/Binders.scala#L65),
[PathBindable](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/mvc/Binders.scala#L150),
[JavascriptLitteral](https://github.com/playframework/playframework/blob/2.2.3/framework/src/play/src/main/scala/play/api/mvc/Binders.scala#L190)



<br />


- WriteableやContentTypeOfは、普通にやるぶんには、そこまで意識する必要ない
  - 独自のオブジェクトを、レスポンスとして返却する場合に使う(たとえば [play-json4s](https://github.com/tototoshi/play-json4s) )


!SLIDE

implicit使われてるけど、型クラスではない(と個人的に思う)もの


<br />


- Application
- Request


<br />


環境引き回す系?

!SLIDE

<img src="http://xuwei-k.github.io/slides/play2typeclasses/play2typeclasses.svg">


!SLIDE

Haskellで表現(1)

```haskell

-- ちょっと省略してるので不正確
data JsResult a = JsSuccess a | JsError

class Writes a where
  writes :: a -> JsValue

class Writes a => OWrites a where
  writes :: a -> JsObject -- Haskellでは本当はオーバーライドは不可能

```

!SLIDE

Haskellで表現(2)

```haskell

class Reads a where
  reads :: JsValue -> JsResult a

class (Writes a, Reads a) => Format a where

class (OWrites a, Reads a) => OFormat a where

```

!SLIDE

Haskellで表現(3)

```haskell

-- https://github.com/ekmett/contravariant/blob/v0.5.2/Data/Functor/Contravariant.hs#L76
class ContravariantFunctor a where
  contramap :: (a -> b) -> f b -> f a

```

!SLIDE

Haskellで表現(4)

```haskell

instance Alternative JsResult where
  -- 実装は略

instance Alternative Reads where
  -- a -> f b において、fがAlternativeなら
  -- 自動的にa -> f bもAlternative

instance ContravariantFunctor Writes where
  -- Writesは実質単なる a -> JsValue という関数と同型なので
  -- ContravariantFunctorになるのは当たり前

```

!SLIDE

### 型クラスを一から丁寧に説明していたら、play2の型クラスの説明ができないので、他の資料に頼る


<br />


- [怖くない型クラス (@kmizu さん)](http://www.slideshare.net/kmizushima/ss-28707326)
- [Scalaで型クラス入門](http://chopl.in/blog/2012/11/06/introduction-to-typeclass-with-scala.html)

!SLIDE

### 勘違いされがちなこと


<br />


- モナドは型クラスの1つにすぎない。そういう意味では何も特別ではない
- Scalaのimplicitは、ある意味型クラスのために導入されたという経緯(?)
  - [おだすきせんせーの論文](http://ropas.snu.ac.kr/~bruno/papers/TypeClasses.pdf)
  - implicitは型クラス以外のものにも使えてしまう
  - 型クラスとしてのimplicitは便利なのでどんどん使おう(?)


!SLIDE

Scalaでの型クラス

<br />

- 型クラスは`trait`や`class`で定義
- 型クラスのインスタンス定義方法は、以下のどれか
    - `implicit val` (`lazy`付く場合も)
    - `implicit def` (引数ある場合とない場合と両方あり得る)
    - `implicit object`

- 使う側は、`implicit parameter`で受け取る

!SLIDE

playでの具体例

!SLIDE

型クラス定義

<br />

```scala

trait Reads[A] {
  def reads(json: JsValue): JsResult[A]

  // その他のメソッド  
}

```

<br />

<span style="font-size: 60%;">
<a target="_blank" href="https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Reads.scala#L17-L23">Readsのソースコード</a>
</span>


!SLIDE

型クラスのインスタンス定義

<br />

```scala
implicit object StringReads extends Reads[String] {
  def reads(json: JsValue) = json match {
    case JsString(s) => JsSuccess(s)
    case _ => JsError(Seq(JsPath() -> Seq(ValidationError("error.expected.jsstring"))))
  }
}
```

<br />

<span style="font-size: 60%;">
<a target="_blank" href="https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Reads.scala#L354-L359">Readsのソースコード</a>
</span>

!SLIDE

なぜ型クラスを受け渡すのに`implicit`を使うのか？

!SLIDE

型クラスは、型によって、値が一つに決まるから(むしろ、そうでなければ型クラスと呼ばない?)


!SLIDE

"型によって、値が一つに決まる" ということはつまり

型パラメータをとらない`implicit`なものは型クラスではない

<br />

- playの`Application`
- `scala.concurrent.ExecutionContext`
- `scala.io.Codec`


!SLIDE

一つに決まらない場合どうするのか？

<br />

- Haskellならnewtype
- ScalaだとHaskellに比べて新しい型を作るのが面倒(かつ、実行時コストかかる)

!SLIDE

Scalaでの解決策

<br />

- implicitな部分に明示的に引数を渡す
- スコープで制御できるならスコープで制御
    - Scalaにはimplicitの解決に優先順位やスコープがあるので
    - 優先順位は複雑(Scalaのversionによって微妙に異なる？)
    - コンパニオンオブジェクトにimplicitなインスタンスあると、それを使いたい場合は便利だけど、使いたくない場合不便
- Tagged Type ？(それほど実用されてない。デメリットもあり) 


!SLIDE

実際仕事でjodatimeのDateTime型のReadsのインスタンスがReadsのコンパニオンに存在していて、しかしそれを使いたくない自体が発生して
面倒な状況に！

<https://github.com/playframework/playframework/blob/2.2.3/framework/src/play-json/src/main/scala/play/api/libs/json/Reads.scala#L267-L295>


!SLIDE

`Monad` と `Applicative`

!SLIDE

- `Monad` も `Applicative` も単なる型クラスの1つ
- play内部には `Applicative` はあるが `Monad` は現状では存在しない
- `Monad` は必ず `Applicative` (Haskellでも次期versionで修正されるらしい)

!SLIDE

### Scalazのクラス図

<img src="http://xuwei-k.github.io/scalaz-typeclasses.svg">

!SLIDE

- <http://xuwei-k.github.io/scalaz-typeclasses.svg>
- <http://d.hatena.ne.jp/xuwei/20131213/1386923702>

!SLIDE

[別の図](http://leifwarner.net/scalaz.svg)

<img src="http://leifwarner.net/scalaz.svg">


!SLIDE

なぜ `Applicative` が重要か？

<br />

- `Monad` にも `Applicative` にもなるが、`Applicative` にする方法が2種類ある場合
- `Monad` にはならないが `Applicative` になるものが存在する
    - そういう場合には、for式が使えない
    - なので、最初に出した、気持ち悪い(?)記法
    - もしくは、Scalazの `|@|` という謎な記号
- `Monad` にすることもできるが、それと整合性がない `Applicative` が存在する


!SLIDE

やっとplayの `Reads` や `Writes` の話

<br />

- playの `Reads` や `Writes` は `Applicative`
- Scalaz知ってる人にとっては、以下の様なものだと思えばいい

<br />

```scala
type JsResult[+A] = Validation[Seq[Error], A] // ちょっと正確じゃない
```

```scala
trait Reads[A] {
  def reads(json: JsValue): JsResult[A]
}
```

!SLIDE

- `Reads` は `JsValue => JsResult[A]` と同型
- `JsResult` は `Applicative`
- `A => F[B]` において、`F[_]` が `Applicative` ならば `A => F[B]` 自体も `Applicative`

!SLIDE

- [play2公式ドキュメント(英語)](http://www.playframework.com/documentation/2.2.x/ScalaJson)
- [play2公式ドキュメントの日本語訳(少し古い)](http://www.playframework-ja.org/documentation/2.1.5/ScalaJson)


!SLIDE

- 重要なのは、playの`Reads`は、エラーを蓄積する機能があるということ
- `JsResult` 自体にエラーを含んでいるので、`Reads` 自体は例外を投げるべきではない
- できるだけ宣言的に書く
- エラーメッセージの国際化の機能まで組み込まれてる(?)

!SLIDE

- インスタンスが1つだけなら、基本的にコンパニオンオブジェクトに定義すると、勝手にスコープに入るので便利
- `Reads` も `Writes` も定義するなら、別々に定義するのではなく、`Format` で一緒に定義したほうが整合性がとれて良い

!SLIDE

- マクロで定義する機能もあるが、`apply` が複数あると使えなかったり、case classのフィールド名変えるだけでJsonのkey変わってしまうので微妙
- ライブラリ作りました
    - <http://d.hatena.ne.jp/xuwei/20140402/1396408213>
    - <https://github.com/xuwei-k/play-json-extra>
    - nullableに対応できない・・・(ReadsのコンパニオンにOptionのReadsがあるので)
    - key自体がない場合と、keyは存在してvalueがnullの場合と区別がある
    - `readNullable[Int]` と `read[Option[Int]]`
    - 2の22乗通りのメソッドを作れば・・・

!SLIDE


- varianceの話もしたかったが、無理ですよね
- 共変とか反変とか
- これ素晴らしかったので、読むといいよ
    - scodecという、シリアライズ、デシリアライズのライブラリ作ってる人
    - scalaz-streamで使われてる
    - Scalaのvarianceに関する詳しい説明
    - Bivarianceとかhiger kindな場合のvarianceとか
    - <https://github.com/mpilquist/variance-explorations>
    - <https://github.com/mpilquist/variance-explorations/raw/master/scodec-variance.pdf>


!SLIDE

おわり
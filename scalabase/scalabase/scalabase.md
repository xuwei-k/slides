!SLIDE

# [Type Classes as Objects and Implicits](http://ropas.snu.ac.kr/~bruno/papers/TypeClasses.pdf)

[Scala基礎勉強会](http://kokucheese.com/event/index/51993/)
<a style="font-size: 10%" rel="license" href="http://creativecommons.org/licenses/by/2.1/jp/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="http://i.creativecommons.org/l/by/2.1/jp/88x31.png" /></a>

!SLIDE

## 自己紹介

<img src="https://twimg0-a.akamaihd.net/profile_images/1931553270/xuwei.gif" width=200 height=200 />

* twitter [@xuwei_k](https://twitter.com/xuwei_k)
* github [xuwei-k](http://github.com/xuwei-k)
* blog [http://d.hatena.ne.jp/xuwei](http://d.hatena.ne.jp/xuwei)
* 無職 ヽ('ω')ﾉ三ヽ('ω')ﾉ
* [Scalaを使って仕事をやっていた](http://www.slideshare.net/hitoasa/mongodb-13561725) 時期があった

!SLIDE

<script src="http://togetter.com/js/parts.js"></script><script>tgtr.ListWidget({id:'368588',url:'http://togetter.com/',width:'640px',height:'480px'});</script>

!SLIDE

<blockquote class="twitter-tweet"><p><a href="https://twitter.com/search/%23ScalaBase">#ScalaBase</a> <a href="https://twitter.com/search/%23ScalaBase合宿">#ScalaBase合宿</a> よっぱらう発表者! 寝る発表者! Scalaの勉強をする参加者! 仕事をするbleisさん!</p>&mdash; mzp (@mzp) <a href="https://twitter.com/mzp/status/259646231092019200" data-datetime="2012-10-20T13:24:20+00:00">October 20, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

一杯しか飲んでいないのに、酔っ払って寝不足で9〜12時くらいまで寝てましたすいません。

そして、起きて、このスライドを仕上げようとしたら・・・

!SLIDE

<blockquote class="twitter-tweet"><p>となりの部屋から、AV観てるのか、実際呼んでるのかわからないけど、喘ぎ声が聞こえてくる件 <a href="https://twitter.com/search/%23ScalaBase">#ScalaBase</a></p>&mdash; Kenji Yoshida (@xuwei_k) <a href="https://twitter.com/xuwei_k/status/259699077455966209" data-datetime="2012-10-20T16:54:19+00:00">October 20, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

<blockquote class="twitter-tweet"><p>レストランに並びながら、ほぼ全員PCを広げている謎の集団 <a href="https://twitter.com/search/%23ScalaBase">#ScalaBase</a> <a href="http://t.co/89d18kLl" title="http://twitter.com/xuwei_k/status/259844406356615169/photo/1">twitter.com/xuwei_k/status…</a></p>&mdash; Kenji Yoshida (@xuwei_k) <a href="https://twitter.com/xuwei_k/status/259844406356615169" data-datetime="2012-10-21T02:31:50+00:00">October 21, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

<pre style="font-size: 35%;">
<span style="font-size: 150%;">1. Introduction</span>
<br />
<span style="font-size: 150%;">2. Type Classes in Haskell</span>
  2.1 Single-parameter type classes
  2.2 Common extensions
<br />
<span style="font-size: 150%;">3. Implicits</span>
  3.1 Implicits in Scala
  3.2 Implicits as the missing link
<br />
<span style="font-size: 150%;">4. The CONCEPT Pattern</span>
  4.1 Concepts: type-class-style interfaces in OO
</pre>

!SLIDE

<pre style="font-size: 35%;">
<span style="font-size: 150%;m">5. Applications and Comparison with Type Classes</span>
  5.1 Ordering concept
  5.2 Abstract data types
  5.3 Statically-typed printf
  5.4 Type class programs are OO programs
<br />
<span style="font-size: 150%;">6. Advanced Uses of Type Classes</span>
  6.1 Associated types in GHC Haskell
  6.2 Implicits and type members
  6.3 Session types
  6.4 Arity-polymorphic ZipWith in Scala
  6.5 ZipWith using prioritised overlapping implicits
  6.6 Encoding generalized constraints
  6.7 Type theories using implicits
<br />
<span style="font-size: 150%;">7. Discussion and Related Work</span>
  7.1 Real-world applications
  7.2 Type classes, JavaGI and concepts
  7.3 Generic programming in the large
<br />
<span style="font-size: 150%;">8. Conclusion</span>
</pre>

!SLIDE

* 2010年の論文
    * 2.8.0が出る前
    * [2.8.0.final](https://github.com/scala/scala/tree/v2.8.0) がでたのは 2010/7/14
* 書いた人
    * Odersky先生
    * Adriaan Moors( EPFLの人で、昔からScala関わってる人 )
    * Bruno C. d. S. Oliveira (誰?)


!SLIDE


<pre style="font-size: 200%;">
＿人人人人人人＿
＞　型クラス　＜
￣^Y^Y^Y^Y^￣
</pre>

!SLIDE

<pre style="font-size: 200%;">
＿人人人人人人＿
＞　implicit　＜
￣^Y^Y^Y^Y^￣
</pre>

!SLIDE

一週間くらい前に、今回の内容と一部被ること話してきたので、そちらもご覧ください

[Haskell と Scala](http://xuwei-k.github.com/slides/haskell_scala)

[Start Haskell に行って、Scalaの話をしてきた](http://d.hatena.ne.jp/xuwei/20121013/1350134161)

!SLIDE

1 簡単な例(ord)を出して、Scalaでの型クラスを説明

!SLIDE

論文の最初にでてくる例

型クラスの定義

```scala
trait Ord [T] {
  def compare (a :T,b :T):Boolean
}
```

!SLIDE

型クラスのインスタンスの定義

```scala
implicit object intOrd extends Ord [Int] {
  def compare (a :Int,b :Int):Boolean = a < b
}
```

!SLIDE

使い方

```scala
scala> sort (List (3,2,1))
List (1,2,3)
```

!SLIDE

* このくらいは既になんとなく知ってますよね？
* まさに、上記のようなものが[標準ライブラリに入ってます](https://github.com/scala/scala/blob/v2.9.2/src/library/scala/collection/SeqLike.scala#L615-639)

!SLIDE

2 Scalaと対比させつつ、Haskellでの型クラスの説明

!SLIDE

Multiple parameter type class

```haskell

class Coerce a b where
  coerce :: a -> b

instance Coerce Char Int where
  coerce = ord

instance Coerce Float Int where
  coerce = floor
```

!SLIDE

Overlapping instance

```haskell

instance Ord a -> Ord [a] where ...

instance Ord [Int] where ...

```

!SLIDE

3 Scalaにおけるimplicit(parameterとconversion両方)の詳細な説明

* 初歩的なimplicit自体の文法の説明
* 単にデフォルト値的な使い方できるよね
* implicit が、scopeを持てる、それによる優先度について
* この章で [Pimp my Library](http://www.artima.com/weblogs/viewpost.jsp?thread=179766) について軽く説明あるのだけど、なぜここででてくるのかいまいち流れがわからない・・・

!SLIDE

4 Concept Pattern

!SLIDE

Concept Patternの利点として以下の点があるらしい

1. Retroactive modeling
2. Multiple method
3. Binary(or n-ary)methods
4. Factory methods

!SLIDE

JavaやC#のようなオブジェクト指向言語では以下の様にするが、これとの違いは？

```scala

trait Ord[T]{
  def compare(x:T):Boolean
}

class Apple(x:Int) extends Ord[Apple]
```

!SLIDE

```scala
a = new Apple(3);

a.compare(new Apple(5));
```

!SLIDE

```scala
a = new Apple(3);

ordApple.compare(a,new Apple(5));
```

!SLIDE

* C#の拡張メソッド
* Haskellのtype class
* [JavaGI](http://www.stefanwehr.de/javagi/)
* C++0x の concepts

!SLIDE

JavaGIの論文など

* [JavaGI: Generalized Interfaces for Java](http://www.stefanwehr.de/publications/ecoop2007-javagi.pdf)
* [On the Decidability of Subtyping with Bounded Existential Types](http://www.stefanwehr.de/publications/Wehr_On_the_decidability_of_subtyping_with_bounded_existential_types.pdf)
* [JavaGI in the Battleﬁeld: Practical Experience with Generalized Interfaces](http://www.stefanwehr.de/publications/Wehr_JavaGI_in_the_battlefield.pdf)
* [Subtyping Existential Types](http://www.stefanwehr.de/publications/Wehr_Subtyping_existential_types.pdf)

!SLIDE

JavaGIではこんな感じで型クラスのインスタンス定義?

```java
implimentation Iterable<Character> [String] {
  public Iterator<Character> iterator() {
    return new Iterator<Character>() {
      private int index = 0;
      public boolean hasNext() { return index < length(); }
      public Character next() { return charAt(index++); }
    };
  }
}
```

!SLIDE

それらでも、同じようなことを表現できるが、ScalaのImplicitを使ったアプローチは

*This make the pattern very natural to use without an additional, pattern-specific, language construct.*

であるという主張

!SLIDE

### 5 以下の様な、実際によくある様な例をだして、Type classでのアプローチと、オブジェクト指向でのアプローチの比較。(また、すべての例について、Haskellのコードもあって、Haskellとの比較も？)

<br />

1. Ordering concept
2. Abstract data types
3. Statically-typed printf
4. Type class programs are OO programs

!SLIDE

 5.1 さんざん例でつかってきた Ord について

!SLIDE

 5.2 以下の様な例を使って、ADTとtype classについての考察

```scala

trait Set[S]{
  val empty:S
  def insert(x:S,y:Int):S
  def contains(x:S,y:Int):Boolean
  def union(x:S,y:S):S
}
```

!SLIDE

```scala

trait ListSet extends Set[List[Int]]{
  val empty = List()
  def insert(x:List[Int],y:Int) = y :: x
  def contains(x:List[Int],y:Int) = x.contains(y)
  def union(x:List[Int],y:List[Int]) = x.union(y)
}
```

!SLIDE

```scala

trait FunctionalSet extends Set[Int => Boolean]{
  val empty = (x:Int) => false
  def insert(f:Int => Boolean,y:Int) =
    z => y.equal(z) | f(z)
  def contains(f:Int => Boolean,y:Int) = f(y)
  def union(f:Int => Boolean,g:Int => Boolean) =
    y => f(y) | g(y)
}
```

!SLIDE

```scala

val setImpl1 = new ListSet()
val setImpl2 = new FunctionalSet()
def test1[S](s:Set[S]):Boolean =
  x.contains(s.insert(s.insert(s.empty),5),6),6)
```

!SLIDE

6 は更に高度な話で、ここをちゃんと説明するのは無理ぽ・・・

* まず、HaskellでのAssociated typeの説明
* 次に、Scalaでtype member + dependet method type を使って、Associated typeをどのように表現するか？という話
* session type
* Arity polymorphic ZipWith in Scala
* ZipWith using prioritised overlapping implicits
* encoding generalized constraints
    * [Predefにある <:<](https://github.com/scala/scala/blob/v2.9.2/src/library/scala/Predef.scala#L412)
* Type theories using implicits
    * [CanBuildFrom](http://scalajp.github.com/scala-collections-impl-doc-ja/) の話とか

!SLIDE

```scala
trait ZipWith[S] {
  type ZipWithType
  def manyApp : Stream[S] => ZipWithType
  def zipWith : S => ZipWithType =
    f => manyApp (repeat (f))
}

class ZipWithDefault f
  implicit def ZeroZW [S] = new ZipWith[S] f
    type ZipWithType = Stream[S]
    def manyApp = xs => xs
  }
}

```

!SLIDE

```scala

object ZipWith extends ZipWithDefault f
  def apply [S] (s: S) (implicit zw:ZipWith[S])
    :zw.ZipWithType = zw.zipWith (s)
  implicit def SuccZW [S,R](implicit zw:ZipWith[R])
    = new ZipWith[S => R] {
    type ZipWithType = Stream[S] => zw:ZipWithType
    def manyApp = xs => ss =>
      zw:manyApp (zapp (xs,ss))
  }
}
```

!SLIDE

## 7 Discussion and Related Work

* 7.1 Real-world applications
* 7.2 Type classes, JavaGI and concepts
* 7.3 Generic programming in the large

!SLIDE

7 にでてくる表

![figure12](scalabase/figure12.png)

!SLIDE

参照されている論文など(一部)

* [A comparison of C++ concepts and Haskell type classes](http://sms.cs.chalmers.se/publications/papers/2008-WGP.pdf)
* [Making implicit parameters explicit. Technical report, Institute of Information and Computing Sciences](http://www.cs.uu.nl/research/techreps/UU-CS-2005-032.html)
* [Fun with type functions](http://research.microsoft.com/en-us/um/people/simonpj/papers/assoc-types/)
* [Pimp my Library](http://www.artima.com/weblogs/viewpost.jsp?thread=179766)
* [Objects to unify type classes and GADTs](http://www.comlab.ox.ac.uk/people/Bruno.Oliveira/objects.pdf)
* [A language for generic programming in the large. Science of Computer Programming, In Press, Corrected Proof](http://www.sciencedirect.com/science/article/B6V17-4TJ6F7D-1/2/7d624b842e8dd84e792995d3422aee21)


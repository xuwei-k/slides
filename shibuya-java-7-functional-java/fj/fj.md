!SLIDE

# Functional Javaの紹介

<https://github.com/functionaljava/functionaljava>

<br /><br />

<a style="font-size: 10%" rel="license" href="http://creativecommons.org/licenses/by/2.1/jp/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="http://i.creativecommons.org/l/by/2.1/jp/88x31.png" /></a>


!SLIDE

<img src="https://pbs.twimg.com/profile_images/1931553270/xuwei.gif" width="100" height="100" />

- twitter [@xuwei_k](https://twitter.com/xuwei_k)
- github [@xuwei-k](https://github.com/xuwei-k)
- blog <http://d.hatena.ne.jp/xuwei/>

!SLIDE

- 2014年3月からドワンゴでScala書いてる
- [Scalaz](https://github.com/scalaz/scalaz) という、Scalaで関数型プログラミングするための怖いライブラリのコミッター
- [先日勉強会をやった](http://d.hatena.ne.jp/xuwei/20140714/1405302866)
- Scalaに必要なJavaの知識はあるけど、普段Javaほとんど書かないのでよくわかりません(>_<)

!SLIDE

## Scala祭り

<br />

- 9月6,7日
- Scala作者のOdersky先生が来ます
- <http://scalamatsuri.org/>

!SLIDE


<https://github.com/functionaljava/functionaljava/graphs/contributors>

<img src="http://xuwei-k.github.io/slides/shibuya-java-7-functional-java/contributors1.png" />

!SLIDE

<img src="http://xuwei-k.github.io/slides/shibuya-java-7-functional-java/contributors2.jpg" />

!SLIDE

# ((((；ﾟДﾟ))))  !?

!SLIDE

2014年7月19日現在

<br />

- 最新versionは4.1
- Java8に対応


!SLIDE

githubの履歴を追った限り

<br />

- 2010年くらいからgithub、それ以前はgoogle code
- google codeからの履歴引き継いでない
- なので、最初の開発開始はよくわからないが、少なくとも2009以前
- github移行後、2014年7月19日までで255コミットしかない


!SLIDE

## ビルドファイルの歴史など

!SLIDE

[github上での最初のtreeのREADME](https://github.com/functionaljava/functionaljava/tree/7d83bfbeffad523ea96)

<img src="http://xuwei-k.github.io/slides/shibuya-java-7-functional-java/old-readme.png" />

!SLIDE

[Build.hs](https://github.com/functionaljava/functionaljava/blob/7d83bfbeffad523ea963fd104d4c3c29bc37d38e/Build.hs)

<img src="http://xuwei-k.github.io/slides/shibuya-java-7-functional-java/old-build-file.png" />

!SLIDE

# ( ﾟДﾟ)・・・！？

!SLIDE

- その後sbt(Scala製ビルドツール)が数年使われる
- Java8がでる
- Java8対応して欲しい! => 原作者達はあまりやる気ない
- 新しい人が主導権握って、めでたく(?) Gradle に

!SLIDE

- しかし、今でもテストコードはScala
- しかもScalacheckという、より関数型なテストライブラリ

!SLIDE

## やっと本題。中身の紹介

!SLIDE

## 関数オブジェクト

- F1からF8まで
- Java8が出る以前から存在するのと、Java7との互換性を保つため、Java8のjava.util.function のものは使われていない


!SLIDE

## 不変データ構造

<br />
一部、不変でないデータ構造もあるが、これから挙げるものはすべて不変

!SLIDE

## 不変データ構造

<br />

- List
- Stream (ScalaのStreamと同じで、不変かつ遅延評価)
- Option
- Either

!SLIDE

## 不変データ構造

<br />

- NonEmptyList (必ず要素が1つ以上あることが保証)
- Validation (Scalazの中にあるあれ)
- Tree (rose tree)
- TreeMap (赤黒木)
- Set (赤黒木)

!SLIDE

## 不変データ構造

<br />

- FingerTree
- HList
- Zipper
- TreeZipper

!SLIDE

<h2><span style="font-size: 30%;">Javaで無理なく表現できる範囲の一部の</span><br />型クラス</h2>

<br />

- Equal
- Ord
- Semigroup
- Monoid
- Hash
- Show

!SLIDE

## 致命的欠点?

<br />
<br />

Equal, Hash, Show があるので、大半のclassで、java.lang.Objectの、equals, hashcode, toString がoverrideされてない！！！


!SLIDE

## 並列並行処理

<br />

- Actor (Scalazと同じで型つき)
- Promise

!SLIDE

## その他

<br />

- HaskellのQuickcheckの移植
- Parser
- Iteratee
- Trampoline(再帰関数でスタックを溢れさせないようにするためのもの)

!SLIDE

ところで・・・
<br /><br />

- Javaで関数型プログラミングといえばJava8のStream!
- 皆さんJava8のStream使ってますか？

!SLIDE

## Java8のStreamとの比較

<br />
<br />

<span style="font-size: 70%;">(もしくは、Java8のStreamをdisりに来ました?)</span>

!SLIDE

- Java8のStreamは不変なコレクションではない
- 同じものを2度使えない、参照透明でないメソッドがあるので、ScalaだとIteratorに近い
- 良くも悪くも、並列実行のための処理が組み込まれている

!SLIDE

- 遅延評価
- もちろん関数型の考えは取り入れられているが・・・
- それでいて、入力や出力を柔軟に制御するための仕組みはなく、すごく複雑な処理が可能なわけでもない

<br />
つまりどういうことか？

!SLIDE

これだけは今日覚えて帰ってもらいたい

!SLIDE

(良し悪しは別にして)Java8のStreamは<br /><br />
<span style="font-size: 150%;">関数型言語の一般的なコレクションとは別で色々独特過ぎる！</span>


!SLIDE

Java8のStreamのデメリット、もしくは単に好きでない特徴

<br /><br />
(もちろん、それぞれの特徴は設計上のトレードオフなので、完全にだめなわけではない)

!SLIDE

不変なコレクションでない、副作用のあるメソッドがある
<br /><br />

↓

<br /><br />
どのメソッドが副作用あるのか把握しないといけない

!SLIDE

遅延評価


<br />

- 副作用全く使わない習慣があるHaskellなどなら問題起きにくい。
- しかし慣れるまで辛い。

!SLIDE

遅延評価


<br />

- 副作用なしで書くか、副作用使うなら発生するタイミングを把握する必要
- そのために、Functional JavaやScalaでは、遅延評価なコレクションとそうでないものを明確に分けて用意

!SLIDE

なぜJava8のStreamは遅延評価か？

<br />

- 効率のため？
- たしかに、中間オブジェクト生成されないほうが効率よい
- いつでも効率が最重要なのか？

!SLIDE

- (効率はそれほど重要ではなく)手軽にmapやfilterなどができる便利な不変コレクションが欲しいときも多いのでは？
- つまり他のモダンな言語と同じように、もっと手軽にJavaを使う用途


!SLIDE


- 効率必要ないなら、遅延評価はデバッグがつらいというデメリットのほうが大きいと思う
- 遅延評価があると、無限な長さのStreamを作って、格好よく扱うというテクニックが可能だったりするが・・・


!SLIDE

## まとめ

- もしJava8のStreamに満足できなくて、更に関数型なプログラミングをJavaでやりたくなったら、怖い人達が作ってるFunctional Java使ってみよう
- 個人的意見として、Java8のStreamは他の関数型言語の似たようなものと比べて独特過ぎるので、あれが普通だと思わないように

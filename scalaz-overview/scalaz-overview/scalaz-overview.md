!SLIDE

# Scalazの歴史と概要

<br /><br />

<a style="font-size: 10%" rel="license" href="http://creativecommons.org/licenses/by/2.1/jp/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="http://i.creativecommons.org/l/by/2.1/jp/88x31.png" /></a>


!SLIDE

<img src="https://pbs.twimg.com/profile_images/1931553270/xuwei.gif" width="100" height="100" />

- twitter [@xuwei_k](https://twitter.com/xuwei_k)
- github [@xuwei-k](https://github.com/xuwei-k)
- blog <http://d.hatena.ne.jp/xuwei/>
- 2014年3月からドワンゴで働いてる(まだ4ヶ月)
- もちろん毎日仕事でScala(not Scalaz)書いてる

!SLIDE

話すこと

- Scalazとは
- 歴史やversion間の違い
- モジュール構成
- 自分とScalaz
- なぜ使うべきか？いつ使うべきか？
- Scalazを勉強する方法、そのための資料
- コミッターの紹介
- 関連ライブラリ紹介

!SLIDE

## Scalazとは？

!SLIDE

<code>|@|&#10;&lt;*&gt;&#10;&lt;**&gt;&#10;^&#10;***&#10;&amp;&amp;&amp;&#10;:-&gt;&#10;&lt;-:&#10;&gt;&gt;=&#10;|||&#10;&lt;&lt;&lt;&#10;&gt;&gt;&gt;&#10;|=&gt;&#10;&lt;=|&#10;|&gt;&#10;??&#10;|+|&#10;!&amp;&amp;&#10;--&gt;&#10;|&#10;?&#10;&lt;^&gt;&#10;!?&#10;:?&gt;&gt;&#10;&lt;&lt;?:&#10;:++&gt;&#10;:++&gt;&gt;&#10;&lt;++:&#10;&lt;===&gt;&#10;-+-&#10;=?=&#10;⊛&#10;</code>

!SLIDE

## 超基本的な情報

<br />

- リポジトリ <https://github.com/scalaz/scalaz>
- 流石に最近はググってもgithubのほうが上位に表示されるが、google codeは古いので一応注意
- scaladoc一覧 <http://scalaz.github.io/scalaz/>
- メーリングリスト <http://groups.google.com/group/scalaz>

!SLIDE

## 読み方

<br />

- すからぜっど ？
- すからじぃー ?

どっちも聞いたことある気がする。まぁどうでもいい。"すからず" ではない、と思う

!SLIDE

## 名前の由来(?)

- <https://gist.github.com/tonymorris/5367920>

!SLIDE

## 適当な要約

- 2008年、Scalaを仕事でつかうことにしたけど、ゴミみたいなライブラリしかない
- 同僚と、オープンソースで作ることに
- 最初は内部的にscalaxという名前だった
- しかし、scalaxという名前のライブラリは他に存在してた
- 見てみたけど、それもまたゴミ
- よって、scalazになった
- (結局zの意味は語られていない・・・？)

!SLIDE

githubには

"An extension to the core Scala library for functional programming"


!SLIDE

It provides purely functional data structures to complement those from the Scala standard library. It defines a set of foundational type classes (e.g. Functor, Monad) and corresponding instances for a large number of data structures.

!SLIDE

<p>Scalaはオブジェクト指向と関数型を融合させた言語</p>
<br />


だが

!SLIDE

Scalazはその<span style="font-size: 200%;">オブジェクト指向部分を基本的に全否定</span>して、Scalaにおける関数型プログラミングのみを追求

!SLIDE


「関数型プログラミング」と一言でいっても、広いので、Scala標準ライブラリで足りない(もしくは使いづらい)部分は、基本的になんでも入っていて、結果大きくなる



!SLIDE

- Scala2.11.1 の scala-library.jar は <span style="font-size: 200%;">5.3MB</span>
- scalaz-core_2.11-7.1.0-RC1.jar は <span style="font-size: 200%;">8.6MB</span>

<br />

- <http://central.maven.org/maven2/org/scalaz/scalaz-core_2.11/7.1.0-RC1/>
- <http://central.maven.org/maven2/org/scala-lang/scala-library/2.11.1/>

!SLIDE

## 歴史、versionの違いの詳細

大きく分けて以下

- 6.x 系
- 7.0.x 系
- 7.1.x 系
- 7.2.x 系

!SLIDE

## 6 とそれ以前

- <https://github.com/scalaz/scalaz/tree/v6.0.4>
- バイナリ互換という概念を保持しようという発想自体なかった
- typesafeがmigration-manegerをそもそも出していなかったので <https://github.com/typesafehub/migration-manager>
- 6系は 6.0.4 が出てからほぼ更新されていない(2012年)


!SLIDE

## 6とそれ以前

- Scala2.10までしか対応してない
- そもそもgoogle codeな時代だったり
- そもそもpull reqベースでの開発が主流でなかった時代
- Scalaz7以降と、かなり異なる部分多い

!SLIDE

## 6以前とそれ以降のコミット数比較

- v6.0.4まで 1789 commit
- v7.0.6まで 3642 commit
- v7.1.0-RC1 4761 commit


!SLIDE

### つまり、6以前にくらべて、それ以降の開発はかなり活発

- <https://github.com/scalaz/scalaz/tree/v6.0.4>
- <https://github.com/scalaz/scalaz/tree/v7.0.6>
- <https://github.com/scalaz/scalaz/tree/v7.1.0-RC1>

!SLIDE

## 6で消えたか変更されたもの

- geo 一旦消えたが別リポジトリに復活
- <https://github.com/scalaz/scalaz-geo>
- http 消えた

!SLIDE

## 7.0.x 系

- [2013年のScalaz](http://d.hatena.ne.jp/xuwei/20131226/1388027060) (自分のblog)
- 2013年4月22日に7.0.0finalリリース
- それ以降、一部の例外を除いて、バイナリ互換性を維持してマイナーリリース(つまり1年以上続いてる)
- 7.0.0 final 以降は、wikiにとても詳細にリリースノートまとまってる
- <https://github.com/scalaz/scalaz/wiki>


!SLIDE


## 7.0.x 系

- 現在7.0.6が最新
- 7.0.7も出す予定
- Scala2.9.2 から 2.11 に対応
- [Scala2.11でのvarianceに関する変更とScalaz](http://d.hatena.ne.jp/xuwei/20140123/1390473456)
- Scala 2.12 対応は未定<span style="font-size: 70%;">(簡単にできそうならやるかもしれないが、簡単にできないかもしれない)</span>


!SLIDE


## 6系と7.0.x以降の違い

- importを分けられるようになった、MAやMABがなくなり、syntaxやOpsができた、という大きな設計変更
- [半自動コード生成という謎技術](https://github.com/scalaz/scalaz/blob/v7.1.0-RC1/project/GenTypeClass.scala)
- モジュール細かく別れた
- その他数えきれないほど色々

!SLIDE


## 7.1.x系

- 7.0.0 finalが2013年4月にでたことにより、開発開始
- Scala 2.9.3 から 2.11 に対応
- Scala 2.12 対応は未定だが、7.0.xに比べればやりやすいはずなので、たぶんやる?
- 7.1.0-M1からM7まで、7つのマイルストーンと、RC1

!SLIDE


## 7.1.x系

- もうすぐfinalがでるはず
- 7.1.0 finalがでるまでは、バイナリ互換は保証されない
- 基本的に、specs2などの7.0.x系に依存してるライブラリと共存できないので注意
- 様々な機能追加はあるが、6から7の変更に比べれば、それなりに互換性ある

!SLIDE

## 7.1.x系

- 他のライブラリとの相性やバイナリ互換を考えて慎重を期すなら、まだ7.0.x使ったほうがいい
- そのあたり気にする必要ない場合や、7.1.xにしか入ってない機能を使いたい場合は、もう7.1.0使うべき
- バイナリ互換崩れないものは、7.0.xにバックポートされてる



!SLIDE


## 7.0.xから7.1.xで、今のところバックポートされてない変更点(一部)

- varianceの変更(共変や反変を使わない流れになった)
- <https://github.com/scalaz/scalaz/pull/383/files>
- abstract classのほうがバイナリ互換崩れないことがわかったので、traitから変更
- <https://github.com/scalaz/scalaz/pull/450/files>



!SLIDE


## 7.0.xから7.1.xで非推奨になったもの

- BKTree, MetricSpace, Cojoin, Each, Length, Index
- [無法者の型クラス](http://eed3si9n.com/learning-scalaz/ja/Lawless-typeclasses.html)



!SLIDE


## 7.0.xから7.1.xで、今のところバックポートされてない追加されたもの

- InvariantFunctor, Align, Inject, ISet, Endomorphic, NotNothing, Scala標準ライブラリのFutureのインスタンス

!SLIDE

## 7.2.x系

- まだ開発始まってない
- 7.1.0 の final が出れば開発開始
- Scala2.9を切り捨てる予定
- よって、value classやmacroを多用して、大きな改善が？
- IOモナドとFutureをFreeモナドベースで統合?

!SLIDE

## モジュール構成

- core
- concurrent
- effect
- iteratee
- iterv
- scalacheck-binding
- typelevel
- xml

!SLIDE


<svg width="582pt" height="287pt"
 viewBox="0.00 0.00 582.00 287.00" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 283)">
<title>dependency&#45;graph</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-283 578,-283 578,4 -4,4"/>
<!-- scalaz&#45;scalacheck&#45;binding -->
<g id="node1" class="node"><title>scalaz&#45;scalacheck&#45;binding</title>
<polygon fill="none" stroke="black" points="0.541016,-117 0.541016,-153 163.459,-153 163.459,-117 0.541016,-117"/>
<text text-anchor="start" x="8.54102" y="-131.8" font-family="Times,serif" font-weight="bold" font-size="14.00">scalaz&#45;scalacheck&#45;binding</text>
</g>
<!-- scalaz&#45;iteratee -->
<g id="node2" class="node"><title>scalaz&#45;iteratee</title>
<polygon fill="none" stroke="black" points="209.979,-196 209.979,-232 306.021,-232 306.021,-196 209.979,-196"/>
<text text-anchor="start" x="217.979" y="-210.8" font-family="Times,serif" font-weight="bold" font-size="14.00">scalaz&#45;iteratee</text>
</g>
<!-- scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;iteratee -->
<g id="edge2" class="edge"><title>scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;iteratee</title>
<path fill="none" stroke="black" d="M122.919,-153.124C148.229,-164.615 181.146,-179.561 208.031,-191.767"/>
<polygon fill="black" stroke="black" points="206.714,-195.013 217.267,-195.96 209.608,-188.639 206.714,-195.013"/>
</g>
<!-- scalaz&#45;xml -->
<g id="node3" class="node"><title>scalaz&#45;xml</title>
<polygon fill="none" stroke="black" points="366.683,-243 366.683,-279 443.317,-279 443.317,-243 366.683,-243"/>
<text text-anchor="start" x="374.683" y="-257.8" font-family="Times,serif" font-weight="bold" font-size="14.00">scalaz&#45;xml</text>
</g>
<!-- scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;xml -->
<g id="edge3" class="edge"><title>scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;xml</title>
<path fill="none" stroke="black" d="M96.5423,-153.023C116.252,-177.714 155.391,-221.243 200,-241 250.049,-263.166 313.603,-265.916 356.363,-264.579"/>
<polygon fill="black" stroke="black" points="356.645,-268.071 366.497,-264.174 356.365,-261.076 356.645,-268.071"/>
</g>
<!-- scalaz&#45;typelevel -->
<g id="node4" class="node"><title>scalaz&#45;typelevel</title>
<polygon fill="none" stroke="black" points="351.917,-94 351.917,-130 458.083,-130 458.083,-94 351.917,-94"/>
<text text-anchor="start" x="359.917" y="-108.8" font-family="Times,serif" font-weight="bold" font-size="14.00">scalaz&#45;typelevel</text>
</g>
<!-- scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;typelevel -->
<g id="edge4" class="edge"><title>scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;typelevel</title>
<path fill="none" stroke="black" d="M163.588,-129.226C218.665,-125.279 290.598,-120.125 341.381,-116.487"/>
<polygon fill="black" stroke="black" points="341.839,-119.963 351.564,-115.757 341.339,-112.981 341.839,-119.963"/>
</g>
<!-- scalaz&#45;concurrent -->
<g id="node5" class="node"><title>scalaz&#45;concurrent</title>
<polygon fill="none" stroke="black" points="200.645,-141 200.645,-177 315.355,-177 315.355,-141 200.645,-141"/>
<text text-anchor="start" x="208.645" y="-155.8" font-family="Times,serif" font-weight="bold" font-size="14.00">scalaz&#45;concurrent</text>
</g>
<!-- scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;concurrent -->
<g id="edge5" class="edge"><title>scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;concurrent</title>
<path fill="none" stroke="black" d="M163.633,-146.122C172.578,-147.356 181.614,-148.602 190.359,-149.808"/>
<polygon fill="black" stroke="black" points="190.083,-153.303 200.468,-151.202 191.04,-146.369 190.083,-153.303"/>
</g>
<!-- scalaz&#45;core -->
<g id="node7" class="node"><title>scalaz&#45;core</title>
<polygon fill="none" stroke="black" points="494.527,-141 494.527,-177 573.473,-177 573.473,-141 494.527,-141"/>
<text text-anchor="start" x="502.527" y="-155.8" font-family="Times,serif" font-weight="bold" font-size="14.00">scalaz&#45;core</text>
</g>
<!-- scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;core -->
<g id="edge10" class="edge"><title>scalaz&#45;scalacheck&#45;binding&#45;&gt;scalaz&#45;core</title>
<path fill="none" stroke="black" d="M122.735,-116.952C193.092,-87.7205 343.003,-37.8181 458,-85 481.56,-94.6663 501.676,-115.704 515.126,-132.887"/>
<polygon fill="black" stroke="black" points="512.374,-135.052 521.179,-140.945 517.971,-130.847 512.374,-135.052"/>
</g>
<!-- scalacheck -->
<g id="node8" class="node"><title>scalacheck</title>
<polygon fill="none" stroke="black" points="219.689,-0 219.689,-36 296.311,-36 296.311,-0 219.689,-0"/>
<text text-anchor="middle" x="258" y="-13.8" font-family="Times,serif" font-size="14.00">scalacheck</text>
</g>
<!-- scalaz&#45;scalacheck&#45;binding&#45;&gt;scalacheck -->
<g id="edge1" class="edge"><title>scalaz&#45;scalacheck&#45;binding&#45;&gt;scalacheck</title>
<path fill="none" stroke="black" d="M106.722,-116.923C130.027,-99.4155 166.818,-72.4441 200,-51 204.983,-47.7795 210.289,-44.5087 215.58,-41.3403"/>
<polygon fill="black" stroke="black" points="217.577,-44.2259 224.414,-36.1319 214.022,-38.1961 217.577,-44.2259"/>
</g>
<!-- scalaz&#45;effect -->
<g id="node6" class="node"><title>scalaz&#45;effect</title>
<polygon fill="none" stroke="black" points="361.768,-188 361.768,-224 448.232,-224 448.232,-188 361.768,-188"/>
<text text-anchor="start" x="369.768" y="-202.8" font-family="Times,serif" font-weight="bold" font-size="14.00">scalaz&#45;effect</text>
</g>
<!-- scalaz&#45;iteratee&#45;&gt;scalaz&#45;effect -->
<g id="edge6" class="edge"><title>scalaz&#45;iteratee&#45;&gt;scalaz&#45;effect</title>
<path fill="none" stroke="black" d="M306.077,-211.403C320.59,-210.602 336.659,-209.715 351.512,-208.896"/>
<polygon fill="black" stroke="black" points="351.775,-212.387 361.567,-208.341 351.389,-205.397 351.775,-212.387"/>
</g>
<!-- scalaz&#45;xml&#45;&gt;scalaz&#45;core -->
<g id="edge9" class="edge"><title>scalaz&#45;xml&#45;&gt;scalaz&#45;core</title>
<path fill="none" stroke="black" d="M442.893,-242.842C448.114,-239.787 453.299,-236.473 458,-233 477.168,-218.838 496.252,-199.924 510.372,-184.763"/>
<polygon fill="black" stroke="black" points="513.106,-186.96 517.278,-177.221 507.943,-182.233 513.106,-186.96"/>
</g>
<!-- scalaz&#45;typelevel&#45;&gt;scalaz&#45;core -->
<g id="edge8" class="edge"><title>scalaz&#45;typelevel&#45;&gt;scalaz&#45;core</title>
<path fill="none" stroke="black" d="M454.738,-130.037C464.601,-133.687 474.967,-137.523 484.811,-141.166"/>
<polygon fill="black" stroke="black" points="483.85,-144.543 494.443,-144.731 486.28,-137.978 483.85,-144.543"/>
</g>
<!-- scalaz&#45;concurrent&#45;&gt;scalaz&#45;effect -->
<g id="edge7" class="edge"><title>scalaz&#45;concurrent&#45;&gt;scalaz&#45;effect</title>
<path fill="none" stroke="black" d="M314.646,-177.037C326.751,-180.961 339.519,-185.099 351.497,-188.982"/>
<polygon fill="black" stroke="black" points="350.749,-192.418 361.341,-192.172 352.907,-185.759 350.749,-192.418"/>
</g>
<!-- scalaz&#45;concurrent&#45;&gt;scalaz&#45;core -->
<g id="edge11" class="edge"><title>scalaz&#45;concurrent&#45;&gt;scalaz&#45;core</title>
<path fill="none" stroke="black" d="M315.441,-159C365.106,-159 436.677,-159 483.986,-159"/>
<polygon fill="black" stroke="black" points="484.283,-162.5 494.283,-159 484.283,-155.5 484.283,-162.5"/>
</g>
<!-- scalaz&#45;effect&#45;&gt;scalaz&#45;core -->
<g id="edge12" class="edge"><title>scalaz&#45;effect&#45;&gt;scalaz&#45;core</title>
<path fill="none" stroke="black" d="M448.289,-190.35C460.018,-186.009 472.838,-181.265 484.873,-176.811"/>
<polygon fill="black" stroke="black" points="486.158,-180.067 494.322,-173.314 483.729,-173.502 486.158,-180.067"/>
</g>
</g>
</svg>

!SLIDE

## core

- <https://github.com/scalaz/scalaz/tree/v7.1.0-RC1/core/src/main/scala/scalaz/>
- Scala標準ライブラリ以外に依存ライブラリなし
- 他のモジュールはすべて必ずこれに依存してる
- 数多くのデータ構造や型クラス入ってる

!SLIDE

## core

- 一番利用者多いのも明らかにこれ(おそらく9割以上)
- 単に"Scalaz"といった場合に、coreを指すこともあるくらい？
- 基本的にこれを使いこなすことから始めて、他のモジュールは気にしなくてもよい、くらいに中身濃い

!SLIDE

## core

- 大きすぎるのでモジュール分けたい話がでたが、(デメリット多すぎで)現実的じゃないという結論になり、大きいまま
- 型クラスは @gakuzzzz さんが話してくれるはず
- データ構造は @pocketberserker さんが話してくれるはず

!SLIDE

<svg width="988pt" height="490pt"
 viewBox="0.00 0.00 988.00 490.00" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 486)">
<title>class&#45;diagram</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-486 984,-486 984,4 -4,4"/>
<!-- scalaz.Applicative -->
<g id="node1" class="node"><title>scalaz.Applicative</title>
<polygon fill="none" stroke="black" points="292.498,-223 292.498,-259 411.502,-259 411.502,-223 292.498,-223"/>
<text text-anchor="middle" x="352" y="-236.8" font-family="Times,serif" font-size="14.00">scalaz.Applicative</text>
</g>
<!-- scalaz.Apply -->
<g id="node3" class="node"><title>scalaz.Apply</title>
<polygon fill="none" stroke="black" points="307.653,-149 307.653,-185 396.347,-185 396.347,-149 307.653,-149"/>
<text text-anchor="middle" x="352" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Apply</text>
</g>
<!-- scalaz.Applicative&#45;&gt;scalaz.Apply -->
<g id="edge1" class="edge"><title>scalaz.Applicative&#45;&gt;scalaz.Apply</title>
<path fill="none" stroke="black" d="M352,-222.937C352,-214.807 352,-204.876 352,-195.705"/>
<polygon fill="black" stroke="black" points="355.5,-195.441 352,-185.441 348.5,-195.441 355.5,-195.441"/>
</g>
<!-- scalaz.ApplicativePlus -->
<g id="node2" class="node"><title>scalaz.ApplicativePlus</title>
<polygon fill="none" stroke="black" points="273.436,-297 273.436,-333 416.564,-333 416.564,-297 273.436,-297"/>
<text text-anchor="middle" x="345" y="-310.8" font-family="Times,serif" font-size="14.00">scalaz.ApplicativePlus</text>
</g>
<!-- scalaz.ApplicativePlus&#45;&gt;scalaz.Applicative -->
<g id="edge2" class="edge"><title>scalaz.ApplicativePlus&#45;&gt;scalaz.Applicative</title>
<path fill="none" stroke="black" d="M346.659,-296.937C347.458,-288.719 348.436,-278.66 349.336,-269.406"/>
<polygon fill="black" stroke="black" points="352.82,-269.732 350.304,-259.441 345.853,-269.055 352.82,-269.732"/>
</g>
<!-- scalaz.PlusEmpty -->
<g id="node22" class="node"><title>scalaz.PlusEmpty</title>
<polygon fill="none" stroke="black" points="153.426,-223 153.426,-259 268.574,-259 268.574,-223 153.426,-223"/>
<text text-anchor="middle" x="211" y="-236.8" font-family="Times,serif" font-size="14.00">scalaz.PlusEmpty</text>
</g>
<!-- scalaz.ApplicativePlus&#45;&gt;scalaz.PlusEmpty -->
<g id="edge3" class="edge"><title>scalaz.ApplicativePlus&#45;&gt;scalaz.PlusEmpty</title>
<path fill="none" stroke="black" d="M313.244,-296.937C294.937,-287.101 271.723,-274.627 252.043,-264.053"/>
<polygon fill="black" stroke="black" points="253.458,-260.84 242.993,-259.19 250.145,-267.006 253.458,-260.84"/>
</g>
<!-- scalaz.Functor -->
<g id="node10" class="node"><title>scalaz.Functor</title>
<polygon fill="none" stroke="black" points="303.377,-75 303.377,-111 400.623,-111 400.623,-75 303.377,-75"/>
<text text-anchor="middle" x="352" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Functor</text>
</g>
<!-- scalaz.Apply&#45;&gt;scalaz.Functor -->
<g id="edge4" class="edge"><title>scalaz.Apply&#45;&gt;scalaz.Functor</title>
<path fill="none" stroke="black" d="M352,-148.937C352,-140.807 352,-130.876 352,-121.705"/>
<polygon fill="black" stroke="black" points="355.5,-121.441 352,-111.441 348.5,-121.441 355.5,-121.441"/>
</g>
<!-- scalaz.Bind -->
<g id="node4" class="node"><title>scalaz.Bind</title>
<polygon fill="none" stroke="black" points="431.54,-223 431.54,-259 512.46,-259 512.46,-223 431.54,-223"/>
<text text-anchor="middle" x="472" y="-236.8" font-family="Times,serif" font-size="14.00">scalaz.Bind</text>
</g>
<!-- scalaz.Bind&#45;&gt;scalaz.Apply -->
<g id="edge5" class="edge"><title>scalaz.Bind&#45;&gt;scalaz.Apply</title>
<path fill="none" stroke="black" d="M443.562,-222.937C427.39,-213.234 406.943,-200.966 389.475,-190.485"/>
<polygon fill="black" stroke="black" points="391.026,-187.334 380.65,-185.19 387.424,-193.336 391.026,-187.334"/>
</g>
<!-- scalaz.Cobind -->
<g id="node5" class="node"><title>scalaz.Cobind</title>
<polygon fill="none" stroke="black" points="45.5396,-149 45.5396,-185 140.46,-185 140.46,-149 45.5396,-149"/>
<text text-anchor="middle" x="93" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Cobind</text>
</g>
<!-- scalaz.Cobind&#45;&gt;scalaz.Functor -->
<g id="edge6" class="edge"><title>scalaz.Cobind&#45;&gt;scalaz.Functor</title>
<path fill="none" stroke="black" d="M140.505,-152.794C183.498,-140.842 246.974,-123.196 293.163,-110.356"/>
<polygon fill="black" stroke="black" points="294.34,-113.662 303.037,-107.611 292.465,-106.918 294.34,-113.662"/>
</g>
<!-- scalaz.Comonad -->
<g id="node6" class="node"><title>scalaz.Comonad</title>
<polygon fill="none" stroke="black" points="23.9326,-223 23.9326,-259 132.067,-259 132.067,-223 23.9326,-223"/>
<text text-anchor="middle" x="78" y="-236.8" font-family="Times,serif" font-size="14.00">scalaz.Comonad</text>
</g>
<!-- scalaz.Comonad&#45;&gt;scalaz.Cobind -->
<g id="edge7" class="edge"><title>scalaz.Comonad&#45;&gt;scalaz.Cobind</title>
<path fill="none" stroke="black" d="M81.5548,-222.937C83.2669,-214.719 85.3625,-204.66 87.2904,-195.406"/>
<polygon fill="black" stroke="black" points="90.7533,-195.944 89.3665,-185.441 83.9005,-194.517 90.7533,-195.944"/>
</g>
<!-- scalaz.ComonadStore -->
<g id="node7" class="node"><title>scalaz.ComonadStore</title>
<polygon fill="none" stroke="black" points="-0.843262,-297 -0.843262,-333 136.843,-333 136.843,-297 -0.843262,-297"/>
<text text-anchor="middle" x="68" y="-310.8" font-family="Times,serif" font-size="14.00">scalaz.ComonadStore</text>
</g>
<!-- scalaz.ComonadStore&#45;&gt;scalaz.Comonad -->
<g id="edge8" class="edge"><title>scalaz.ComonadStore&#45;&gt;scalaz.Comonad</title>
<path fill="none" stroke="black" d="M70.3699,-296.937C71.5113,-288.719 72.9083,-278.66 74.1936,-269.406"/>
<polygon fill="black" stroke="black" points="77.6686,-269.827 75.5777,-259.441 70.7352,-268.864 77.6686,-269.827"/>
</g>
<!-- scalaz.Foldable -->
<g id="node8" class="node"><title>scalaz.Foldable</title>
<polygon fill="none" stroke="black" points="477.657,-75 477.657,-111 580.343,-111 580.343,-75 477.657,-75"/>
<text text-anchor="middle" x="529" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Foldable</text>
</g>
<!-- scalaz.Foldable1 -->
<g id="node9" class="node"><title>scalaz.Foldable1</title>
<polygon fill="none" stroke="black" points="534.157,-149 534.157,-185 643.843,-185 643.843,-149 534.157,-149"/>
<text text-anchor="middle" x="589" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Foldable1</text>
</g>
<!-- scalaz.Foldable1&#45;&gt;scalaz.Foldable -->
<g id="edge9" class="edge"><title>scalaz.Foldable1&#45;&gt;scalaz.Foldable</title>
<path fill="none" stroke="black" d="M574.781,-148.937C567.417,-140.1 558.279,-129.135 550.109,-119.331"/>
<polygon fill="black" stroke="black" points="552.625,-116.882 543.534,-111.441 547.247,-121.364 552.625,-116.882"/>
</g>
<!-- scalaz.InvariantFunctor -->
<g id="node11" class="node"><title>scalaz.InvariantFunctor</title>
<polygon fill="none" stroke="black" points="278.112,-1 278.112,-37 425.888,-37 425.888,-1 278.112,-1"/>
<text text-anchor="middle" x="352" y="-14.8" font-family="Times,serif" font-size="14.00">scalaz.InvariantFunctor</text>
</g>
<!-- scalaz.Functor&#45;&gt;scalaz.InvariantFunctor -->
<g id="edge10" class="edge"><title>scalaz.Functor&#45;&gt;scalaz.InvariantFunctor</title>
<path fill="none" stroke="black" d="M352,-74.937C352,-66.8072 352,-56.8761 352,-47.7047"/>
<polygon fill="black" stroke="black" points="355.5,-47.4406 352,-37.4407 348.5,-47.4407 355.5,-47.4406"/>
</g>
<!-- scalaz.IsEmpty -->
<g id="node12" class="node"><title>scalaz.IsEmpty</title>
<polygon fill="none" stroke="black" points="154.433,-297 154.433,-333 255.567,-333 255.567,-297 154.433,-297"/>
<text text-anchor="middle" x="205" y="-310.8" font-family="Times,serif" font-size="14.00">scalaz.IsEmpty</text>
</g>
<!-- scalaz.IsEmpty&#45;&gt;scalaz.PlusEmpty -->
<g id="edge11" class="edge"><title>scalaz.IsEmpty&#45;&gt;scalaz.PlusEmpty</title>
<path fill="none" stroke="black" d="M206.422,-296.937C207.099,-288.807 207.927,-278.876 208.691,-269.705"/>
<polygon fill="black" stroke="black" points="212.204,-269.697 209.547,-259.441 205.228,-269.115 212.204,-269.697"/>
</g>
<!-- scalaz.Monad -->
<g id="node13" class="node"><title>scalaz.Monad</title>
<polygon fill="none" stroke="black" points="434.322,-297 434.322,-333 527.678,-333 527.678,-297 434.322,-297"/>
<text text-anchor="middle" x="481" y="-310.8" font-family="Times,serif" font-size="14.00">scalaz.Monad</text>
</g>
<!-- scalaz.Monad&#45;&gt;scalaz.Applicative -->
<g id="edge12" class="edge"><title>scalaz.Monad&#45;&gt;scalaz.Applicative</title>
<path fill="none" stroke="black" d="M450.429,-296.937C432.885,-287.145 410.659,-274.74 391.769,-264.197"/>
<polygon fill="black" stroke="black" points="393.237,-261.008 382.799,-259.19 389.825,-267.12 393.237,-261.008"/>
</g>
<!-- scalaz.Monad&#45;&gt;scalaz.Bind -->
<g id="edge13" class="edge"><title>scalaz.Monad&#45;&gt;scalaz.Bind</title>
<path fill="none" stroke="black" d="M478.867,-296.937C477.84,-288.719 476.583,-278.66 475.426,-269.406"/>
<polygon fill="black" stroke="black" points="478.893,-268.929 474.18,-259.441 471.948,-269.798 478.893,-268.929"/>
</g>
<!-- scalaz.MonadError -->
<g id="node14" class="node"><title>scalaz.MonadError</title>
<polygon fill="none" stroke="black" points="857.553,-371 857.553,-407 980.447,-407 980.447,-371 857.553,-371"/>
<text text-anchor="middle" x="919" y="-384.8" font-family="Times,serif" font-size="14.00">scalaz.MonadError</text>
</g>
<!-- scalaz.MonadError&#45;&gt;scalaz.Monad -->
<g id="edge14" class="edge"><title>scalaz.MonadError&#45;&gt;scalaz.Monad</title>
<path fill="none" stroke="black" d="M857.514,-372.003C854.303,-371.296 851.117,-370.623 848,-370 739.201,-348.263 610.632,-331.327 538.174,-322.575"/>
<polygon fill="black" stroke="black" points="538.245,-319.058 527.9,-321.343 537.412,-326.008 538.245,-319.058"/>
</g>
<!-- scalaz.MonadListen -->
<g id="node15" class="node"><title>scalaz.MonadListen</title>
<polygon fill="none" stroke="black" points="552.826,-445 552.826,-481 681.174,-481 681.174,-445 552.826,-445"/>
<text text-anchor="middle" x="617" y="-458.8" font-family="Times,serif" font-size="14.00">scalaz.MonadListen</text>
</g>
<!-- scalaz.MonadTell -->
<g id="node19" class="node"><title>scalaz.MonadTell</title>
<polygon fill="none" stroke="black" points="559.539,-371 559.539,-407 674.461,-407 674.461,-371 559.539,-371"/>
<text text-anchor="middle" x="617" y="-384.8" font-family="Times,serif" font-size="14.00">scalaz.MonadTell</text>
</g>
<!-- scalaz.MonadListen&#45;&gt;scalaz.MonadTell -->
<g id="edge15" class="edge"><title>scalaz.MonadListen&#45;&gt;scalaz.MonadTell</title>
<path fill="none" stroke="black" d="M617,-444.937C617,-436.807 617,-426.876 617,-417.705"/>
<polygon fill="black" stroke="black" points="620.5,-417.441 617,-407.441 613.5,-417.441 620.5,-417.441"/>
</g>
<!-- scalaz.MonadPlus -->
<g id="node16" class="node"><title>scalaz.MonadPlus</title>
<polygon fill="none" stroke="black" points="134.26,-371 134.26,-407 251.74,-407 251.74,-371 134.26,-371"/>
<text text-anchor="middle" x="193" y="-384.8" font-family="Times,serif" font-size="14.00">scalaz.MonadPlus</text>
</g>
<!-- scalaz.MonadPlus&#45;&gt;scalaz.ApplicativePlus -->
<g id="edge16" class="edge"><title>scalaz.MonadPlus&#45;&gt;scalaz.ApplicativePlus</title>
<path fill="none" stroke="black" d="M229.022,-370.937C250.241,-360.886 277.272,-348.082 299.899,-337.364"/>
<polygon fill="black" stroke="black" points="301.435,-340.509 308.974,-333.065 298.438,-334.183 301.435,-340.509"/>
</g>
<!-- scalaz.MonadPlus&#45;&gt;scalaz.Monad -->
<g id="edge17" class="edge"><title>scalaz.MonadPlus&#45;&gt;scalaz.Monad</title>
<path fill="none" stroke="black" d="M251.933,-372.236C254.996,-371.468 258.033,-370.718 261,-370 330.556,-353.168 351.27,-353.39 424.368,-334.043"/>
<polygon fill="black" stroke="black" points="425.289,-337.42 434.043,-331.45 423.477,-330.658 425.289,-337.42"/>
</g>
<!-- scalaz.MonadReader -->
<g id="node17" class="node"><title>scalaz.MonadReader</title>
<polygon fill="none" stroke="black" points="269.501,-371 269.501,-407 402.499,-407 402.499,-371 269.501,-371"/>
<text text-anchor="middle" x="336" y="-384.8" font-family="Times,serif" font-size="14.00">scalaz.MonadReader</text>
</g>
<!-- scalaz.MonadReader&#45;&gt;scalaz.Monad -->
<g id="edge18" class="edge"><title>scalaz.MonadReader&#45;&gt;scalaz.Monad</title>
<path fill="none" stroke="black" d="M370.363,-370.937C390.425,-360.975 415.933,-348.309 437.399,-337.65"/>
<polygon fill="black" stroke="black" points="439.233,-340.647 446.633,-333.065 436.12,-334.378 439.233,-340.647"/>
</g>
<!-- scalaz.MonadState -->
<g id="node18" class="node"><title>scalaz.MonadState</title>
<polygon fill="none" stroke="black" points="420.326,-371 420.326,-407 541.674,-407 541.674,-371 420.326,-371"/>
<text text-anchor="middle" x="481" y="-384.8" font-family="Times,serif" font-size="14.00">scalaz.MonadState</text>
</g>
<!-- scalaz.MonadState&#45;&gt;scalaz.Monad -->
<g id="edge19" class="edge"><title>scalaz.MonadState&#45;&gt;scalaz.Monad</title>
<path fill="none" stroke="black" d="M481,-370.937C481,-362.807 481,-352.876 481,-343.705"/>
<polygon fill="black" stroke="black" points="484.5,-343.441 481,-333.441 477.5,-343.441 484.5,-343.441"/>
</g>
<!-- scalaz.MonadTell&#45;&gt;scalaz.Monad -->
<g id="edge20" class="edge"><title>scalaz.MonadTell&#45;&gt;scalaz.Monad</title>
<path fill="none" stroke="black" d="M584.77,-370.937C566.106,-361.056 542.416,-348.514 522.385,-337.91"/>
<polygon fill="black" stroke="black" points="523.946,-334.776 513.47,-333.19 520.67,-340.962 523.946,-334.776"/>
</g>
<!-- scalaz.Nondeterminism -->
<g id="node20" class="node"><title>scalaz.Nondeterminism</title>
<polygon fill="none" stroke="black" points="692.105,-371 692.105,-407 839.895,-407 839.895,-371 692.105,-371"/>
<text text-anchor="middle" x="766" y="-384.8" font-family="Times,serif" font-size="14.00">scalaz.Nondeterminism</text>
</g>
<!-- scalaz.Nondeterminism&#45;&gt;scalaz.Monad -->
<g id="edge21" class="edge"><title>scalaz.Nondeterminism&#45;&gt;scalaz.Monad</title>
<path fill="none" stroke="black" d="M698.459,-370.937C649.539,-358.578 584.143,-342.057 537.88,-330.37"/>
<polygon fill="black" stroke="black" points="538.572,-326.935 528.02,-327.879 536.858,-333.721 538.572,-326.935"/>
</g>
<!-- scalaz.Plus -->
<g id="node21" class="node"><title>scalaz.Plus</title>
<polygon fill="none" stroke="black" points="172.091,-149 172.091,-185 249.909,-185 249.909,-149 172.091,-149"/>
<text text-anchor="middle" x="211" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Plus</text>
</g>
<!-- scalaz.PlusEmpty&#45;&gt;scalaz.Plus -->
<g id="edge22" class="edge"><title>scalaz.PlusEmpty&#45;&gt;scalaz.Plus</title>
<path fill="none" stroke="black" d="M211,-222.937C211,-214.807 211,-204.876 211,-195.705"/>
<polygon fill="black" stroke="black" points="214.5,-195.441 211,-185.441 207.5,-195.441 214.5,-195.441"/>
</g>
<!-- scalaz.Traverse -->
<g id="node23" class="node"><title>scalaz.Traverse</title>
<polygon fill="none" stroke="black" points="413.913,-149 413.913,-185 516.087,-185 516.087,-149 413.913,-149"/>
<text text-anchor="middle" x="465" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Traverse</text>
</g>
<!-- scalaz.Traverse&#45;&gt;scalaz.Foldable -->
<g id="edge23" class="edge"><title>scalaz.Traverse&#45;&gt;scalaz.Foldable</title>
<path fill="none" stroke="black" d="M480.167,-148.937C488.101,-140.012 497.964,-128.916 506.745,-119.037"/>
<polygon fill="black" stroke="black" points="509.469,-121.24 513.497,-111.441 504.238,-116.59 509.469,-121.24"/>
</g>
<!-- scalaz.Traverse&#45;&gt;scalaz.Functor -->
<g id="edge24" class="edge"><title>scalaz.Traverse&#45;&gt;scalaz.Functor</title>
<path fill="none" stroke="black" d="M438.221,-148.937C423.132,-139.323 404.09,-127.19 387.742,-116.774"/>
<polygon fill="black" stroke="black" points="389.293,-113.612 378.979,-111.19 385.532,-119.515 389.293,-113.612"/>
</g>
<!-- scalaz.Traverse1 -->
<g id="node24" class="node"><title>scalaz.Traverse1</title>
<polygon fill="none" stroke="black" points="533.413,-223 533.413,-259 642.587,-259 642.587,-223 533.413,-223"/>
<text text-anchor="middle" x="588" y="-236.8" font-family="Times,serif" font-size="14.00">scalaz.Traverse1</text>
</g>
<!-- scalaz.Traverse1&#45;&gt;scalaz.Foldable1 -->
<g id="edge25" class="edge"><title>scalaz.Traverse1&#45;&gt;scalaz.Foldable1</title>
<path fill="none" stroke="black" d="M588.237,-222.937C588.35,-214.807 588.488,-204.876 588.615,-195.705"/>
<polygon fill="black" stroke="black" points="592.118,-195.488 588.758,-185.441 585.119,-195.391 592.118,-195.488"/>
</g>
<!-- scalaz.Traverse1&#45;&gt;scalaz.Traverse -->
<g id="edge26" class="edge"><title>scalaz.Traverse1&#45;&gt;scalaz.Traverse</title>
<path fill="none" stroke="black" d="M558.851,-222.937C542.275,-213.234 521.316,-200.966 503.411,-190.485"/>
<polygon fill="black" stroke="black" points="504.765,-187.221 494.366,-185.19 501.228,-193.262 504.765,-187.221"/>
</g>
</g>
</svg>


!SLIDE


<svg width="703pt" height="194pt"
 viewBox="0.00 0.00 703.00 194.00" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 190)">
<title>class&#45;diagram</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-190 699,-190 699,4 -4,4"/>
<!-- scalaz.Arrow -->
<g id="node1" class="node"><title>scalaz.Arrow</title>
<polygon fill="none" stroke="black" points="133.881,-149 133.881,-185 224.119,-185 224.119,-149 133.881,-149"/>
<text text-anchor="middle" x="179" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Arrow</text>
</g>
<!-- scalaz.Category -->
<g id="node5" class="node"><title>scalaz.Category</title>
<polygon fill="none" stroke="black" points="-0.505371,-75 -0.505371,-111 104.505,-111 104.505,-75 -0.505371,-75"/>
<text text-anchor="middle" x="52" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Category</text>
</g>
<!-- scalaz.Arrow&#45;&gt;scalaz.Category -->
<g id="edge1" class="edge"><title>scalaz.Arrow&#45;&gt;scalaz.Category</title>
<path fill="none" stroke="black" d="M148.903,-148.937C131.631,-139.145 109.75,-126.74 91.1526,-116.197"/>
<polygon fill="black" stroke="black" points="92.7467,-113.077 82.3213,-111.19 89.2944,-119.167 92.7467,-113.077"/>
</g>
<!-- scalaz.Profunctor -->
<g id="node11" class="node"><title>scalaz.Profunctor</title>
<polygon fill="none" stroke="black" points="122.215,-75 122.215,-111 235.785,-111 235.785,-75 122.215,-75"/>
<text text-anchor="middle" x="179" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Profunctor</text>
</g>
<!-- scalaz.Arrow&#45;&gt;scalaz.Profunctor -->
<g id="edge2" class="edge"><title>scalaz.Arrow&#45;&gt;scalaz.Profunctor</title>
<path fill="none" stroke="black" d="M179,-148.937C179,-140.807 179,-130.876 179,-121.705"/>
<polygon fill="black" stroke="black" points="182.5,-121.441 179,-111.441 175.5,-121.441 182.5,-121.441"/>
</g>
<!-- scalaz.Split -->
<g id="node12" class="node"><title>scalaz.Split</title>
<polygon fill="none" stroke="black" points="253.926,-75 253.926,-111 334.074,-111 334.074,-75 253.926,-75"/>
<text text-anchor="middle" x="294" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Split</text>
</g>
<!-- scalaz.Arrow&#45;&gt;scalaz.Split -->
<g id="edge3" class="edge"><title>scalaz.Arrow&#45;&gt;scalaz.Split</title>
<path fill="none" stroke="black" d="M206.253,-148.937C221.609,-139.323 240.988,-127.19 257.625,-116.774"/>
<polygon fill="black" stroke="black" points="259.925,-119.463 266.544,-111.19 256.211,-113.53 259.925,-119.463"/>
</g>
<!-- scalaz.Bifoldable -->
<g id="node2" class="node"><title>scalaz.Bifoldable</title>
<polygon fill="none" stroke="black" points="351.605,-75 351.605,-111 464.395,-111 464.395,-75 351.605,-75"/>
<text text-anchor="middle" x="408" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Bifoldable</text>
</g>
<!-- scalaz.Bifunctor -->
<g id="node3" class="node"><title>scalaz.Bifunctor</title>
<polygon fill="none" stroke="black" points="482.326,-75 482.326,-111 589.674,-111 589.674,-75 482.326,-75"/>
<text text-anchor="middle" x="536" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Bifunctor</text>
</g>
<!-- scalaz.Bitraverse -->
<g id="node4" class="node"><title>scalaz.Bitraverse</title>
<polygon fill="none" stroke="black" points="415.388,-149 415.388,-185 526.612,-185 526.612,-149 415.388,-149"/>
<text text-anchor="middle" x="471" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Bitraverse</text>
</g>
<!-- scalaz.Bitraverse&#45;&gt;scalaz.Bifoldable -->
<g id="edge4" class="edge"><title>scalaz.Bitraverse&#45;&gt;scalaz.Bifoldable</title>
<path fill="none" stroke="black" d="M456.07,-148.937C448.26,-140.012 438.551,-128.916 429.908,-119.037"/>
<polygon fill="black" stroke="black" points="432.48,-116.662 423.261,-111.441 427.212,-121.271 432.48,-116.662"/>
</g>
<!-- scalaz.Bitraverse&#45;&gt;scalaz.Bifunctor -->
<g id="edge5" class="edge"><title>scalaz.Bitraverse&#45;&gt;scalaz.Bifunctor</title>
<path fill="none" stroke="black" d="M486.404,-148.937C494.461,-140.012 504.479,-128.916 513.397,-119.037"/>
<polygon fill="black" stroke="black" points="516.152,-121.209 520.255,-111.441 510.956,-116.518 516.152,-121.209"/>
</g>
<!-- scalaz.Compose -->
<g id="node7" class="node"><title>scalaz.Compose</title>
<polygon fill="none" stroke="black" points="119.708,-1 119.708,-37 226.292,-37 226.292,-1 119.708,-1"/>
<text text-anchor="middle" x="173" y="-14.8" font-family="Times,serif" font-size="14.00">scalaz.Compose</text>
</g>
<!-- scalaz.Category&#45;&gt;scalaz.Compose -->
<g id="edge6" class="edge"><title>scalaz.Category&#45;&gt;scalaz.Compose</title>
<path fill="none" stroke="black" d="M80.6753,-74.937C96.9815,-65.2341 117.6,-52.9655 135.213,-42.4848"/>
<polygon fill="black" stroke="black" points="137.307,-45.3115 144.111,-37.19 133.728,-39.2959 137.307,-45.3115"/>
</g>
<!-- scalaz.Choice -->
<g id="node6" class="node"><title>scalaz.Choice</title>
<polygon fill="none" stroke="black" points="5.32568,-149 5.32568,-185 98.6743,-185 98.6743,-149 5.32568,-149"/>
<text text-anchor="middle" x="52" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Choice</text>
</g>
<!-- scalaz.Choice&#45;&gt;scalaz.Category -->
<g id="edge7" class="edge"><title>scalaz.Choice&#45;&gt;scalaz.Category</title>
<path fill="none" stroke="black" d="M52,-148.937C52,-140.807 52,-130.876 52,-121.705"/>
<polygon fill="black" stroke="black" points="55.5001,-121.441 52,-111.441 48.5001,-121.441 55.5001,-121.441"/>
</g>
<!-- scalaz.Enum -->
<g id="node8" class="node"><title>scalaz.Enum</title>
<polygon fill="none" stroke="black" points="607.433,-149 607.433,-185 694.567,-185 694.567,-149 607.433,-149"/>
<text text-anchor="middle" x="651" y="-162.8" font-family="Times,serif" font-size="14.00">scalaz.Enum</text>
</g>
<!-- scalaz.Order -->
<g id="node10" class="node"><title>scalaz.Order</title>
<polygon fill="none" stroke="black" points="607.829,-75 607.829,-111 694.171,-111 694.171,-75 607.829,-75"/>
<text text-anchor="middle" x="651" y="-88.8" font-family="Times,serif" font-size="14.00">scalaz.Order</text>
</g>
<!-- scalaz.Enum&#45;&gt;scalaz.Order -->
<g id="edge8" class="edge"><title>scalaz.Enum&#45;&gt;scalaz.Order</title>
<path fill="none" stroke="black" d="M651,-148.937C651,-140.807 651,-130.876 651,-121.705"/>
<polygon fill="black" stroke="black" points="654.5,-121.441 651,-111.441 647.5,-121.441 654.5,-121.441"/>
</g>
<!-- scalaz.Equal -->
<g id="node9" class="node"><title>scalaz.Equal</title>
<polygon fill="none" stroke="black" points="607.826,-1 607.826,-37 694.174,-37 694.174,-1 607.826,-1"/>
<text text-anchor="middle" x="651" y="-14.8" font-family="Times,serif" font-size="14.00">scalaz.Equal</text>
</g>
<!-- scalaz.Order&#45;&gt;scalaz.Equal -->
<g id="edge9" class="edge"><title>scalaz.Order&#45;&gt;scalaz.Equal</title>
<path fill="none" stroke="black" d="M651,-74.937C651,-66.8072 651,-56.8761 651,-47.7047"/>
<polygon fill="black" stroke="black" points="654.5,-47.4406 651,-37.4407 647.5,-47.4407 654.5,-47.4406"/>
</g>
<!-- scalaz.Split&#45;&gt;scalaz.Compose -->
<g id="edge10" class="edge"><title>scalaz.Split&#45;&gt;scalaz.Compose</title>
<path fill="none" stroke="black" d="M265.325,-74.937C249.018,-65.2341 228.4,-52.9655 210.787,-42.4848"/>
<polygon fill="black" stroke="black" points="212.272,-39.2959 201.889,-37.19 208.693,-45.3115 212.272,-39.2959"/>
</g>
</g>
</svg>



!SLIDE


## effect

- <https://github.com/scalaz/scalaz/tree/v7.1.0-RC1/effect/src/main/scala/scalaz/effect>
- coreのみに依存
- IOモナド、STモナドなど

!SLIDE

## concurrent

- effectに依存
- <https://github.com/scalaz/scalaz/tree/v7.1.0-RC1/concurrent/src/main/scala/scalaz/concurrent>
- 型がつくActor, Future, MVar, Chan など

!SLIDE

## iterv

- 古いIteratee
- effectに依存
- 350行程度のファイル1つのみ
- <https://github.com/scalaz/scalaz/blob/v7.0.6/iterv/src/main/scala/scalaz/Iteratee.scala>
- オワコン
- 7.0.xまでしか存在せず、7.1.xには無い

!SLIDE

## iteratee

- effectに依存
- <https://github.com/scalaz/scalaz/tree/v7.1.0-RC1/iteratee/src/main/scala/scalaz/iteratee>
- itervに比べれば新しいIteratee
- だがこれももうすぐオワコン？
- scalaz-streamに置き換えたいらしい

!SLIDE

## iteratee

- play2のiterateeよりジェネリックな感じ(play2はFuture固定)
- それほど実用してるの見かけない
- パフォーマンス出るのかどうかも謎?
- メンテも、少し放置され気味

!SLIDE

## scalacheck-binding

- 他の全部のモジュールに依存
- <http://scalacheck.org/>
- 唯一、本体のリポジトリのモジュールでは、外部のライブラリであるScalacheckに依存
- Scalazのテストは、[他のScalaライブラリにはない面白い仕組みがある](http://d.hatena.ne.jp/xuwei/20121222/1356203049)


!SLIDE

## typelevel

- coreのみに依存
- <https://github.com/scalaz/scalaz/tree/v7.1.0-RC1/typelevel/src/main/scala/scalaz/typelevel>
- HList、型レベル自然数、など
- オワコンになってしまった
- shapelessと機能が被るから、という理由

!SLIDE

## xml

- <https://github.com/scalaz/scalaz/tree/v7.1.0-RC1/xml/src/main/scala/scalaz/xml>
- coreのみに依存
- 純粋関数型なスタイルでxmlをparseするためのもの
- Haskellのポーティング
- あまり使われてない？
- パフォーマンスでるのかどうか謎
- 将来的に本体のリポジトリから外す(geoのような扱いにする)ことが検討されてる

!SLIDE

自分とScalaz

- 2013年6月からコミッター
- 2013年のコミット数1位
- 2014年もコミット数だと今のところ1位
- (細かい変更が多いので)変更行数だと、総合5位くらい？
- 総合のコミット数2位
- <https://github.com/scalaz/scalaz/graphs/contributors>

!SLIDE

<blockquote class="twitter-tweet" lang="en"><p>「<a href="https://twitter.com/xuwei_k">@xuwei_k</a> は普通に読んでても気付かないような細かいバグに気付いてる。どうやっているんだ?」みたいなことを Scalaz の他のコミッタが nescala 後の懇親会で言ってた。あと「彼は nescala に来るべきだった」</p>&mdash; eugene yokota (@eed3si9n_ja) <a href="https://twitter.com/eed3si9n_ja/statuses/443248298926043136">March 11, 2014</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

!SLIDE

## Scalazコミッタになる方法

- 無職になる
- 暇なので、iPadにソースコード入れるなどして、ひたすらコードを隅々まで読む(現実逃避)
- 細かいバグもしくは改善点見つかる
- pull reqする
- 繰り返し

!SLIDE


## Scalazコミッタになる方法

- そのうちコミット権限貰える
- ✌('ω'✌ )三✌('ω')✌三( ✌'ω')✌


!SLIDE

## なぜ使うべき？
## いつ使うべきか？

!SLIDE

## いつ使うべきか？

- Scala標準ライブラリでは(関数型プログラミングするのに)物足りないと感じたとき
- チーム開発で使うなら、ある程度メンバーがついてこれそうなとき

!SLIDE

## 使わない方がいいか、必ずしも使う必要ないとき

- Scalaの基本的な言語仕様自体や、標準ライブラリの使い方もわかってない場合
- Scala標準ライブラリで事足りるかどうかもわかってないとき
- Optionに少し便利メソッドを追加する程度の使い方(今後それ以外の部分も使っていく予定ならともかく、本当にそれだけのために使うなら微妙)

!SLIDE

## 初心者におおすすめなところ

- [Either](https://github.com/scalaz/scalaz/blob/v7.1.0-RC1/core/src/main/scala/scalaz/Either.scala)
- [Validation](https://github.com/scalaz/scalaz/blob/v7.1.0-RC1/core/src/main/scala/scalaz/Validation.scala) (@pab_tech さんがあとで発表してくれるはず)
- (Scala標準ライブラリの) [Futureのインスタンス](https://github.com/scalaz/scalaz/blob/v7.1.0-RC1/core/src/main/scala/scalaz/std/Future.scala)
- Monoid
- NonEmptyList
- Foldable

!SLIDE

## 勉強方法

- コード読もう
- Haskell勉強しよう？
- 圏論はあったら役に立つかもしれないけど、必須じゃない(自分も圏論わかりません)
- [Functional Programming in Scala](http://www.manning.com/bjarnason/) Scalazの本ではないが役に立つ
- [独習Scalaz](http://eed3si9n.com/learning-scalaz/ja/) 網羅してるのはごく一部だったり、説明や例が簡素なことが多いので注意


!SLIDE


## 主要なコミッタの紹介


!SLIDE

## @retronym

- <https://github.com/retronym>
- コミット数1位
- Scalaのコミッターでもある
- Scalaz7のプロトタイプを、ほぼ一人で作った
- IntelliJ IDEAのScalaのプラグインもコミッタ

!SLIDE


## @tonymorris

- <https://github.com/tonymorris>
- コミット数3位
- Scalazの原作者
- specs2作者の同僚？


!SLIDE


## @larsrh

- <https://github.com/larsrh>
- 最近のリリース関連全部やってる
- wikiのリリースノートもほぼ全部書いてる
- バイナリ互換の方針とか、開発方針とか中心になってすすめてくれてる

!SLIDE


## @S11001001

- <https://github.com/S11001001>
- @ekmettの同僚(金融系？)
- Bazaarというマイナーな(?)バージョン管理システムが好きらしい
- <https://launchpad.net/~scompall>


!SLIDE


## @runarorama

- <https://github.com/runarorama>
- @ekmettの元同僚
- Freeモナド詳しい
- Functional Programming in Scalaという本書いてる
- <http://www.manning.com/bjarnason/>


!SLIDE


## @pchiusano
- <https://github.com/pchiusano>
- scalaz-stream作者
- Functional Programming in Scalaという本書いてる
- <http://www.manning.com/bjarnason/>


!SLIDE


## @puffnfresh
- <https://github.com/puffnfresh>
- RoyというAltJSの作者
- pure scriptのコミッター？


!SLIDE


## @markhibberd
- <https://github.com/markhibberd>
- argonautのコミット数1位
- tonymorrisさんの同僚

!SLIDE


## @pthariensflame
- pipeというHasekellのライブラリをScalaに移植してた(メンテされてない)
- コミット数は別に多くない
- [agdaのコード投げてきた人](https://github.com/scalaz/scalaz/issues/601#issuecomment-30088659)


!SLIDE


## 関連ライブラリ紹介

- [githubをScalazで検索した結果](https://github.com/search?q=%22org.scalaz%22+%25%25+%22scalaz-core%22+7.0&type=Code&ref=searchresults) 1000件以上？


!SLIDE

## <https://github.com/scodec/scodec>

- シリアライズ、デシリアライズ
- scalaz-streamで使われている

!SLIDE

## <https://github.com/scalaz/scalaz-stream>

- scalazコミッタの人が作ってる
- iterateeなどの代替となるはずもの
- 開発活発だし、パフォーマンス重視してて実用的？


!SLIDE

## <https://github.com/julien-truffaut/Monocle>

- 別のLens実装
- Scalaz本体にもLensあるのに・・・
- 最近argonautがこれ使うようになった

!SLIDE

## <https://github.com/puffnfresh/scala-lenz>

- 別のLens実装その2
- みんななぜそんなにLensライブラリ作るのか


!SLIDE

## <https://github.com/argonaut-io/argonaut>

- Jsonライブラリ
- 一応、自分もコミッタ
- それなりな速度でるらしいし、結構実用的かも？


!SLIDE

## <https://github.com/etorreborre/specs2>

- みんな大好き(?) specs2
- かなり有名なテストライブラリの一つ
- 昔からscalaz6を内部に取り込んで使ってた
- salaz 7.0.0 finalでたあとは、直接依存
- よって現状 scalaz 7.1.x と混ぜて使えないので注意

!SLIDE

## <https://github.com/milessabin/shapeless>

- scalazに依存してるわけではない！
- 一部機能かぶってる(tagged type, Lens, HList など)
- scalazよりある意味ヤバイ
- <https://github.com/typelevel/shapeless-contrib>
- akka-streamやplay2のanormのテストに使われ始めるなど、実用されてる

!SLIDE

## <https://github.com/non/spire>

- scalazに依存してるわけではない！
- これも一部機能かぶってる(Monoidなど)

!SLIDE

## <https://github.com/runarorama/scala-machines>

- scalaz-streamと似たなにか
- オワコンなのか、メンテされるのか謎

!SLIDE

## <https://github.com/scalaz/scalaz-geo>

- 一度削除されたが、別リポジトリで復活した
- 地理関係のなにか
- あまり使ってる人多くないと思う

!SLIDE

## <https://github.com/arjanblokzijl/scala-conduits>

- conduitsの移植
- 全然メンテされてない

!SLIDE

## <https://github.com/http4s/http4s>

- scalaz-stream使ってる
- Scalatra3(まだ出てない)は、これ使うようになるらしい

!SLIDE

## <https://github.com/scalatra/scalatra>

- commandsのモジュールでscalazが使われている

!SLIDE

## <https://github.com/pthariensflame/scala-pipes>
- Haskellにpipesというライブラリがあるのだが、それの移植
- あまりメンテされてない

!SLIDE

## <https://github.com/xuwei-k/play2scalaz>

- 自作
- play2とscalazの型クラスを相互変換するなど

!SLIDE

## <https://github.com/xuwei-k/applybuilder>
- 自作その2
- scalaz本体のApplyのsyntaxが12までしか使えなかったり、推論が残念なのを補うためのもの


!SLIDE

## <https://github.com/xuwei-k/iarray>
- 自作その3
- scalaz本体のImmutable Arrayが使いづらいので自作


!SLIDE

おわり？質問タイム?

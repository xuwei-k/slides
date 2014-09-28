!SLIDE

# Akka Stream and Playframework

<br /><br />

<a style="font-size: 10%" rel="license" href="http://creativecommons.org/licenses/by/2.1/jp/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="http://i.creativecommons.org/l/by/2.1/jp/88x31.png" /></a>

!SLIDE

<img src="https://pbs.twimg.com/profile_images/1931553270/xuwei.gif" width="100" height="100" />

- twitter [@xuwei_k](https://twitter.com/xuwei_k)
- github [@xuwei-k](https://github.com/xuwei-k)
- blog <http://d.hatena.ne.jp/xuwei/>
- Scalaz commiter

!SLIDE

- 3月からDwangoでScala書いてる(7ヶ月くらい)
- 最近はakkaとakka-remoteを少し仕事で使ってる
- そのうち発表したい・・・

!SLIDE

### 前提知識

<br />

- 現時点(2014/9/28)で
- のplayframeworkの最新安定版は2.3.4
- 2.4.0-M1がでている

!SLIDE

### 現状のPlay2.3

<br />

- Nettyに依存
- 無理やりservlet上で動かすものもある <https://github.com/play2war/play2-war-plugin> が、あくまでも無理やり

!SLIDE

## "Split Netty server code into separate subproject"

<br />

- <https://github.com/playframework/playframework/commit/674aff609a04cb701857a6367f>

<br />

akka-stream, netty以外の実装も書きやすくなるかもしれない？
(例えばservlet)



!SLIDE

## "Experimental Akka HTTP server backend and Reactive Streams integration"

<br />


- <https://github.com/playframework/playframework/pull/3332>

!SLIDE

- play2.4では、akka-streamが使えるようになる
- 今までのNettyがあくまでもデフォルト

!SLIDE

### 現時点のplay2.4のSNAPSHOTでakka-streamを使う方法

<br />

まずplayをpublishLocal

<br />

```
sbt \
  -Dplay.version=2.4-20140928 \
  -Dscala.version=2.11.2 publishLocal
```

!SLIDE

<https://github.com/playframework/playframework/blob/0d4c9b73396d57/documentation/manual/experimental/AkkaHttpServer.md>

<br />

```scala
libraryDependencies +=
  "com.typesafe.play" %% "play-akka-http-server-experimental" % "%PLAY_VERSION%"
```

!SLIDE

```
run -Dserver.provider=play.core.server.akkahttp.AkkaHttpServerProvider
```

<br />

(今後変更の可能性あり)


!SLIDE

- play2.4の次は、2.5ではなくplay3？
- と、以前は言っていた気がしたけど、Roadmap見たら2.5も出すらしい？
- <https://docs.google.com/document/d/11sVi1-REAIDFVHvwBrfRt1uXkBzROHQYgmcZNGJtDnA/pub>

!SLIDE

### ベンチマークしてみた

<br />

- <https://github.com/xuwei-k/akka-stream-vs-netty>
- 最新のplay2.4をビルドしてpublishLocal(2.4.0-M1にはまだakka-streamのコミット入ってないので)
- 数MBのデータを送信 => そのまま返す
- それを100回くらい繰り返し
- 並列にリクエストするのと、1回ずつリクエストするのをやった

!SLIDE

### 結果

<br />

- 別にあまり差がでなかった・・・
- ベンチマークの方法が悪い？もっと色々な方法試す必要あり？
- とりあえず、明らかに速くなったり遅くなったりすることはなさそう
- お客様の中に誰かベンチマーク得意な方いらっしゃいませんか・・

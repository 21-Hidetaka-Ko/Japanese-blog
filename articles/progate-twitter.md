世界一わかりやすいと話題のプログラミング学習サイトProgateについて「だれが一番ツイートしているか」をRのフロントエンドExploratoryを使って分析してみた


##1. Twitterのデータをインポート

外部のデータをインポートしてきたいので、まず、Import Remote Dataを選びます。
![](images/twitter-import1.png)

Twitterを選びます。

![](images/twitter-import2.png)

連携を許可します。

![](images/twitter-import3.png)

##2. だれが一番ツイートしているか？





##3. なんで１週間分のツイートしか取得できないの？

![](images/.png)

Progateとツイートされた日付を表すcreated_atカラムを見るとわかるように、1週間分のツイートしか取得できていません。
これは、Twitterの検索APIの仕様の問題でそうなっているようです。もしどうしても、すべてのツイートを読み込みたい場合は、Twitterはstreaming apiってのが提供しているはずなので、それをdbに入れておいてそれを解析するか、seleniumとか使ってすべてスクレイプするしかなさそうです。めんどくさそうですね笑。

##4. なんでTwitterのアナリティクス機能を使った分析はできないの？

![](images/twitter-analytics.png)

現在はまだ実装されていませんが、将来的にサポートする予定です。

##5. 余談

余談ですが、ぼくは、Progateで５ヶ月間ほどソフトウェアエンジニアとして有給インターンしていました。本当にたくさんのことを学ばせて頂きました。詳しく知りたい方は、[文系初心者が東大理系エンジニア集団Progateで、エンジニアとしてインターンするまで気付かなかった「１人・教材・海外留学」より大切なこと](http://storys.jp/story/20856?to=story&referral=pc_story_sidebar&context=author_other
)をどうぞ。何と、１万人以上の人に読まれました。

![](images/progate-story.jp.png)

##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。

チュートリアルは[こちら](http://docs.exploratory.io/tutorials/intro.html
)から見ることができます。

英語が読める方は[Introducing Exploratory Desktop — UI for R](https://blog.exploratory.io/introducing-exploratory-desktop-ui-for-r-895d94ef3b7b#.4dncgv1rt
)もどうぞ

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ


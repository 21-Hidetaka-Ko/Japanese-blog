
##はじめに




##問題点



##〜形でスクレイピングする

 '# Custom R function as Data.
sample_2_pages.func <- function(){
loadNamespace("httr")
loadNamespace("rvest")
loadNamespace("dplyr")

res_list <- lapply(seq(4), function(i){
  httr::GET("http://search.e-gov.go.jp/servlet/Public", query=list(CLASSNAME="PCMMSTLIST", Mode=2, Page=i-1))
})

tables <- lapply(res_list, function(res){
  main <- rvest::html_node(httr::content(res, encoding = "Shift_JIS"), "#main")
  children <- rvest::html_children(main)
  result_list <- list()
  current_tag <- ""
  for(child in children){
    tag <- rvest::html_tag(child)
    if(tag == "h2"){
      current_tag <- rvest::html_text(child)
    } else if (tag=="table"){
      caption <- rvest::html_text(rvest::html_node(child, "caption"))
      table <- rvest::html_table(child, fill = TRUE)
      result_list[[length(result_list)+1]] <- dplyr::mutate(table, label=current_tag, caption=caption)
    }
  }
  do.call(rbind, result_list)
})

do.call(rbind, tables)

}'

##summarize関数を使う


フライト全体の平均遅延到着時間を見たくありませんか？　そういうときは、summarize関数の、summarizeコマンドを使うと見ることができます。

ARR_DELAY列のヘッダーをクリックしてfilterコマンドを選びます。

![ARR_DELAY-header.png](https://qiita-image-store.s3.amazonaws.com/0/91522/df398510-30ab-c3ff-d849-f0acdc2b1b74.png)


![ARR_DELAY-header2.png](https://qiita-image-store.s3.amazonaws.com/0/91522/530c9b12-905a-f5d4-72cb-71f918894d15.png)


すると、自動的にsummarize(ARR_DELAY_mean = mean(ARR_DELAY, na.rm = TRUE))が入力されます。Runボタンを押します。

引数の‘na.rm = TRUE’は、‘mean()’関数が計算の前にNA値を除去するかどうかを設定します。もし、TRUEではなくFALSEだったら、データにNA値があるときは、NA値を返します。Rは統計学者がよく使うプログラミング言語なので、とても強力で正確です。

![ARR_DELAY_mean.png](https://qiita-image-store.s3.amazonaws.com/0/91522/172db2ef-732f-eb68-c76f-a4d24dd9cc05.png)


1.5337分だとわかりましたね。すると、次は、到着遅延時間が1.5337分以上のフライトだけをfilter()関数を使って見てみましょう。

ARR_DELAY列のヘッダーをクリックしてfilterコマンドを選びます。

![ARR_DELAY-header.png](https://qiita-image-store.s3.amazonaws.com/0/91522/626c26c2-4256-01c1-e254-03890b0c0ed0.png)


1.5337分以上なので、>を使います。

![ARR_DELAY-header3.png](https://qiita-image-store.s3.amazonaws.com/0/91522/70ed4087-8f7e-aa33-b247-157791827eee.png)


そして、先ほどのmean(ARR_DELAY, na.rm = TRUE))を加え、filter(ARR_DELAY > mean(ARR_DELAY, na.rm = TRUE))の状態で、Runボタンを押します。

![ARR_DELAY-header4.png](https://qiita-image-store.s3.amazonaws.com/0/91522/8bcb25b9-219d-f623-d7b7-946245f73c00.png)


ぼくは、Progate株式会社にインターンしていた当時、データ分析をするときは、SQLをよく使っていました。SQLは、とても便利でしたし、SQLを使ってデータ分析をしている会社は日本には多いと思います。SQL芸ができるエンジニアは、イケメン、すごいという風潮さえありますよね。

しかし、シリコンバレーに来てdplyrに出会って以来、dplyrの直感的で便利な文法のおかげで、全くSQLを使うことがなくなってしまいました。

例えば、「平均到着遅延時間以上のフライトだけを見る」と言った質問に対して、ぼくは、filter(ARR_DELAY > mean(ARR_DELAY, na.rm = TRUE))と書きました。これをSQLで書くと

'SELECT *:<br>
 FROM flight<br>　　
WHERE ARR_DELAY > (SELECT AVG(ARR_DELAY)<br>　　
                FROM flight　<br>　
             )'

のような感じになるはずです。ご覧のとおり、1行で収めるのは難しく、直感的ではないですよね。

SQLとdplyrには、データ分析の流れが、入れ子になるか、フローになるかという違いがあります。

もっと噛み砕いて、説明すると、dplyrは一連の関数のフローによって、tableを変形していくことが出来ます。それに対し、SQLはfunctionをかくごとにtableが変形しない。5回関数を書いたとき、dplyrなら1回ごとにtableが変形していくが、SQLは５回分を一気に一回で変形するイメージと言えば通じるでしょうか。（ただし、SQLにおけるfunctionってRの概念におけるfunctionは少し違いますが、ここでは割愛）

だから、dplyrのほうがSQLに比べ、後から見たときに、分析の流れがフローになっているためわかりやすく、再現性が高いのです。

だから、ぼくはよくQiitaでdplyrのコマンドがいかに直感的かについて説明しているんです。


##Groupでsummarize関数を使う


全体の平均遅延到着時間ではなく、フライトそれぞれの平均遅延到着時間を見てみたくありませんか？この質問に答えるためには、以下のように、filterをする以前に、CARRIERに対して、group_by()関数を加える必要があります。

CARRIER列のヘッダーをクリックしてgroup_byコマンドを選びます。

![group_by_carrier.png](https://qiita-image-store.s3.amazonaws.com/0/91522/2625ff40-50a0-591f-d6bb-f0a387ace3cf.png)


すると、自動的にgroup_by(CARRIER)が入力されます。Runボタンを押します。

![group_by-caae.png](https://qiita-image-store.s3.amazonaws.com/0/91522/424436aa-a917-26bb-237e-b979ba23ad76.png)


とてもシンプルですよね。このgroup_by()関数は、mean()のようなデータを集計する関数がそれぞれのグループに対して集計できるように、指定した列をグルーピングするんです。

次に、summarizeコマンドを使って、データを集計します。

ARR_DERAY列のヘッダーをクリックしてsummarizeコマンドを選びます。平均を出したいので、meanを選びます。

![mean-groupby-arr.png](https://qiita-image-store.s3.amazonaws.com/0/91522/685dbf39-6d06-653d-0197-0c092054d250.png)


すると、自動的にsummarize(ARR_DELAY_mean = mean(ARR_DELAY, na.rm = TRUE))が入力されます。Runボタンを押します。

![aa-average.png](https://qiita-image-store.s3.amazonaws.com/0/91522/0dc8c9ea-5ead-e686-358a-e7d925665058.png)


これで、それぞれのフライト会社における遅延時間を計算して出すことができました。これを見ると、AA会社の平均遅延時間は、1.9343分だとわかりますね。

以上が、dplyrでの基本的なフィルタリングに関する操作第2弾です。dplyrでは、他にもwindow関数を使ったり、文字列を整形したり、date関数を使ったり、まだまだいろんなことができます。そちらの方はこれから書いていく予定ですので、お楽しみに！

##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。

分析してほしいデータがある方や、データ分析のご依頼はhidetaka.koh@gmail.comまでどうぞ

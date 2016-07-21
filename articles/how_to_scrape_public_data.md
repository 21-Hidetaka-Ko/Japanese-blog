#お役所のデータで消耗するのはもうごめん。データ分析ツールExploratoryを使って、お役所の汚いデータをスクレイピングからビジュアライズまでしてみた


##はじめに

Exploratoryのユーザ様から、以下のような質問メールが届いたのが事の発端です。


ーーー

Quittaなど、どれも興味深い記事で楽しく拝見させていただいております。

Exploratoryを使っているのですが、スクレイプしたいデータの一つ一つがタイトルごとに分かれてしまいます。
スクレイプしたい段階で、一つのデータにまとめる方法はありますか。
ご回答いたただけると幸いです。

ーーー

####問題点

ユーザーが抱えてる問題をもっと理解するために、スクリーンショットとタイトルごとに分かれてしまうデータのリンクを送って頂き、自分の環境でも試してみました。

こんな感じのページで、リンクは[こちら](http://search.e-gov.go.jp/servlet/Public?CLASSNAME=PCMMSTLIST&Mode=2
)です。


![](images/public_data.png)

このページをExploratoryのScrape Web Pageからデータをインポートしてみます。

![](images/scrape_web_page.png)


すると、このような形でデータをスクレイピングしてしまいます。

![](images/problem_data.png)

見ていただけると、おわかりかもしれませんがこのままだと問題が3つあります。

- 1: テーブルがタイトルごとにわかれてしまっている
- 2: 結果の公示日を表す7月21日が行ではなく、列になっている
- 3: 外国為替、貿易／貿易　や　【案件番号：595116050】  「平成２８年度「水産物」「こんぶ」及び「ばら干しのあおのり及びひとえぐさ」の輸入割当てについて（案）」に対する意見募集の結果について　などの情報がスクレイピングできていない


Exploratoryだと、データ分析だけでなく、Import by Writing R scriptという機能を使えば、こういった複雑で汚いウェブ・ページからデータを思い通りにスクレイピングしてくることができたり、さらにはその後のデータの加工もデータ分析しながら簡単に出来るといことなので、早速やってみました。スクレイピングして、クリーン・アップした最後にはデータを簡単にビジュアライズしてみたいと思います。


##〜形でスクレイピングする

Exploratoryでは、Import by Writing R scriptというところから、自分でRのプログラムを書いていくことができます。この機能を使うと、先ほどの、titleごとに分かれてしまい思うようにスクレイピングできなかったデータも、titleごとにテーブルをわけないでデータを分けない状態で、スクレイピングしていくことができます。

![](images/excel-difficult.png)

今回、ここに書いたコードは以下です。このスクレイピングのために書いた詳しい解説は、また別の記事に書きたいと思います。

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


すると、このようにtitleごとに分かれないで、同じテーブルになってデータをスクレイピングしてくることができました。

![](images/excel-difficult.png)


##データを整形する


####不要なデータを取り除く

####gatherとspreadを駆使して、列と行を入れ替える

####日付のデータタイプをcharacterからDateに変更する

##簡単にビジュアライズする


##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。

分析してほしいデータがある方や、データ分析のご依頼はhidetaka.koh@gmail.comまでどうぞ

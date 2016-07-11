# データ分析ツールExploratoryを使って、dplyrを使いこなす！


##はじめに

この記事は、Rは知らないけど、SQLとか他のプログラミング言語はある程度やったことあるみたいな人向けです。
興味がなかったり、意味のないデータを取り除きたいと思ったことはありませんか？　データをフィルタリングすることは、データ分析をする上での基本的な操作です。dplyrには、filter()関数というのが備わっています。dplyrはデータをただ単にフィルタリングできるだけではありません。dplyrがあれば、SQLや他のBIツールのようにデータを直感的に分析していくことができます。


##dplyrとは

データフレームの操作に特化したRのパッケージです。
Rは基本的に処理速度はあまり早くないですが、dplyrはC++で書かれているのでかなり高速に動作します。

SQLと比較するとわかりやすいです。主要なdplyrの機能とSQLの対比は、下図となります。

| SQL        | dplyr       |説明　　　　　　　　　　　　　　　　　　　|
|:-----------|------------:|:---------------------------------:|　　　　　　　　　　　　　
| where      |    filter   |行の絞り込み　　　　　　　　　　　　　　　|
| group by   |    group_by |グルーピングする　　　　　　　　　　　　　|
| select     |      select |データフレームから指定した列のみ抽出する　|
| order by   |     arrange |行を並べ替える　　　　　　　　　　　　　　|
| count,max  |   summarise |集計する　　　　　　　　　　　　　　　　　|


これから、Rのフロントエンドと呼ばれているデータ分析ツールExploratoryを使いながら、dplyrについて簡単に説明していきます。データは[こちら](
)からダウンロードできます。

##Selectコマンドを使って、列を選択する

![](images/flight-dplyer.png)

Selectコマンドを使うことで、分析したい列だけを選ぶことができます。

![](images/flight-dplyer-select.png)


##知りたいデータだけをフィルタリングする


例えば、United Airline (UA)会社のフライトのデータだけを見たい場合は、下図のようにするとみることができます。

![](images/filter-flight.png)


また、andを意味する&演算子を使うと、条件を加えることができます。&を使って、United Airline (UA)会社のフライトに加え、出発地点がSan Francisco airport (SFO)のフライトのデータも見てみましょう。

![](images/filter-and.png)


もしくは、!=演算子を使って、条件を反対にすることもできます。!=を使って、出発地点は、San Francisco airport (SFO)のままだけれど、United Airline (UA)会社ではないフライトのデータを見ることもできます。


![](images/filter-!=.png)


##Filtering with multiple values


What if you want to see only the data for the flights that are of either United Airline (UA) or American Airline (AA) ? You can use ‘%in%’ for this, just like the IN operator in SQL.

![](images/.png)


We can’t really tell if it’s working or not by looking at the first 10 rows. Let’s run count() function to summarize this quickly.

![](images/.png)


We can see only AA and UA as we expected. And yes, I know, this ‘count()’ function is amazing. It literally does what you would intuitively imagine. It returns the number of the rows for each specified group, in this case that is CARRIER. We could have done this by using ‘group_by()’ and ‘summarize()’ functions, but for something like this simple ‘count()’ function alone does the job in such a quick way.


##Reverse the condition logic

What if you want to see the flight that are not United Airline (UA) and American Airline (AA) this time ? It’s actually very simple with R and dplyr. Here’s a magic one letter you can use with any condition to reverse the effect. It’s ‘!’ (exclamation mark). And, it goes like this.

![](images/.png)

Notice that there is the exclamation mark at the beginning of the condition inside the filter() function. This is a very handy ‘function’ that basically flips the effect of the condition that is after the exclamation mark. This is why the result above doesn’t include ‘UA’ nor ‘AA’. It might look a bit weird until you get used to it especially if you’re coming from outside of R world, but you are going to see this a lot and will appreciate its power and convenience.


##Filtering out NA values

Now, let’s go back to the original data again.


When you look closer you’d notice that there are some NA values in ARR_DELAY column. You can get rid of them easily with ‘is.na()’ function, which would return TRUE if the value is NA and FALSE otherwise.

![](images/.png)


Oops, it looks like all the values in ARR_DELAY are now NA, which is opposite of what I hoped. Well, as you saw already we can now try the ‘!’ (exclamation mark) function again like below.




This is how you can work with NA values in terms of filtering the data.
This is the basic of how ‘filter’ works with dplyr. But this is just the beginning. You can do a lot more by combining with aggregate, window, string/text, and date functions, which I’m going to cover at the next post. Stay tuned!


##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。


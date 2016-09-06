#なぜデータ分析においては、SQLではなくRを使うべきか？ サンフランシスコの犯罪の〜を分析してみてた

この記事は、Rは知らないけど、SQLとか他のプログラミング言語はある程度やったことあるみたいな人向けです。
SQLでデータ分析をしていて、煩わしいと感じたことはありませんか？　これは、SQLは、データを保存するために作られているので、ある意味当たり前のことなんです。しかし、データ分析において、Rをオススメしたい理由は、実はRは、データデータの加工、分析のために作られているからなんです。Rでなら、SQLを書くだけでは考えられなかったより高度なことや効果的な方法で素早くデータを分析していくことができるのです。

というわけで、今回は、ぼくの留学先であるサンフランシスコの街の2003年から2015年までの犯罪データを使って、いかにRがSQLに比べてデータ分析に向いているかを説明していきたいと思います。データは[こちら](https://data.sfgov.org/Public-Safety/SFPD-Incidents-from-1-January-2003/tmnf-yvry)からダウンロードすることができます。

##どの曜日が犯罪が多いか

まず、どの曜日が犯罪が多いかを分析していきたいと思います。

mdy関数を使って、データ・タイプをcharacterからDateに変えたいと思います。

![](images/mdy-sf.png)

`
mutate(Date = mdy(Date))
`

![](images/mdy-sf2.png)

次に、wday関数を使って、Dateから曜日を抽出したいと思います。

`
mutate(Date_wday = wday(Date, label = TRUE))
`

![](images/wday-sf.png)

「どの曜日が犯罪件数が多いのか」を計算するために、Date_wdayをグルーピングします。

`
group_by(Date_wday)
`
![](images/wday-grouping.png)

そして、n関数を使って、犯罪件数を計算します。

`
summarize(numreports = n())
`



![](images/wday-n.png)


どうやら金曜日が一番多いみたいですね。


これを、SQLで書こうとするとこうなります。どちらが直感的でシンプルかは一目瞭然ですよね。


```
select
  extract(dayofweek from date)
  , extract(hour from date)
  , count(1)
from
  sf_crime_2003_2015
group by
  1
  , 2
order by
  3
```

```
select 0, 'Sunday'
union select 1, 'Monday'
union select 2, 'Tuesday'
union select 3, 'Wednesday'
union select 4, 'Thursday'
union select 5, 'Friday'
union select 6, 'Saturday'
```

<!-- ##どの時間帯が犯罪が多いか

R

`
mutate(split_time = str_split(Time, ":"))
`

`
mutate(Hour = as.numeric(list_extract(split_time, 1)))
`

SQL

```
select
  extract(hour from date)
  , count(1)
from
  sf_crime_2003_2015
group by
  1
  , 2
order by
  3
``` -->

##最も犯罪件数が多い月日はいつか？

R

`
mutate(Date_month = month(Date))
`

`
mutate(Date_day = day(Date))
`

`
mutate(DOY = str_c(Date_month, Date_day, sep="-"))
`

`
group_by(DOY)
`

`
summarize(count = n())
`



これを、SQLで書こうとするとこうなります。とても長くて複雑ですね。なぜ、こんなに複雑になっているかというとうるう年を計算してうるう年の年とでない年で、月日の設定を分岐させているからなんです。Dateカラムのデータタイプが、characterになっているので、RでやったようにDateカラムから月日を抽出することができないのです。たとえ、データタイプがcharacterになっていても、Rならmdy関数一発で、データタイプをDateタイプに変えて月日を抽出していくことができますが、SQLだとそうもいかないのです。だから、Rはデータ分析に向いているんです。


```
with crime_by_doy as
(
select
  -- Check to see if it is a leap year
  case when (extract(year from date) % 4 = 0
        and extract(year from date) % 100 <> 0)
        or extract(year from date) % 400 = 0
         then
            case when extract(dayofyear from date) = 60
                   then 'Feb. 29'
                 when extract(dayofyear from date) > 60
                   then (extract(dayofyear from date) - 1)::text
                 else (extract(dayofyear from date))::text
            end
      else
         -- If it is not a leap year, no conversions are necessary
         (extract(dayofyear from date))::text
   end as doy,
   count(1) as num_incidents
from
   sf_crime_2003_2015
group by
   1
)
```

```
, generated_series as
 (
   select
      (getdate()::date - row_number()
        over(order by true) )::date as n
    from
      sf_crime_2003_2015
    limit 730 -- value for 2 years
 )

, doy_mapping as
(
  select
     (extract(doy FROM (n)::timestamp))::text as "doy"
     , to_char(n, 'Month DD') as "mm_dd"
  from
     generated_series
  where
     extract(year from n) = 2015
)

select
  crime_by_doy.doy
  , "mm_dd" as date
  , crime_by_doy.num_incidents
from
  crime_by_doy
  join doy_mapping on
    crime_by_doy.doy = doy_mapping.doy
order by 3 desc

```


こちらの方で、Rの中でもデータ分析に特化しているdplyrの文法について詳しく解説しているので、よかったらご覧ください。

- [なぜデータ分析においては、SQLではなくRを使うべきか？　 データ分析ツールExploratoryを使って、dplyrを使いこなす第1弾 Filter関数編](http://qiita.com/21-Hidetaka-Ko/items/117caea621562f05ffe1)

- [なぜデータ分析においては、SQLではなくRを使うべきか？　データ分析ツールExploratoryを使って、dplyrを使いこなす第2弾　データ集計編](http://qiita.com/21-Hidetaka-Ko/items/bc7766e730a60ebf4561)

- [なぜデータ分析においては、SQLではなくRを使うべきか？　 データ分析ツールExploratoryを使って、dplyrを使いこなす第3弾　Window関数編](http://qiita.com/21-Hidetaka-Ko/items/fb9fe00ce982f240829a)

- [なぜデータ分析においては、SQLではなくRを使うべきか？　 データ分析ツールExploratoryを使って、dplyrを使いこなす第4弾　date関数編](http://qiita.com/21-Hidetaka-Ko/items/1645d89683e312f4d65c)




##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。


ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

分析してほしいデータがある方や、データ分析のご依頼はhidetaka.koh@gmail.comまでどうぞ
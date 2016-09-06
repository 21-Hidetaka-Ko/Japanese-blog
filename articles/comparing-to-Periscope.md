#なぜデータ分析においては、SQLではなくRを使うべきか？ サンフランシスコの犯罪の〜を分析してみてた

R

`
mutate(Date = mdy(Date))
`

`
mutate(Date_wday = wday(Date, label = TRUE))
`

`
mutate(split_time = str_split(Time, ":"))
`

`
mutate(Hour = as.numeric(list_extract(split_time, 1)))
`

SQL


`select  <br>
  extract(dayofweek from date)
  , extract(hour from date)
  , count(1)
from
  sf_crime_2003_2015
group by
  1
  , 2
order by
  3  `

`
select 0, 'Sunday'
union select 1, 'Monday'
union select 2, 'Tuesday'
union select 3, 'Wednesday'
union select 4, 'Thursday'
union select 5, 'Friday'
union select 6, 'Saturday'
`


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

SQL

`
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
`

`
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

`


##SQL is not designed to do data analysis

This is just a tip of the iceberg. There are many other examples on why SQL is not for analyzing data. It is not SQL’s fault per se, because SQL has never claimed to be designed for analyzing the data. It was rather designed for querying and managing the data including inserting, updating, and deleting rows of data. I used to work at Oracle who had brought SQL to this world in a commercial way in the first place, and we tried so many things to make SQL look like the language that could be used even for analyzing the data. But at the end of the day, the ways to make it work within SQL always turned out to be so convoluted that many people outside of the core geeks including myself couldn’t understand anymore and even people like us had hard time to optimize the queries to perform in a reasonable time. Again, the relational database is not designed for analyzing the data, and SQL is not designed for analyzing the data either.


##R is designed for data analysis

Instead of keep talking about why SQL sucks for analyzing data, let me quickly demonstrate how great R and dplyr package are for analyzing data, not just for statisticians, but for ordinal people including SQL users, Excel users, and BI tools users in a super effective way.
Before starting, if you are not familiar with ‘dplyr’ yet, it provides a set of the data wrangling grammar based commands (or functions) and they can be flexibly used together.



##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。


ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

分析してほしいデータがある方や、データ分析のご依頼はhidetaka.koh@gmail.comまでどうぞ
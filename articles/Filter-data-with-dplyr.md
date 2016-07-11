#Filter data with dplyr


Filtering data is one of the very basic operation when you work with data. You want to remove a part of the data that is invalid or simply you’re not interested in. Or, you want to zero in on a particular part of the data you want to know more about. Of course, dplyr has ’filter()’ function to do such filtering, but there is even more. With dplyr you can do the kind of filtering, which could be hard to perform or complicated to construct with tools like SQL and traditional BI tools, in such a simple and more intuitive way.
Let’s begin with some simple ones. Again, I’ll use the same flight data I have imported in the previous post.
Select columns
First, let’s select columns that are interesting for now. If you want to know more about ‘how to select columns’ please check this post I have written before.
library(dplyr)
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME)

flight data
Filter with a value
Let’s say you want to see only the flights of United Airline (UA). You can run something like below.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(CARRIER == "UA")
If you want to use ‘equal’ operator you need to have two ‘=’ (equal sign) together like above. If you run the above you’ll see something like below.

filter with UA
And now, let’s find the flights that are of United Airline (UA) and left San Francisco airport (SFO). You can use ‘&’ operator as AND and ‘|’ operator as OR to connect multiple filter conditions. This time we’ll use ‘&’.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(CARRIER == "UA" & ORIGIN == "SFO")

flights with UA and SFO
Or, you might want to see only the flights that left San Francisco airport (SFO) but are not of United Airline (UA). You can use ‘!=’ operator as ‘not equal’.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(CARRIER != "UA" & ORIGIN == "SFO")
Filtering with multiple values
What if you want to see only the data for the flights that are of either United Airline (UA) or American Airline (AA) ? You can use ‘%in%’ for this, just like the IN operator in SQL.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(CARRIER %in% c("UA", "AA"))

flights either UA or AA
We can’t really tell if it’s working or not by looking at the first 10 rows. Let’s run count() function to summarize this quickly.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(CARRIER %in% c(“UA”, “AA”)) %>%
  count(CARRIER)

flights with count by carriers
We can see only AA and UA as we expected. And yes, I know, this ‘count()’ function is amazing. It literally does what you would intuitively imagine. It returns the number of the rows for each specified group, in this case that is CARRIER. We could have done this by using ‘group_by()’ and ‘summarize()’ functions, but for something like this simple ‘count()’ function alone does the job in such a quick way.
Reverse the condition logic
What if you want to see the flight that are not United Airline (UA) and American Airline (AA) this time ? It’s actually very simple with R and dplyr. Here’s a magic one letter you can use with any condition to reverse the effect. It’s ‘!’ (exclamation mark). And, it goes like this.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(!CARRIER %in% c("UA", "AA")) %>%
  count(CARRIER)

Notice that there is the exclamation mark at the beginning of the condition inside the filter() function. This is a very handy ‘function’ that basically flips the effect of the condition that is after the exclamation mark. This is why the result above doesn’t include ‘UA’ nor ‘AA’. It might look a bit weird until you get used to it especially if you’re coming from outside of R world, but you are going to see this a lot and will appreciate its power and convenience.
Filtering out NA values
Now, let’s go back to the original data again.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME)

When you look closer you’d notice that there are some NA values in ARR_DELAY column. You can get rid of them easily with ‘is.na()’ function, which would return TRUE if the value is NA and FALSE otherwise.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(is.na(ARR_DELAY))

flights with NA values in ARR_DELAY
Oops, it looks like all the values in ARR_DELAY are now NA, which is opposite of what I hoped. Well, as you saw already we can now try the ‘!’ (exclamation mark) function again like below.
flight %>%
  select(FL_DATE, CARRIER, ORIGIN, ORIGIN_CITY_NAME, ORIGIN_STATE_ABR, DEP_DELAY, DEP_TIME, ARR_DELAY, ARR_TIME) %>%
  filter(!is.na(ARR_DELAY))

flights with non-NA in ARR_DELAY
This is how you can work with NA values in terms of filtering the data.
This is the basic of how ‘filter’ works with dplyr. But this is just the beginning. You can do a lot more by combining with aggregate, window, string/text, and date functions, which I’m going to cover at the next post. Stay tuned!

##SQLと比較するとわかりやすい

SQLと比較するとわかりやすいです。主要なdplyrの機能とSQLの対比は、下図となります。

| SQL        | dplyr       |説明　　　　　　　　　　　　　　　　　　　|
|:-----------|------------:|:---------------------------------:|　　　　　　　　　　　　　
| where      |    filter   |行の絞り込み　　　　　　　　　　　　　　　|
| group by   |    group_by |グルーピングする　　　　　　　　　　　　　|
| select     |      select |データフレームから指定した列のみ抽出する　|
| order by   |     arrange |行を並べ替える　　　　　　　　　　　　　　|
| count,max  |   summarise |集計する　　　　　　　　　　　　　　　　　|


##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。


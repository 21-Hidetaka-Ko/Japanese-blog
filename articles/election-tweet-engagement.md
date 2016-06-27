# Explore Trump, Clinton, and Sanders Tweets data and find their engagement effectiveness

![](images/engagement_type.png)


Exploratoryのすごいところは、Rを使って、とても簡単に、様々なデータ・ソースからデータをインポートすることができることです。Twitterもその１つです。技術的には、Jeff Gentryさんが開発した[twitteR](https://github.com/geoffjentry/twitteR)というパッケージを使っています。

そういえば、日本では今年の参院選から選挙権年齢が18歳に引き下げられましたね。舛添さんも退任することになり、東京都知事を決める選挙が行われることになりましたね。

ぼくが、留学しているアメリカ大統領選の話についてはご存知でしょうか？　

ぼくは、在日韓国人なので、日本も故郷である大阪の選挙権も持ってないこともあり、日本にいた頃は、政治の話を友達としたことなど皆無に等しかったです。しかし、アメリカ人の友達やシリコンバレーのエンジニアは、政治の話が大好きです。留学当初は、政治の話になると、途端に会話のレベルやスピードがアップしてついていけなくてコンプレックスを感じていたものでした。

アメリカは民主党・共和党の二大政党制ですが、それぞれの主張や支持者層は多岐にわたります。すべての争点において「この候補者・この党の考え方に賛成する」ということは不可能ですし、よく耳にする「民主党=庶民層、共和党=富裕層」といった分類もナイーブすぎます。

アメリカでは4年に一度の大統領選挙が行われています。アメリカの大統領は国家元首であり、行政の最高責任者であり、連邦軍の総指揮官でもあります。日本の天皇と総理大臣を合わせたような存在に近いかもしれません。大統領は議会をしのぐ力を持っているわけではありませんが、大統領が変わるとアメリカが進む方向が変わるのも事実です。

大統領選は間接選挙の形を取っていますが、事実上、国民が直接選ぶ仕組みです。今年の11月に投票が行われますが、現在は二大政党の民主党と共和党が予備選挙を行っていて、それぞれの党の候補者は7月までに決まります。

さて、誰が今回の大統領選を戦っているかご存知ですか？

トランプ氏、クリントン氏、サンダース氏です。

<!-- 今日は、候補者の中でも特にふたり、トランプ氏とクリントン氏をとりあげます。 -->

トランプ氏、クリントン氏、サンダース氏の最近１０日間のツイートデータを使って、両者について分析してみたいと思います。

##Twitterのサーチデータをインポートする

まず、Remote Data SourcesからTwitterを選びます。

![](images/remote_data_sources.png)

Authorize appをクリックしてください。

![](images/authorize_twitter.png)

これが終わると、Exploratoryデスクトップアプリケーションに戻ってください。そして、検索したい単語を入力していきます。

どうやって、サーチクエリが作られているかについて知りたい方は、詳しくは、[Twitter’s Search API　](https://dev.twitter.com/rest/public/search)をどうぞ。とても強力です。

ここでは、まず最初に、トランプ氏のツイートを抜き出したいと思います。トランプ氏のアカウント名は、‘realDonaldTrump’なので、‘Search Text’パラメータに入力します。‘@’は、入力する必要はありません。このような感じです。

from:realDonaldTrump

データフレーム名を、‘twitter_trump’に設定して、‘Max # of tweets’を1000に変えます。そして、Get Dataボタンをクリックしてください。

![](images/from-realDonaldTrump.png)

もしプレビューのデータが大丈夫そうなら、データをロードするためにSaveボタンをクリックしてください。

![](images/summary_twitter_trump.png)


ただし、TwitterのサーチAPIの制限で、データはは、７日間分だけになっています。


クリントン氏も同じようにします。

`from:HillaryClinton`

![](images/from-HillaryClinton.png)

サンダース氏も同じようにします。

`from:SenSanders`

![](images/from-SenSanders.png)

##3人のデータを結びつけます。

この時点で、トランプ氏、クリントン氏、サンダース氏それぞれのデータフレームを見ることができますね？

![](images/three-dataframe_before.png)

３者のデータを比較するために、このデータを結びつけてみましょう。‘bind_rows’関数を使うと、３者のデータを結びつけることができます。

別にどのデータフレームからでも、この関数を使うことができますが、今回は、‘twitter_sanders’データフレームからしてみましょう。下図のように、ドロップダウンメニューから＋アイコンを押して、‘Bind Rows’を選んでください。

![](images/bind_rows.png)

すると、自動的に、‘bind_rows’と打ち込まれたのを確認することができます。

![](images/bind_rows2.png)

カンマで区切って、‘twitter_clinton’と‘twitter_trump’を選んでください。Runするときのコマンドはこのような感じです。

`bind_rows(twitter_clinton, twitter_trump)`

You want to select both of them separating them by comma. Here is the final command we want to run.
bind_rows(twitter_clinton, twitter_trump)
After that, we can see that three twitter account names are shown under ‘screenName’ column.
We can see that Clinton has the most tweets and Sanders has the least in the last ten days.
How are they tweeting?
We can quickly start visualizing the data by going to Chart view. First, we can assign ‘created’ column to X-Axis, change the aggregation level to ‘DAY’, and assign ‘screenName’ to Color.
We can see that Trump had increased the tweets dramatically on Jun 2nd and 3rd while Clinton had been getting more active from May 31st.
Who is getting the most social engagement?
These tweets are not just their thoughts of the day they just happened to share. These are for the US Presidiential Campaign so if you are one of the candidates you want it to be not only widely spread but also getting a lot more engagements than the other candidates. From that point of view, there are two key indicators to measure the user engagement. One is ‘favoriteCount’, this is the numbers how many people clicked the red heart button for each tweet.
And another is ‘retweetCount’, this is the number of the times people retweeted for each tweet.
By using these two metrics we can find who is succeeding the campaigns the most or getting more engagements. For example, we can assign ‘retweetCount’ to X-Axis, ‘favoriteCount’ to Y-Axis, and ‘screenName’ to Color.
We can see that Trump’s tweets tend to be more liked than retweeted in a comparison to the other two candidates while Clinton’s tweets tended to be more retweened than liked compared to the other two. However, when we look closer we notice visually that the amount of the ‘liked’ and ‘retweeted’ tweets by Trumps are significantly a lot more than the other two.
This goes to show how Trump has been successfully promoting his messages by reaching out a lot bigger audience through Twitter for the last 10 days.
We can actually see this with a simple bar chart by assigning ‘created’ date to X-Axis, ‘retweetCount’ to Y-Axis, and ‘screenName’ to Color, like below.
We can see how consistently Trump has been succeeding for spreading his tweets for the last 10 days.
Another way to look at this is to label each tweet based on whether the tweet was retweeted (or liked) more than a certain times like, for example, three thousand times.
You can create such label simply by running a command like below.
mutate(retweetCounts_threshold = retweetCount > 3000)
This will test the condition (retweetCount > 3000) and return TRUE if it satisfies the test, otherwise FALSE.
Then, we can assign ‘screenName’ to X-Axis and this new column ‘retweenCounts_threshold’ to Color like below.
Wow! This simple chart actually contrasts the difference between Trump and the other two candidates so significantly. The majority of Trump’s tweets got retweeted at least three thousand times while such tweets are rather rare rewards for Clinton and Sanders. Trump’s tweets are much more contagious.
Trend for Retweeted and Liked Counts for each Candidates
Lastly, let’s look at the trend by comparing the distribution of each ‘favoriteCount’ and ‘retweetCount’ values among the candidates.
If we want to bring the two metrics into the chart together we can gather those two columns into Key and Value columns so that the Key column will have the information to identify whether the values presented there are ‘favoriteCount’ or ‘retweetCount. We can use ‘gather’ command from dplyr package like below.
gather(engagement_type, counts, favoriteCount, retweetCount)
The first argument value is the name of the new Key column and the second is the name of the new Value column this command will create. The third and the forth are the names of the columns that we want to gather and assign them to the new Key and Value columns.
After running the command we can see the new ‘engagement_type’ and ‘counts’ columns being added.
Now we can go to Chart and quickly assign ‘engagement_type’ to X-Axis, ‘counts’ to Y-Axis, and ‘screenName’ to Color.
It excludes the outliers as default. We can include them by clicking ‘Include Outlier’ checkbox next to Y-Axis.
Either way, it looks that Trump is completely playing a different game than the other two here. This could be because the Democrats are split to two candidates currently, once they will have picked one single candidate then we might see more effective engagement for the Democrats. Or, maybe not. The Trump’s masterful capability of influencing through social media is something that has been proved through the Republican primary race already, and that is something I’m sure the strategy and PR folks at the Democrats are super worried about.
Bringing Twitter Search data quickly into Exploratory means that we can explore the data quickly and iteratively by employing our usual grammar of data wrangling — dplyr and tidyr — and the visualization to find something we didn’t even know before. And this is what thrills me the most every time I get to work with a new data set and hope it will do for you, too!
If you liked this post, please click the green heart below to share with the world!
And, if you like to try analyzing Twitter data quickly you can sign up for Exploratory Desktop Beta at our website.
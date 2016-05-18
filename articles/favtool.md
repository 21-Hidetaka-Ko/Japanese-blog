おかげさまで現在、世界中からたくさんの人たちにExploratory Desktopのベータ・トライアルの方にサインアップしていただいています。もしまだサイン・アップしてなくて興味のある方はぜひ[こちら](http://docs.exploratory.io/tutorials/flight4.html)からどうぞ
ところで、サインアップしていただく時に、皆さんの普段使っているデータ分析ツールが何か、聞かせてもらっています。そのデータがこちらにあるので、それを元にどういったツールが今世界中で人気があるか出してみたいと思いますが、実はいくつか面倒くさい問題があります。

- カンマ区切りなので、単純にツールごとに集計できない
- フリーフォームなので、スペースがいろんなとこに入ってたり、英語の大文字、小文字が混じってて、実は同じ名前であるのに同じ名前としてカウントできない。

![](images/favtool1.png)

こういったデータって結構データ分析をやっていると普通だったりするのですが、Exploratoryを使うと簡単にクリーン・アップすることができます。クリーン・アップした最後にはトップ５のデータ分析ツールを出してみたいと思います。

str_spritします。
![](images/favtool2.png)

str_spritできた。
![](images/favtool3.png)

Unnestしないと。

![](images/favtool4.png)

Unnestします。

![](images/favtool5.png)

Unnestできた。

![](images/favtool6.png)

str_titleしないと。

![](images/favtool7.png)

str_titleします。

![](images/favtool8.png)

str_titleできた。
![](images/favtool9.png)

str_trimしないと。

![](images/favtool10.png)

str_trimします。

![](images/favtool11.png)

str_trimできた。
![](images/favtool12.png)


トップ５のデータ分析ツールを出す

group_byします。

![](images/favtool13.png)

countします。

![](images/favtool14.png)

countできました。

![](images/favtool15.png)

top_nします。
![](images/favtool16.png)

top_nできました。

![](images/favtool17.png)





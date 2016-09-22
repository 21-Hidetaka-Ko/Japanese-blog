#

##はじめに

興味がなかったり、意味のないデータを取り除きたいと思ったことはありませんか？　データをフィルタリングすることは、データ分析をする上での基本的な操作です。dplyrには、filter()関数というのが備わっています。dplyrはデータをただ単にフィルタリングできるだけではありません。dplyrがあれば、SQLや他のBIツールのようにデータを直感的に分析していくことができます。

以前、[こちら](http://qiita.com/21-Hidetaka-Ko/items/117caea621562f05ffe1)の記事で、dplyrのfilter関数について説明しました。Exploratoryは、2.0から新しいUIに変わりました。これから、新バージョンのExploratoryのUIを使いながら、新しいUIがいかにdplyrを扱うのに便利がについて簡単に説明していきます。データは[こちら](https://www.dropbox.com/s/x2g3qgo28syxhcl/airline_delay_2016_01.csv?dl=0)からダウンロードできます。

##データをインポートする

まず、ここからプロジェクトを作ることができます。

![](images/create-project.png)

次に、ここからデータをインポートできます。

![](images/flight-import.png)

データをインポートすると、データのサマリー画面を見ることができます。

![](images/flight-dplyernew.png)

##Selectコマンドを使って、列を選択する

Selectコマンドを使うことで、分析したい列だけを選ぶことができたり、指定した列を取り除くことができます。Selectボタンを押してください。

![](images/select123.png)

新しいUIでは、ダイアログから、自由に列を選んでいくことができます。

![](images/select1234.png)

また、Functionを押すと、ダイアログから、Selectコマンドに関数を簡単に加えていくことができます。

![](images/select12345.png)



##最後に

Exploratoryの2.0でdplyrを扱うことがどれだけ簡単かおわかりいただけたでしょうか？　Exploratoryでは、複雑で冗長なコードを書くことなしに、文法を中心としたデータ分析とビジュアライゼーションによってこんなに簡単に、自分でデータをよりよく理解していくことができます。

##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。


ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

分析してほしいデータがある方や、データ分析のご依頼はhidetaka.koh@gmail.comまでどうぞ
#なぜデータ分析においては、SQLではなくRを使うべきか？ Google公式のGoogle BigQueryを爆速で設定する

この記事は、一度もGoogle BigQueryを触ったことがない人のための、セットアップの説明書です。Google BigQueryにアクセスするにはまず、Google　BigQueryにサインアップする必要があります。自分のクレジットカード情報を記入する必要がありますが、60日間の無料お試し期間があるので利用しましょう。限度額は、300ドルです。もし限度を超えることがあったとしても、勝手に課金される前に、YesかNoと断れるようになってます。

##プロジェクトを作成する

いったんサインアップすると、Google Cloud Consoleページからプロジェクトを作成することができます。

![](images/how_to_create_project_google.png)

ヘッダーのドロップダウンメニューからプロジェクトを作成を選んで下さい。

プロジェクト名を入力してください。

![](images/how_to_create_project_google2.png)

##データセットを作りましょう

いったんプロジェクトを作ると、左側のメニューボタンを押して下さい。

![](images/how_to_create_project_google1.png)

メニューからBigQueryを選び、Google BigQueryのUIを開きましょう。

![](images/how_to_create_project_google3.png)

矢印のアイコンの隣にあるボタンを押して、データセットを作成してください。

![](images/how_to_create_project_google4.png)

データセットIDをタイプして、OKボタンをクリックしてください。

![](images/how_to_create_project_google5.png)

これで設定は終了です。Google BigQueryのデータ分析については、こちら↓で書いているのでよろしかったらどうぞ。

[なぜデータ分析においては、SQLではなくRを使うべきか？ Google公式のGoogle BigQueryのGitHubデータを使って、Rのパッケージを爆速でクラスタリングする](https://github.com/21-Hidetaka-Ko/Japanese-blog/blob/master/articles/Google%20BigQuery-tutorial.md)


##興味を持っていただいた方、実際に触ってみたい方へ

Exploratoryは[こちら](https://exploratory.io/
)からβ版の登録ができます。こちらがinviteを完了すると、ダウンロードできるようになります。


ExploratoryのTwitterアカウントは、[こちら](https://twitter.com/ExploratoryData
)です。

Exploratoryの日本ユーザー向けの[Facebookグループ](https://www.facebook.com/groups/1087437647994959/members/
)を作ったのでよろしかったらどうぞ

分析してほしいデータがある方や、データ分析のご依頼はhidetaka.koh@gmail.comまでどうぞ
OpenAMのアップグレードはとても簡単です。ただし、バージョンの組み合わせによっては、アップグレードがうまくできなかったり、手作業で一部の設定を変更しなければならないこともあります。アップグレードする前に、対象バージョンのForgeRock公式ドキュメントを参照し、追加の手順や制限事項などが無いか確認しておいて下さい。

### アップグレードの概要
アップグレード手順は、以下のようになります。

1. Javaコンテナを停止
2. warファイルを更新し、展開済みのディレクトリを削除
3. Javaコンテナを起動
4. OpenAMにアクセスすると、アップグレード画面が表示されるので、アップグレードを実施
　ただし、アップグレードに失敗したり、アップグレード後の動作に問題がある可能性もあるので、バックアップを取得しておくことをお勧めします。バックアップするのは以下のディレクトリになります。

/path/to/tomcat/webapps/openam/  (openam.warが展開されたディレクトリ)
~/openam/  (OpenAMの設定ディレクトリ)
~/.openamcfg/  (OpenAMの設定ディレクトリへのパスが記載されたファイルを含むディレクトリ)
　バックアップの際に、Tomcatのworkディレクトリ以下も削除しておいて下さい。アップグレード前に生成されたクラスファイルなどが原因で、異常な動作する可能性があります。また、カスタマイズしたファイルなどがある場合は、アップグレード後にそれを適宜反映することを忘れないで下さい。

アップグレード手順

　では、実際にアップグレードをしてみましょう。私の環境では、CentOS 6.3にyumでインストールしたTomcat 6.0.24があり、その上にOpenAM 11.0.0がデプロイされていたので、これを12.0.0にアップグレードしてみます。

　OpenAMのwarファイルは以下のページから、ダウンロードできます。

　https://backstage.forgerock.com/#!/downloads/OpenAM/

　ダウンロードが完了したら、Tomcatを停止し、バックアップの取得と、warファイルの更新を行って、再度Tomcatを起動します。

```sh
# service tomcat6 stop
Stopping tomcat6:                                          [  OK  ]
# mkdir backup
# mv /usr/share/tomcat6/webapps/openam backup/webapps_openam
# cp -pr /usr/share/tomcat6/openam backup/openam
# cp -pr /usr/share/tomcat6/.openamcfg backup/.openamcfg
# rm -fr /usr/share/tomcat6/work/
# cp OpenAM-12.0.0.war /usr/share/tomcat6/webapps/openam.war
cp: `/usr/share/tomcat6/webapps/openam.war' を上書きしてもよろしいですか(yes/no)? yes
# service tomcat6 start
Starting tomcat6:                                          [  OK  ]
```

　Tomcatが起動したら、OpenAMにアクセスして下さい。以下のような画面が表示されます。

Screenshot-OpenAM - Mozilla Firefox-1

　「OpenAM 12.0.0へのアップグレード」と書かれたリンクをクリックして下さい。

※画面表示直後は、リンクが不活性になっている場合があります。この時、OpenAM内部でアップグレードの初期化処理が実行されているので、しばらく待って下さい。しばらく待っても、活性化しない場合は、何らかのバグにより、初期化処理が失敗している可能性があります。後述のトラブルシューティングで原因を特定して下さい。

Screenshot-OpenAM - Mozilla Firefox-2

　ライセンスの確認ダイアログが表示されるので、同意して「Continue」ボタンをクリックすると、以下のような画面が表示されます。

Screenshot-OpenAM - Mozilla Firefox-3

　「アップグレード」ボタンをクリックすると、アップグレードが開始されますが、その前にどのような変更が行われるか確認するため、「レポートの保存」ボタンをクリックして、アップグレードレポートを確認して下さい。アップグレードレポートには、以下のようにサービスやプロパティの変更内容が記載されています。アップグレードにより、意図しない影響が無いか念のためチェックして下さい。

Screenshot-upgradereport.1440847588666 (~-デスクトップ) - gedit

※アップグレードレポートは、アップグレード後にサーバーにも保存されます。あとで確認する必要が出た場合は、そちらを確認して下さい。

```sh
# vi /usr/share/tomcat6/openam/upgrade/upgradereport.20150829201121
```

　レポートを確認したら、「アップグレード」ボタンをクリックして、アップグレードを実施して下さい。

Screenshot-OpenAM - Mozilla Firefox-4

　完了すると、以下のようなダイアログが表示されます。

Screenshot-OpenAM - Mozilla Firefox-5

　問題無く完了したら、画面の指示通りTomcatを再起動して下さい。再起動後、OpenAMにアクセスすると、新しいログイン画面が表示されます。旧バージョンの画面が表示される場合は、ブラウザのキャッシュが残っているかもしれないので、それを削除してみて下さい。

Screenshot-OpenAM (ログイン) - Mozilla Firefox

　以上で基本的には完了ですが、追加の手順が必要な場合もあります。前述のForgeRockのアップグレードガイドに記載されているように、OAuth 2.0クライアントを使っていた場合は、設定の変更が必要です。

　なお、今回アップグレードはGUIで行いましたが、コマンドラインのツールで行うこともできます。詳細については、アップグレードガイドを参照して下さい。

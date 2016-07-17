## トラブルシューティング情報の記録

OpenAM 13.0.0から、トラブルシューティングに必要な情報を記録する機能が追加されました。cURLのようなRESTクライアントかssoadmツールでOpenAMサーバーにリクエストを送信すると、その時点からのOpenAMサーバーの様々な情報を記録して、zipファイルに圧縮して保存します。

この機能では、トラブルシューティングに必要となる以下の情報を記録します:

- OpenAMのデバッグログ
- スレッドダンプ(JStackのスタックトレースに似た出力で、すべてのアクティブなスレッドの状態を表示する)
- 重要なランタイム・プロパティ
- OpenAMの設定

RESTの場合は、まずOpenAMの管理者で認証を行ってSSOトークンを取得します。

```
$ curl \
 --request POST \
 --header "X-OpenAM-Username: demo" \
 --header "X-OpenAM-Password: changeit" \
 --header "Content-Type: application/json" \
 --data "{}" \
 http://openam.example.co.jp:8080/openam/json/authenticate
{ "tokenId": "AQIC5w...NTcy*", "successUrl": "/openam/console" }
```

http://[OpenAMサーバのURL]/json/records?_action=startに、以下のようなJSON形式のデータをPOSTメソッドで送信すると記録を開始します。

```
# curl --header "iPlanetDirectoryPro: AQIC5w (略) ..NTcy*"
 --header "Content-Type: application/json"
 --request POST
 --data 
'{
   "issueID":1,
   "referenceID":"ref001",
   "description":"Troubleshooting information for a customer",
   "zipEnable":true,
   "configExport":{
      "enable":true,
      "password":"password",
      "sharePassword":false
   },
   "debugLogs":{
      "debugLevel":"MESSAGE",
      "autoStop":{
         "time":{
            "timeUnit":"SECONDS",
            "value":15
         },
         "fileSize":{
            "sizeUnit":"GB",
            "value":1
         }
      }
   },
   "threadDump":{
      "enable":true,
      "delay":{
         "timeUnit":"SECONDS",
         "value":5
      }
   }
}'
"http://openam.example.co.jp:8080/openam/json/records?_action=start"
```

このリクエストは、OpenAMの設定情報をエクスポートしたxmlファイル、15秒間のデバッグログとスレッドダンプなどを出力し、まとめてzipファイルに圧縮することをOpenAMに要求します。これに対して、以下のようなレスポンスが返ります。

```
{
   "recording":true,
   "record":{
      "issueID":1,
      "referenceID":"ref001",
      "description":"Troubleshooting information for a customer",
      "zipEnable":true,
      "threadDump":{
         "enable":true,
         "delay":{
            "timeUnit":"SECONDS",
            "value":5
         }
      },
      "configExport":{
         "enable":true,
         "password":"xxxxxx",
         "sharePassword":false
      },
      "debugLogs":{
         "debugLevel":"message",
         "autoStop":{
            "time":{
               "timeUnit":"MILLISECONDS",
               "value":15000
            },
            "fileSize":{
               "sizeUnit":"KB",
               "value":1048576
            }
         }
      },
      "status":"RUNNING",
      "folder":"/usr/share/tomcat6/openam/openam/debug/record/1/ref001/"
   }
}
```

この中の”folder”の1つ上のディレクトリを確認してみると、zipファイルが生成されていることが分かります。

```
# ll /usr/share/tomcat6/openam/openam/debug/record/1/
Total 488
-rw-r--r-- 1 tomcat tomcat 131551 Dec 11 10:48 2015 ref001_2015-12-11-10-48-38-655-JST.zip
```

これを解凍すると、次のようなファイルが生成されます。

```
# unzip /usr/share/tomcat6/openam/openam/debug/record/1/ref001_2015-12-11-10-48-38-655-JST.zip
Archive: /usr/share/tomcat6/openam/openam/debug/record/1/ref001_2015-12-11-10-48-38-655-JST.zip
inflating: OpenamConfigExport_2015-12-11-10-48-23-646-JST.xml
inflating: RecordsHistory.txt
inflating: info.json
creating: threadDump/
inflating: threadDump/threadDump_2015-12-11-10-48-23-653-JST.txt
inflating: threadDump/threadDump_2015-12-11-10-48-28-675-JST.txt
inflating: threadDump/threadDump_2015-12-11-10-48-33-694-JST.txt
creating: debug/
inflating: debug/Session
```

スレッドダンプは、5秒間隔で3回取得されます。この間隔は、リクエストの”delay”の”value”で指定した値になります。

```
# view threadDump/threadDump_2015-12-11-10-48-23-653-JST.txt

***********************************
DATE : 2015-12-11 10:48:23:654 AM JST
"I/O dispatcher 14" Id=620 RUNNABLE (in native)
at sun.nio.ch.EPollArrayWrapper.epollWait(Native Method)
at sun.nio.ch.EPollArrayWrapper.poll(EPollArrayWrapper.java:269)
at sun.nio.ch.EPollSelectorImpl.doSelect(EPollSelectorImpl.java:79)
at sun.nio.ch.SelectorImpl.lockAndDoSelect(SelectorImpl.java:87)
at sun.nio.ch.SelectorImpl.select(SelectorImpl.java:98)
at org.apache.http.impl.nio.reactor.AbstractIOReactor.execute(AbstractIOReactor.java:257)
at org.apache.http.impl.nio.reactor.BaseIOReactor.execute(BaseIOReactor.java:106)
at org.apache.http.impl.nio.reactor.AbstractMultiworkerIOReactor$Worker.run(AbstractMultiworkerIOReactor.java:590)
...

"I/O dispatcher 13" Id=619 RUNNABLE (in native)
at sun.nio.ch.EPollArrayWrapper.epollWait(Native Method)
at sun.nio.ch.EPollArrayWrapper.poll(EPollArrayWrapper.java:269)
at sun.nio.ch.EPollSelectorImpl.doSelect(EPollSelectorImpl.java:79)
at sun.nio.ch.SelectorImpl.lockAndDoSelect(SelectorImpl.java:87)
at sun.nio.ch.SelectorImpl.select(SelectorImpl.java:98)
at org.apache.http.impl.nio.reactor.AbstractIOReactor.execute(AbstractIOReactor.java:257)
at org.apache.http.impl.nio.reactor.BaseIOReactor.execute(BaseIOReactor.java:106)
at org.apache.http.impl.nio.reactor.AbstractMultiworkerIOReactor$Worker.run(AbstractMultiworkerIOReactor.java:590)
...
(以下略)
```

記録機能が実行中であるかどうかは、/openam/json/records?_action=statusにリクエストを送信することで確認できます。

```
# curl --header "iPlanetDirectoryPro: AQIC5w (略) ..NTcy*"
 --header "Content-Type: application/json"
 --request POST
"http://openam.example.co.jp:8080/openam/json/records?_action=status"
```

“recording”:trueで”status”:”RUNNING”となっていれば、記録中ということになります。

```
{
   "recording":true,
   "record":{
      "issueID":1,
      "referenceID":"ref001",
      "description":"Troubleshooting information for a customer",
      "zipEnable":true,
      "threadDump":{
         "enable":true,
         "delay":{
            "timeUnit":"SECONDS",
            "value":5
         }
      },
      "configExport":{
         "enable":true,
         "password":"xxxxxx",
         "sharePassword":false
      },
      "debugLogs":{
         "debugLevel":"message",
         "autoStop":{
            "time":{
               "timeUnit":"MILLISECONDS",
               "value":15000
            },
            "fileSize":{
               "sizeUnit":"KB",
               "value":1048576
            }
         }
      },
      "status":"RUNNING",
      "folder":"/usr/share/tomcat6/openam/openam/debug/record/1/ref001/"
   }
}
```

この機能に関する詳細は、OpenAM 13.0.0のガイドを参照して下さい。

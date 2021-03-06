.. -*- mode: rst; coding: utf-8-unix -*-

===============================================================================================
テーブルをキューとして使ったメッセージングを再び起動したい場合にすること
===============================================================================================

概要
============

テーブルをキューとして使ったメッセージングを一端終了した後に再び起動させたい場合、処理対象データの処理済みフラグを再設定する必要がある。


手順
============

以下の方法でデータを再設定できる。

1.h2/bin/h2.batを実行する。

.. tip::

  必ず生成したバッチプロジェクトに含まれるh2.batを起動すること。


2.しばらく待つとブラウザが起動するので、各項目に以下の通りに入力し、[Test Connection]ボタンをクリックする。

============= ========================= ============================================================================
項目          値                        補足
============= ========================= ============================================================================
JDBC URL      ``jdbc:h2:../db/SAMPLE``  左記の通り、h2.batからの相対パスでデータファイルの位置を |br|
                                        指定する必要がある。
User Name     ``SAMPLE``
Password      ``SAMPLE``
============= ========================= ============================================================================

3.画面下部に「Test successful」と表示されていることを確認する。

4.Password欄を再び入力し、[Connect]ボタンをクリックする。

.. important ::

  [Connect]ボタンクリック時に、指定したURLにH2のデータファイルが存在しない場合、H2のデータファイルが新規生成される。

  トラブルを避けるために、手順2にあるように必ず[Test Connection]をクリックして、データファイルの存在を確認すること。


5.右側のペインの上部がSQLを入力するスペースになっているので、そこに以下のSQLを入力する。

.. code-block:: sql

  DELETE FROM SAMPLE_USER WHERE USER_INFO_ID = '00000000000000000001';

  INSERT INTO SAMPLE_USER(
    USER_INFO_ID
    , LOGIN_ID
    , KANA_NAME
    , KANJI_NAME
    , STATUS
  ) VALUES (
    '00000000000000000001'
    , 'tarou'
    , 'たろう'
    , '太郎'
    , '0' -- 0: 未処理
  );

  COMMIT;


.. tip ::

  上記SQLは、テーブルをキューとして使ったメッセージングの処理対象レコードの状態を「未処理」に設定している。


6.画面上部に存在する[Run]ボタン(緑色のボタン)をクリックする。

7.左上のdisconnectボタン(赤色で書かれたアイコンのボタン)をクリックして切断する。 

.. important ::

  アーキタイプから生成したプロジェクトはH2の組み込みモードを使用している。

  組み込みモード使用時は、1プロセスからのみ接続を受け付ける。

  そのため、 **切断を忘れると、アプリケーションからH2に接続できなくなる。**


.. |br| raw:: html

  <br />
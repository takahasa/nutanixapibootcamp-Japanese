.. _api_cluster_list:

----------------------
API: クラスターの一覧
----------------------

概要
++++++++

この演習では Prism Central に接続さているクラスタ一覧を取得します。
後ほど行う演習では得られたJSONレスポンスに含まれるクラスタのUUIDが必要となります。

.. ノート::

  想定演習時間: **5 分**



演習: クラスター一覧の取得
+++++++++++++++++++++++++++++++++++++++++++

#. Postmanでメイン画面にある「+」をクリックして新しいタブを作成します。

#. HTTPメソッドのドロップダウンをクリックしてPOSTを選択します。

    - Nutanix v3 API では POST を使ってサーバーサイドでのフィルタリング、グルーピング、ソートが標準化されています。

#. クラスターをリスト表示するためのURLを入力します。

    - https://{{prism_central_ip}}:9440/api/nutanix/v3/clusters/list

#. API呼び出しのためのベーシック認証の設定をします。

        - **Authorization** タブをクリックし、 **Basic Auth** をTypeのドロップダウンから選択してください。
        - クラスターのクレデンシャル情報を入力し、 **Update Request** をクリックしてください。
            - **Username** - admin
            - **Password** - 講師から与えられた“Prism login password”を使います。

        .. figure:: images/basicauth.png

#. メディアタイプを「JSON」にします。

        - Bodyタブをクリックします。
        - ラジオボタン(選択ボタン)でrawを選択します。
        - Textのドロップダウンをクリックし、「JSON」を選択します。

        .. figure:: images/jsonmediatype.png

#. Bodyにリクエストペイロードの値を記述します。

    - Bodyタブをクリックします。
    - コピーもしくは空データをJSONで記述します。

    .. code-block:: bash

      {}

    .. figure:: images/apimetajson.png

#. Sendボタンを押してv3 APIにリクエストを送信します。

    - レスポンスにはアレイ(リスト)形式でクラスターのリソース一覧が表示されます。
    - metadataの下に定義されているクラスターUUIDをメモしておきます。

  .. figure:: images/clusteruuid.png



まとめ
+++++++++
この演習ではクラスターをリスト表示するAPIの呼び出しを行いました。
全てのNutanix v3 APIの呼び出しではクラスターUUIDが必要となります。
また、ここで設定したクラスターの認証情報は同様に残りの演習でも利用します。

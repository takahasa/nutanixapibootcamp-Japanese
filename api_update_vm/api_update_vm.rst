.. _api_update_vm:

----------------------
API: VMの更新
----------------------

概要
++++++++

この演習では仮想マシンの2つの更新を1回のAPI呼び出しで同時に更新をします。
具体的には先程作成した仮想マシンに仮想ディスクを接続し、電源をONにします。

.. ノート::

  想定演習時間: **20 分**




演習: VMの更新
++++++++++++++++++++

#. Postmanの「+」ボタンを押して新しいリクエストのタブを作成します。

#. HTTPメソッドで「PUT」を選択します。

    - v3 API は PUT メソッドを使って更新させる「spec」の状態を宣言します。どのようにVMを更新するかをボディのspecに記述します。

#. VMを更新するために以下のURLを入力します。

    - 3番目の演習で使った右のURLをコピーします。: https://{{prism_central_ip}}:9440/api/nutanix/v3/vms/{{uuid}}
    - {{prism_central_ip}} を正しい Prism Central のIPに置き換えます。

    .. figure:: images/updatevm.png

#. ベーシック認証を設定します。設定が残っていれば本手順は飛ばします。

    - **Authorization** タブをクリックし **Basic Auth** をTypeのドロップダウンから選択します。
    - プリズムのクレデンシャルを入力し **Update Request** をクリックします。:
        - **Username** - admin
        - **Password** - 講師から与えられた“Prism login password”を使います。
    - v3 API はHTTPをステートレス(状態がない)なプロトコルとして扱います。そのため、認証はAPIの呼び出しごとに毎回おこなわれます。

#. メディアタイプを「JSON」にします。

        - Bodyタブをクリックします。
        - ラジオボタン(選択ボタン)でrawを選択します。
        - Textのドロップダウンをクリックし、「JSON」を選択します。

#. Bodyにリクエストペイロードの値を記述します。

    - **演習3(VMの状態の取得)** のリクエストタブを開きます。
    - レスポンスデータの全てをコピーします。
    - この演習のリクエストタブをクリックして設定に戻ります。
    - HTTPメソッドをPUTにして、先ほど取得したレスポンスデータをボディとして貼り付けます。
    - ボディから「status(keyとvalueの両方)」の行を全て削除し、specとmetadataはそのまま保持します。

    .. figure:: images/deletestatus.png

#. ボディを編集してディスクのマウントとパワーオンを追加します。

    - power_state要素をOFFからONに変更します。
    - 「"disk_list": [] 」を探し、以下のように書き換えます。

    .. code-block:: bash

        "disk_list": [{
      "device_properties": {
          "disk_address": {
              "device_index": 0,
              "adapter_type": "SCSI"
          },
          "device_type": "DISK"
      },
      "data_source_reference": {
          "kind": "image",
          "uuid": "<imageuuid>"
      }
      }]



 - 上記の <imageuuid> を **演習4** で確認したCentOSイメージのUUIDに置き換えます。

    .. figure:: images/updatevmstate.png

#. Sendボタンを押してv3 APIにリクエストを送信します。

    - v3 のPUT命令は **202** を設定受付の成功時に返します。
    - レスポンスのstateが **PENDING** となっている場合は、VMを要求した状態に移行している最中です。
    - 多くのAPIの実装では仮想マシンのパワーオンとディスクの追加をおこなうためには2回のリクエストが必要です。一方、v3 APIでは両方の操作(厳密には2以上も可能)を1度の **PUT **リクエストで実施することができます。
    - そのため、v3 APIは劇的に少ないAPIのURL数で様々な要素の変更をPUTリクエストのspec要素の指定にて実行できます。

#. 仮想マシンの状態を取得します。

    - 仮想マシンの状態を取得した **演習3** のタブをクリックします。
    - **Send** ボタンを再度クリックして、 **GET** メソッドで改めて仮想マシンの状態を取得します。
    - **state** が COMPLETE になったら、**status** に変更が適用されています。

#. Prismにログインして確認します。

    - ブラウザを開いてPrism Centralにアクセスします。: https://{{prism_central_ip}}:9440/console/
    - Prism Centralで **Username** と **Password** を入力してログインします。
    - 「f」キーを押すか検索アイコンをクリックして検索バーを表示します。
    - 仮想マシン名を入力します。イニシャルが名前につけられているはずです。
    - テーブルにある仮想マシンをクリックして選択し、テーブル下に表示される **Launch Console** ボタンを押します。
    - CentOSにログインするためのウィンドウが表示されます(電源ONとディスクのアタッチに成功している事になります。)

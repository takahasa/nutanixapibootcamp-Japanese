.. _api_create_vm:

----------------------
API: VMの作成
----------------------

概要
++++++++

この演習では Nutanix v3 APIs を利用して仮想マシンの作成を行います。
仮想マシンは仮想ディスク(vDisk)を搭載せず、パワーオフの状態で作成されます。

.. ノート::

  想定演習時間: **20 分**



演習: 仮想マシンの作成
++++++++++++++++++++++++++++++

#. Postmanの「+」ボタンをクリックして、新しいリクエストタブを作成します。

    .. figure:: images/newtab.png

#. HTTPメソッドのドロップダウンをクリックしPOSTを選択します。

    - v3 API は RESTful な設計になっており、GET, POST, PUT, DELETEといった一般的なHTTPメソッドを利用します。

    .. figure:: images/postfunction.png

#. 仮想マシンを作成するために以下のURLを入力します。

    - https://{{prism_central_ip}}:9440/api/nutanix/v3/vms
    - v3 APIはセマンティックURLという仕組みを採用しており、APIの理解と利用を簡単にしています。

    .. figure:: images/urlcreate.png

#. ベーシック認証の設定をします。

    - **Authorization** タブをクリックし **Basic Auth** をTypeのドロップダウンから選択します。
    - プリズムのクレデンシャルを入力し **Update Request** をクリックします。:
        - **Username** - admin
        - **Password** - 講師から与えられた“Prism login password”を使います。
        - v3 API はHTTPをステートレス(状態がない)なプロトコルとして扱います。そのため、認証はAPIの呼び出しごとに毎回おこなわれます。

    .. figure:: images/basicauth.png

#. メディアタイプを「JSON」にします。

        - Bodyタブをクリックします。
        - ラジオボタン(選択ボタン)でrawを選択します。
        - Textのドロップダウンをクリックし、「JSON」を選択します。

        .. figure:: images/jsonmediatype.png

#. Bodyにリクエストペイロードの値を記述します。

    - 「VM intent input」として以下をコピーもしくは入力します。

    .. code-block:: bash

      {
          "spec": {
              "name": "API-VM-<initial>",
              "cluster_reference": { "kind": "cluster", "uuid": "<clusteruuid>"},
              "resources": {
                  "num_vcpus_per_socket": 1,
                  "num_sockets": 1,
                  "memory_size_mib": 1024,
                  "power_state": "OFF"
              }
          },
          "api_version": "3.0",
          "metadata": {
              "kind": "vm"
          }
      }


    - 仮想マシン名の<initial>を受講者のイニシャルに変更します。
    - クラスタのUUIDの<clusteruuid>を先の演習で確認したクラスタのUUIDに変更します。
    - このあとに得られるレスポンスボディから、仮想マシンのUUIDを確認してメモしておきます。

7. Sendボタンを押してv3 APIにリクエストを送信します。

    v3 APIはリクエストに応じたレスポンス(ステータスとボディ)を返し、Postmanはそれを表示します。

    .. figure:: images/vmuuid.png

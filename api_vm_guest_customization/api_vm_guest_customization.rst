.. _api_vm_guest_customization:

----------------------
API: ゲストVMのカスタマイズ
----------------------

概要
++++++++

.. ノート::

  想定演習時間: **20 分**

この演習ではWindows VMをNutanix v3 APIを使って作成します。
APIのボディに「unattend.xml」を設定することで、ゲストカスタマイズ機能を使います。
「unattend.xml」はbase64でエンコードされています。
もし興味があれば、Linuxを使って "echo <base64-content> | base64 --decode" とすれば、base64をデコードした生のXMLを確認できます。



演習: Windows VMの作成
++++++++++++++++++++++++++++++

#. Postmanの「+」ボタンをクリックして新しいリクエストタブを作成します。

    .. figure:: images/newtab.png

#. HTTPメソッドのドロップダウンで POST を選択します。

    - v3 API は Restfulな設計で、一般的なHTTPメソッドであるGET, POST, PUT, DELETEなどを使います。

    .. figure:: images/postfunction.png

#. 仮想マシンを作成するために以下のURLを入力します。

    - https://{{prism_central_ip}}:9440/api/nutanix/v3/vms
    - v3 は簡単なセマンティックAPIを提供します。

    .. figure:: images/urlcreate.png

#. ベーシック認証を設定します。設定が残っていれば本手順は飛ばします。

    - **Authorization** タブをクリックし **Basic Auth** をTypeのドロップダウンから選択します。
    - プリズムのクレデンシャルを入力し **Update Request** をクリックします。:
        - **Username** - admin
        - **Password** - 講師から与えられた“Prism login password”を使います。
    - v3 API はHTTPをステートレス(状態がない)なプロトコルとして扱います。そのため、認証はAPIの呼び出しごとに毎回おこなわれます。

    .. figure:: images/basicauth.png

#. メディアタイプを「JSON」に設定する

    - Bodyタブをクリックします。
    - ラジオボタンで raw を選択します。
    - Text のドロップダウンをクリックし「JSON」を選択

    .. figure:: images/jsonmediatype.png

#. Bodyにリクエストペイロードの値を記述します。

    - 以下をコピーもしくは入力します。

    .. code-block:: bash

      {

      "spec":{

      "name":"win-sysprep-postman-<initial>",

      "resources":{

      "nic_list":[

      {

      "nic_type":"NORMAL_NIC",

      "network_function_nic_type":"INGRESS",

      "subnet_reference":


      { "kind":"subnet", "uuid":"<subnetuuid>" }

      }

      ],

      "boot_config":{

      "boot_device":{

      "disk_address":


      { "device_index":0, "adapter_type":"SCSI" }

      }

      },

      "num_vcpus_per_socket":1,

      "num_sockets":1,

      "memory_size_mib":1024,

      "power_state":"ON",

      "guest_customization":{

      "sysprep":


      { "install_type":"PREPARED", "unattend_xml":"PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCiAgICA8dW5hdHRlbmQgeG1sbnM9InVybjpzY2hlbWFzLW1pY3Jvc29mdC1jb206dW5hdHRlbmQiPg0KICAgICAgICA8c2V0dGluZ3MgcGFzcz0ib29iZVN5c3RlbSI+DQogICAgICAgICAgICA8Y29tcG9uZW50IG5hbWU9Ik1pY3Jvc29mdC1XaW5kb3dzLVNoZWxsLVNldHVwIiBwcm9jZXNzb3JBcmNoaXRlY3R1cmU9ImFtZDY0IiBwdWJsaWNLZXlUb2tlbj0iMzFiZjM4NTZhZDM2NGUzNSIgbGFuZ3VhZ2U9Im5ldXRyYWwiIHZlcnNpb25TY29wZT0ibm9uU3hTIiB4bWxuczp3Y209Imh0dHA6Ly9zY2hlbWFzLm1pY3Jvc29mdC5jb20vV01JQ29uZmlnLzIwMDIvU3RhdGUiIHhtbG5zOnhzaT0iaHR0cDovL3d3dy53My5vcmcvMjAwMS9YTUxTY2hlbWEtaW5zdGFuY2UiPg0KICAgICAgICAgICAgICAgIDxVc2VyQWNjb3VudHM+DQogICAgICAgICAgICAgICAgICAgIDxBZG1pbmlzdHJhdG9yUGFzc3dvcmQ+DQogICAgICAgICAgICAgICAgICAgICAgICA8VmFsdWU+TnV0YW5peDEyMyM8L1ZhbHVlPg0KICAgICAgICAgICAgICAgICAgICAgICAgPFBsYWluVGV4dD50cnVlPC9QbGFpblRleHQ+DQogICAgICAgICAgICAgICAgICAgIDwvQWRtaW5pc3RyYXRvclBhc3N3b3JkPg0KICAgICAgICAgICAgICAgICAgICA8TG9jYWxBY2NvdW50cz4NCiAgICAgICAgICAgICAgICAgICAgICA8TG9jYWxBY2NvdW50IHdjbTphY3Rpb249ImFkZCI+DQogICAgICAgICAgICAgICAgICAgICAgICAgPFBhc3N3b3JkPg0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIDxWYWx1ZT5OdXRhbml4MTIzIzwvVmFsdWU+DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgPFBsYWluVGV4dD50cnVlPC9QbGFpblRleHQ+DQogICAgICAgICAgICAgICAgICAgICAgICAgPC9QYXNzd29yZD4NCiAgICAgICAgICAgICAgICAgICAgICAgICA8RGVzY3JpcHRpb24+VGVzdCBhY2NvdW50PC9EZXNjcmlwdGlvbj4NCiAgICAgICAgICAgICAgICAgICAgICAgICA8RGlzcGxheU5hbWU+TnV0YW5peDwvRGlzcGxheU5hbWU+DQogICAgICAgICAgICAgICAgICAgICAgICAgPEdyb3VwPkFkbWluaXN0cmF0b3JzO1Bvd2VyIFVzZXJzPC9Hcm91cD4NCiAgICAgICAgICAgICAgICAgICAgICAgICA8TmFtZT5OdXRhbml4PC9OYW1lPg0KICAgICAgICAgICAgICAgICAgICAgIDwvTG9jYWxBY2NvdW50Pg0KICAgICAgICAgICAgICAgICAgICA8L0xvY2FsQWNjb3VudHM+DQogICAgICAgICAgICAgICAgPC9Vc2VyQWNjb3VudHM+DQogICAgICAgICAgICAgICAgPE9PQkU+DQogICAgICAgICAgICAgICAgICAgIDxIaWRlRVVMQVBhZ2U+dHJ1ZTwvSGlkZUVVTEFQYWdlPg0KICAgICAgICAgICAgICAgIDwvT09CRT4NCiAgICAgICAgICAgIDwvY29tcG9uZW50Pg0KICAgICAgICAgICAgPGNvbXBvbmVudCBuYW1lPSJNaWNyb3NvZnQtV2luZG93cy1JbnRlcm5hdGlvbmFsLUNvcmUiIHByb2Nlc3NvckFyY2hpdGVjdHVyZT0iYW1kNjQiIHB1YmxpY0tleVRva2VuPSIzMWJmMzg1NmFkMzY0ZTM1IiBsYW5ndWFnZT0ibmV1dHJhbCIgdmVyc2lvblNjb3BlPSJub25TeFMiIHhtbG5zOndjbT0iaHR0cDovL3NjaGVtYXMubWljcm9zb2Z0LmNvbS9XTUlDb25maWcvMjAwMi9TdGF0ZSIgeG1sbnM6eHNpPSJodHRwOi8vd3d3LnczLm9yZy8yMDAxL1hNTFNjaGVtYS1pbnN0YW5jZSI+DQogICAgICAgICAgICAgICAgPElucHV0TG9jYWxlPmVuLVVTPC9JbnB1dExvY2FsZT4NCiAgICAgICAgICAgICAgICA8U3lzdGVtTG9jYWxlPmVuLVVTPC9TeXN0ZW1Mb2NhbGU+DQogICAgICAgICAgICAgICAgPFVJTGFuZ3VhZ2U+ZW4tVVM8L1VJTGFuZ3VhZ2U+DQogICAgICAgICAgICAgICAgPFVzZXJMb2NhbGU+ZW4tVVM8L1VzZXJMb2NhbGU+DQogICAgICAgICAgICA8L2NvbXBvbmVudD4NCiAgICAgICAgPC9zZXR0aW5ncz4NCiAgICAgICAgPHNldHRpbmdzIHBhc3M9InNwZWNpYWxpemUiPg0KICAgICAgICAgICAgPGNvbXBvbmVudCBuYW1lPSJNaWNyb3NvZnQtV2luZG93cy1TaGVsbC1TZXR1cCIgcHJvY2Vzc29yQXJjaGl0ZWN0dXJlPSJhbWQ2NCIgcHVibGljS2V5VG9rZW49IjMxYmYzODU2YWQzNjRlMzUiIGxhbmd1YWdlPSJuZXV0cmFsIiB2ZXJzaW9uU2NvcGU9Im5vblN4UyIgeG1sbnM6d2NtPSJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL1dNSUNvbmZpZy8yMDAyL1N0YXRlIiB4bWxuczp4c2k9Imh0dHA6Ly93d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIj4NCiAgICAgICAgICAgICAgICA8Q29tcHV0ZXJOYW1lPkNhbG08L0NvbXB1dGVyTmFtZT4NCiAgICAgICAgICAgICAgICA8UmVnaXN0ZXJlZE9yZ2FuaXphdGlvbj5OdXRhbml4PC9SZWdpc3RlcmVkT3JnYW5pemF0aW9uPg0KICAgICAgICAgICAgICAgIDxSZWdpc3RlcmVkT3duZXI+QWNyb3BvbGlzPC9SZWdpc3RlcmVkT3duZXI+DQogICAgICAgICAgICAgICAgPFRpbWVab25lPlBhY2lmaWMgU3RhbmRhcmQgVGltZTwvVGltZVpvbmU+DQogICAgICAgICAgICA8L2NvbXBvbmVudD4NCiAgICAgICAgPC9zZXR0aW5ncz4NCiAgICA8L3VuYXR0ZW5kPg==" }

      },

      "disk_list":[

      {

      "data_source_reference":



      { "kind":"image", "uuid": "<diskimageuuid>" }

      ,

      "device_properties":{

      "disk_address":



      { "device_index":0, "adapter_type":"SCSI" }

      ,

      "device_type":"DISK"

      }

      }

      ]

      }

      },

      "api_version":"3.0",

      "metadata":{

      "kind":"vm",

      "categories":


      { "Project":"default" }

      }

      }




    上記のボディの何箇所かを修正する必要があります。
      - VM名の最後をあなたのイニシャルに設定します。上記の <initial> をあなたのイニシャルに変更します。
      - サブネットの UUID を受講者のクラスターが持つサブネットのUUIDに変更します。上記の <subnetuuid> を置き換えます。
      - ディスクイメージのUUIDをWindowsに変更します。上記の <diskimageuuid> を置き換えます。


    以下の設定が unattend.xml より適用されます。
      - 新しいユーザー Nutanix を作成し、そのパスワードを Nutanix123# とします。
      - タイムゾーンを PST にします。
      - システム言語を en-US にします。
      - ホスト名を Calm にします。


7. Sendボタンを押してv3 APIにリクエストを送信します。

    v3 APIは要求されたリクエストに対するステータスとレスポンスボディを返します。

    .. figure:: images/createresponse.png



8. Prism UI で確認する

    - ブラウザを開いてPrism Centralにアクセスします。: https://{{prism_central_ip}}:9440/console/
    - Prism Centralで **Username** と **Password** を入力してログインします。
    - 「f」キーを押すか検索アイコンをクリックして検索バーを表示します。
    - 仮想マシン名を入力します。イニシャルが名前につけられているはずです。
    - テーブルに表示されたVMをクリックし、テーブル下に表示された **Launch Console** ボタンを押します。
    - Windowsのログイン画面が表示されたウィンドウが表示されます。

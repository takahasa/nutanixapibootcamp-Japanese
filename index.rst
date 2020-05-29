.. title:: Nutanix API Bootcamp

.. toctree::
  :maxdepth: 2
  :caption: API Labs
  :name: _api_labs
  :hidden:

  env_setup/env_setup
  api_cluster_list/api_cluster_list
  api_create_vm/api_create_vm
  api_vm_status/api_vm_status
  api_image_list/api_image_list
  api_update_vm/api_update_vm
  api_subnet_list/api_subnet_list
  api_vm_guest_customization/api_vm_guest_customization
  api_delete_vm/api_delete_vm
  api_vm_list/api_vm_list

.. toctree::
  :maxdepth: 2
  :caption: Nutanix Configuration
  :name: _nutanix_configuration
  :hidden:



.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:


  appendix/glossary
  appendix/basics

.. _getting_started:

---------------
はじめに
---------------

ようこそ Nutanix API Bootcamp へ!
この演習では Prism Central v3 APIs を使ってもらうことで、NutanixのAPIに慣れる事ができます。
演習の題材として仮想マシン(VM)のデプロイとアップデートを実施します。


準備
+++++++++++++

- 配られたパスワードをメモします。
- 下記にある情報にしたがって、リモートマシンに接続します。
- Prismのイメージサービスを使ってWindowsとLinuxのイメージをアップロードします。:
  - 「WindowsToolsVM.qcow2」(http://10.42.194.11/workshop_staging/WindowsToolsVM.qcow2)
  - 「Windows2016.qcow2」(http://10.42.194.11/workshop_staging/Windows2016.qcow2)
  - 「CentOS7.qcow2」(http://10.42.194.11/workshop_staging/CentOS7.qcow2)
- Postmanというツールのアカウントを取得しておきます。(必須ではありません。)

演習環境について
+++++++++++++++++++

この演習(Nutanix Workshops)は Nutanix社の Hosted POC (検証用の環境)を使って行われます。
参加者が利用するクラスターは必要なイメージ、ネットワーク、仮想マシンなどの設定が事前に準備されています。

ネットワーク
..........

NutanixのHosted POCにあるクラスターは以下のルールに沿って設定されています。:

- **クラスター名** - POC\ *XYZ*
- **サブネット** - 10.**21**.\ *XYZ*\ .0
- **クラスターIP(外部IP)** - 10.**21**.\ *XYZ*\ .37

もしマーケティング用のPOC環境でクラスタが準備されていれば以下となります。:

- **クラスター名** - MKT\ *XYZ*
- **サブネット** - 10.**20**.\ *XYZ*\ .0
- **クラスターIP(外部IP)** - 10.**20**.\ *XYZ*\ .37

クラスターの例:

- **クラスター名** - POC055
- **サブネット** - 10.21.55.0
- **クラスターIP(外部IP)** - 10.21.55.37

この演習をとおしてガイドのなかにある  *XYZ* といった表記を受講者の環境の値(たとえばネットワークのアドレスなど)に置き換える場合があります。
たとえば以下のようなものです。:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.21.\ *XYZ*\ .37
     - Nutanix Cluster の Virtual IP(外部IP)
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local ドメインコントローラー(AD)

それぞれのクラスターは仮想マシンに利用できる2つのVLANが設定されています。:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCPのレンジ
  * - Primary
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

認証情報
...........

.. note::

   *<Cluster Password>* は各クラスターごとに異なります。このワークショップの実施者から提供されますのでメモしておいてください。

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

各クラスターはそれぞれ仮想マシンのドメインコントローラー **DC** を持っており、 **NTNXLAB.local** というドメインとしてAD機能を提供しています。
ドメインは以下の Users と Groups を提供しています。:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

環境へのアクセス方法
+++++++++++++++++++

Nutanix Hosted POC 環境へのアクセス方法はいくつかあります:

ラボにアクセスするためのユーザー認証情報
...........................

PHXのクラスター:
**Username:** PHX-POCxxx-User01 (から PHX-POCxxx-User20 まで), **Password:** *<Provided by Instructor>*

RTPのクラスター:
**Username:** RTP-POCxxx-User01 (から RTP-POCxxx-User20 まで), **Password:** *<Provided by Instructor>*

FrameのVDI
.........

Login to: https://frame.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Parallels VDI
.................

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

社員向けの Pulse Secure VPN
..........................

Download the client:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

クライアントソフトをダウンロードします。

ツールを開いて、 **Add** で接続を追加する:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com


Nutanix環境のバージョン
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337
- **AOS Version** - 5.11.2.3
- **PC Version** - 5.11.2.1

## Network > Network Interface > コンソール使用ガイド


#### ネットワークインターフェイス作成

* 名前：ネットワークインターフェイスの名前を入力します。

* VPC：ネットワークインターフェイスを作成するVPCを選択します。

* サブネット：ネットワークインターフェイスを作成するSubnetを選択します。

* 仮想IP：冗長化のための仮想IP用のネットワークインターフェイスを作成します。<br>仮想IPとして使用するIPを他のインスタンス、ロードバランサーなどのリソースに割り当てられないようにできます。<br>仮想IPとして作成されたネットワークインターフェイスは"VIRTUAL_IP"というデバイス名を持ち、ルーティングテーブルのルート設定でルートのゲートウェイに指定できます。<br>仮想IPとして作成したネットワークインターフェイスは、インスタンス作成時に直接接続して使用できません。

* セキュリティ：スプーフィングを防止し、ネットワークインターフェイスにセキュリティグループを設定することができる機能です。<br>セキュリティ機能を使用すると、ネットワークインターフェイスのIP/MACアドレスを出発地として持たないパケットが該当ネットワークインターフェイスを介して送信されることを防ぎ、スプーフィングを防止し、セキュリティグループを利用してトラフィック制御をできます。 <br>セキュリティ機能を使用しない場合、このようなスプーフィング防止機能およびセキュリティグループ設定が全て解除されます。これにより、すべてのパケットが許可されるため、独自のセキュリティ機能(ファイアウォールなど)を持っていない場合は必ず使用することを推奨します。

* セキュリティグループ選択：ネットワークインターフェイスで使用するセキュリティグループを選択します。複数選択できます。

**確認**をクリックするとネットワークインターフェイスが作成されます。

> [参考]インスタンス作成時にネットワークインターフェイスを作成する場合と既存のネットワークインターフェイスを指定する場合の違い
>
> * 新規にネットワークインターフェイスを作成する場合
> そのインスタンスを削除すると、自動的に作成したネットワークインターフェイスも一緒に削除されます。
> * 既存のネットワークインターフェイスを指定してインスタンスを作成し、接続されたインスタンスを削除する場合
> そのインスタンスで使用していたネットワークインターフェイスは一緒に削除されません。残されたネットワークインターフェイスは、後日、他のインスタンスで接続して使用できます。


#### ネットワークインターフェイス変更
ネットワークインターフェイスのプロパティのうち、名前、IP、セキュリティグループを変更できます。
Floating IP接続がない時のみ変更が可能です。
IP変更が反映されるには、インスタンスの再起動を行いDHCPが更新されるまで時間が必要です。

#### ネットワークインターフェイス削除
選択したネットワークインターフェイスを削除できます。
削除をするにはネットワークインターフェイが装置に接続されていてはいけません。

#### ソース/対象確認変更
ソース/対象の確認を有効化または無効化してインスタンスが受信するトラフィックのソースまたは対象であることを確認できます。
NATインスタンスのネットワークインターフェイスの場合には、ソース/対象確認が有効になっていてはいけないため、このメニューから有効化/無効化機能を管理できます。

#### 仮想IP使用
仮想IP用に作成したネットワークインターフェイスは、IP先取りとルーティングテーブルでルートゲートウェイとして指定するためのもので、特定のインスタンスと直接接続されません。
仮想IPとして指定したIPを使用するためには、次のような手順が必要です。

1. セキュリティ設定の変更
ネットワークインターフェイスの**セキュリティ**機能は、スプーフィング防止機能を有効にし、インスタンスから出発するパケットがクラウドシステムで割り当てたIPを送信元アドレスとして持つ場合にのみ送信できます。
したがって、仮想IPを使用するためには、このようなスプーフィング防止機能を迂回するようにセキュリティ設定を変更する必要があります。
    * セキュリティ機能の解除
       ネットワークインターフェイスの**セキュリティ**機能を使用しない場合、スプーフィング防止機能が解除され、仮想IPを制限なく使用できます。
       ただし、この場合、セキュリティグループを適用できないため、独自のセキュリティ機能を備えた場合以外は推奨しません。
    * 追加許可アドレスの登録
       仮想IPを使用するインスタンスのネットワークインターフェイスに仮想IPを「追加許可アドレス」として登録すると、このIPを送信元アドレスとして持つパケットの送信を許可します。
       制限的にスプーフィング防止機能を迂回し、セキュリティ機能を解除せず、セキュリティグループの適用も可能です。
       ただし、現在、コンソールで機能を提供していないため、この機能を使用するためには、仮想IPを作成した後、下記のフォームで[1:1お問い合わせ](https://www.nhncloud.com/kr/support/inquiry)を行ってください(今後、コンソールに機能を提供予定)。
       <pre><code class="language-console">1. 追加許可アドレス登録申請リクエスト
       \- 組織ID/プロジェクトID:
       \- リージョン: (パブリック/公共、 KR1/KR2)<br>2. 仮想IP
       \- IPアドレス:
       \- VPC ID:<br>3. 追加許可アドレス登録対象
       \- インスタンス名: 
       \- インスタンスUUID:
       \- 仮想IPを接続するネットワークインターフェイスのIP:</code></pre>

2. インスタンス運営システムの構成
セキュリティ設定の変更が完了したら、インスタンスに仮想IPを構成します。クラウドシステムでは、これに対する機能を別途提供しておらず、インスタンス内でインスタンスの運営システムやディストリビューションに応じた適切な方法でIPを追加したり、アプリケーションを利用して直接構成する必要があります。

> [参考]仮想IPを使用する場合、次の点に注意してください。
> * 仮想IPとこれを使用するインスタンスのネットワークインターフェイスは、同じサブネット内に位置する必要があります。
> * 仮想IPを使用して通信する場合、仮想IPを使用するインスタンスは既存のセキュリティグループがそのまま適用されます。つまり、仮想IPには別途に適用されるセキュリティグループのルールがありません。
> * 仮想IPを送信元アドレスとして持つパケットを受信する必要があるインスタンスは、セキュリティグループに仮想IPをリモートで受信できるルールを確認する必要があります。
>  仮想IPはどのセキュリティグループにも属さないため、受信ルールにリモートで「セキュリティグループ」を指定した場合、仮想IPがそのセキュリティグループに含まれず、通信ができない場合があります。
> * 仮想IPを使用するインスタンスは、ネットワーク構成に応じて、インスタンス内部のルーティングルールを直接調整する必要があります。

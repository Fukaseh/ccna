# コマンド一覧

## 補足
|モード名|プロンプト|下表での記載方法|
|-------|---------|---------------|
|ユーザEXECモード|>|>|
|特権EXECモード|#|#|
|グローバルコンフィギュレーションモード|(config)#|config|
|インターフェイスコンフィギュレーションモード|(config-if)#|config-if|
|ルーティングコンフィギュレーションモード|(config-router)#|config-router|
|ラインコンフィギュレーションモード|(config-line)#|config-line|
|標準ACLコンフィギュレーションモード|(config-std-nacl)#|config-std-nacl|
|拡張ACLコンフィギュレーションモード|(config-ext-nacl)#|config-ext-nacl|
|DHCPコンフィギュレーションモード|(dhcp-config)#|dhcp-config|
|VLANコンフィギュレーションモード|(config-vlan)#|config-vlan|
|サブインターフェイスコンフィギュレーションモード|(config-subif)#|config-subif|
|インターフェイスコンフィギュレーションモード|(config-if-range)#|config-if-range|

### 分類、補足の項
|分類、補足|説明|
|---------|----|
|MODE|モード切替のコマンド|
|SHOW|設定を確認するコマンド|
|1, 2, ... |流れで覚えるコマンド, その間に別の操作が必要な場合は()で表記. やり方が複数ある場合は同じ番号が連続する|
|※|モードに注意|
|※C|コマンドを間違えやすいので注意|


## 第2章 Ciscoルータの初期設定
### Ciscoルータの操作の基本
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|>|enable|ユーザEXEC→特権EXEC|MODE|
|#|disable|特権EXEC→ユーザEXEC|MODE|
|#|configure terminal|特権EXEC→config|MODE|
|-|end|様々な設定モード→特権EXEC|MODE|
|-|exit|ひとつ前のモードに戻る|MODE|
|#|show <確認したい項目>|設定を確認|SHOW|
|#|copy running-config startup-config|running-configの内容をstartup-configに保存|-|
|#|erase startup-config|startup-configを削除(初期化)|-|
|#|reload|ルータを再起動(初期化に必要)|-|

### showコマンドのパイプを用いた検索機能
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|#|show <調べたい内容> \| begin <指定する内容>|一致した行から表示を開始|SHOW| 
|#|show <調べたい内容> \| exclude <指定する内容>|一致した行を表示しない|SHOW|
|#|show <調べたい内容> \| include <指定する内容>|一致した行のみを表示|SHOW|
|#|show <調べたい内容> \| section <指定する内容>|一致したセクションを表示|SHOW|



### Ciscoルータの基本設定
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|hostname <ホスト名>|ホスト名(機器の名前)を変更|-|
|config|line console 0|config→config-line|MODE, 1|
|config-line|password <パスワード>|パスワードを設定|2|
|config-line|login|パスワード認証を有効化|3|
|config|enable password <パスワード>|イネーブルパスワードの設定|-|
|config|enable secret <パスワード>|暗号化されたイネーブルパスワードの設定(こちらの方が優先)|-|
|config|line vty <開始ライン番号> [<終了ライン番号>]|VTYパスワードの設定のためのconfig-lineに.config→config-line|MODE|
|config|username <ユーザ名> [privilege <特権レベル> ] password <パスワード>|ユーザアカウントの作成|1,※|
|config-line|login local|ユーザアカウントを使用して認証(認証方法を変更)|(2),※|
|config|service password-encryption|パスワード全体を暗号化|-|
|config-line|exec-timeout <分> [<秒>]|自動ログアウトの設定|※|

### SSHの設定
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|hostname <ホスト名>|ホスト名(機器の名前)を変更|1|
|config|ip domain-name <ドメイン名>|ドメイン名を設定|2|
|config|crypto key generate rsa [modulus <暗号鍵サイズ]|RSA暗号鍵の生成|3|
|config|username <ユーザ名> [privilege <特権レベル> ] password <パスワード>|ユーザアカウントの作成|4|
|config-line|login local|ユーザアカウントを使用して認証(認証方法を変更)|(5),※|
|config-line|transport input <telnet\|ssh\|all\|none>|SSHの接続許可の設定(SSH推奨)|6|
|config|ip ssh version <1\|2>|SSHのバージョンの設定(2推奨)|7|


## 第3章 ルータの機能とルーティング
### ルータへのIPアドレスの設定
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|interface <タイプ> <インターフェイス番号>|config→config-if.タイプはEthernetやFastEthernetなど(p133).show runで確認|MODE,1|
|config-if|ip address <IPアドレス> <サブネットマスク>|IPアドレスの設定|2|
|config-if|no shutdown|インターっフェイスの有効化|3|

### ルータの基本設定
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config-if|duplex <auto \|full \|half>|通信モードの設定(初期:auto)|-|
|config-if|speed <10\|100\|1000\|auto>|通信速度の設定(初期:auto).単位はMbps|-|
|#|show interfaces [<インターフェイス>]|ルータやスイッチのインターフェイスの状態の確認|SHOW|
|#|show ip interface [brief] [<インターフェイス>]|インターフェイスのIPに関する情報の確認|SHOW|
|#|ping <IPアドレス\|ホスト名>|標準ping(宛先機器との接続を確認)|-|
|#|ping|拡張ping(この後指示に従って設定)|-|
|#|ping <IPアドレス\|ホスト名>source <IPアドレス\|インターフェイス名>|送信元インターフェイスも指定した標準ping|-|
|#|traceroute <IPアドレス\|ホスト名>|宛先までの通信経路の確認(Cisco機器)|

### スタティックルーティング
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|#|show ip route|ルーティングテーブルの確認|SHOW|
|config|ip route <宛先ネットワーク> <サブネットマスク> <ネクストホップアドレス\|出力インターフェイス> [<アドミニストレーティブディスタンス値>]|スタティックルートの設定(ネクストホップアドレスは隣接ルータのIPアドレス)|-|
|config|ip route 0.0.0.0 0.0.0.0 <ネクストホップアドレス\|出力インターフェイス> [<アドミニストレーティブディスタンス値>]|デフォルトゲートウェイの設定|-|
|config|ip route <宛先IPアドレス> 255.255.255.255 <ネクストホップアドレス\|出力インターフェイス> [<アドミニストレーティブディスタンス値>]|ホストルート(特定の1台宛のルート)を設定|-|

## 第4章 OSPF

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|router ospf <プロセスID>|OSPF(ダイナミックルーティング)の有効化のためにconfig-routerへ(ID:1-65535を設定可).config→config-router|MODE,1|
|config-router|network <IPアドレス> <ワイルドカードマスク> area <エリアID>|IPアドレスの範囲に含まれるインターフェイスでOSPFを有効化|2|
|config-if|ip ospf <プロセスID> area <エリアID>|インターフェイスごとに個別にOSPFを有効化|-|
|config-router|passive-interface <インターフェイス>|パッシブインターフェイス(Helloパケットを送信しない)の設定|※|
|config-router|router-id <ルータID>|ルータID(形式はIPアドレスと同じ)を手動で設定|※|
|config|interface Loopback <番号>|ループバックインターフェイス(仮想的なインターフェイス)の作成(番号0-2,147,483,647を設定可)|※|
|config-if|ip ospf network <broadcast \|non-broadcast\|point-to-multipoint[non broadcast]\|point-to-point>|インターフェイスのネットワークタイプの変更(p211)|※|
|#|show ip ospf neighbor|ネイバーテーブルの確認|SHOW|
|#|show ip ospf database|LSDBの要約情報(LSAのリスト)であるトポロジテーブルの確認|SHOW|
|#|show ip protocols|OSPFのプロトコルの情報(上のコマンドで設定した内容)の確認|SHOW|
|#|show ip ospf interface [<インターフェイス>]|OSPFが動作しているインターフェイスの情報の確認|SHOW|
|config-if|ip ospf priority <ルータプライオリティ値>|ルータプライオリティの変更|-|
|config-if|ip ospf cost <コスト値>|コスト(OSPFのメトリック)の変更|-|
|config-if|bandwidth <帯域幅>|コストの計算に用いる帯域幅の変更(単位:Kbps)|-|
|config-if|ip ospf hello-interval <秒数>|Helloインターバルの変更(自動でDeadインターバルが4倍の時間に設定される)|-|
|config-if|ip ospf dead-interval <秒数>|Deadインターバルのみを変更|-|
|config-if|ip ospf mtu-ignore|MTUの不一致を検出する機能を無効にする|-|
|config-router|default-information originate [always]|設定したデフォルトルートをOSPFで他のルータに配布するように設定|-|



## 第5章 ACL
### IPv4の標準ACL
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------| 
|config|access-list <ACL番号> <permit\|deny> <送信元IPアドレス> <ワイルドカードマスク>|番号付き標準ACLの作成(ACL番号:1-99,1300-1999)|1,2|
|config|ip access-list standard <ACL名>|名前付き標準ACLの作成.config→config-std-nacl|MODE,1|
|config-std-nacl|[<シーケンス番号>] <permit\|deny> <送信元IPアドレス> <ワイルドカードマスク>|名前付き標準ACLの条件設定(シーケンス番号:任意or指定がないと10,20,30,...)|2|
|config-if|ip access-group <ACL番号\|ACL名> <in\|out>|作成した標準ACLを(宛先に近い)インターフェイスに適用|※,(3)|
|config-line|access-class <ACL番号\|ACL名> <in\|out>|リモートアクセス制御の設定|※,(3)|
|#|show access-lists [<ACL番号\|ACL名>]|作成されたACLの確認|SHOW,※C|
|config|no access-list [<ACL番号\|ACL名>]|ACLの削除.番号付きACLはすべてのエントリが消えるので注意|-|

### IPv4の拡張ACL
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------| 
|config|access-list <ACL番号> <permit\|deny> <プロトコル> <送信元IPアドレス> <ワイルドカードマスク> [<送信元ポート番号>] <宛先IPアドレス> <ワイルドカードマスク> [<オプション>]|番号付き拡張ACLの作成.(ACL番号:100-199,2000-2699)(プロトコル:ip,tcp,udp,icmp等)|1,2|
|config|ip access-list extended <ACL名>|名前付き拡張ACLの作成.config→config-ext-nacl|MODE,1|
|config-ext-nacl|<シーケンス番号> <permit\|deny> <プロトコル> <送信元IPアドレス> <ワイルドカードマスク> [<送信元ポート番号>] <宛先IPアドレス> <ワイルドカードマスク> [<オプション>]|拡張名前付きACLの条件設定|2|
|config-if|ip access-group <ACL番号\|ACL名> <in\|out>|作成した標準ACLを(送信元に近い)インターフェイスに適用|※,(3)|

|プロトコル|オプション例|説明|
|---------|-----------|----|
|ip|dscpなど|IPに関する項目|
|tcp|eq 80など|eq(等しい)、neq(等しくない)、It(小さい)、gt(大きい).数字はポート番号で名称でも可|
|udp|eq 161など|tcpと同様の指定の仕方|
|icmp|echo(エコー要求), echo-reply(エコー応答)など|ICMPのメッセージのタイプ|


## 第6章 NAT・DHCP・DNS
### NAT

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config-if|ip nat <inside\|outside>|NATでルータのインターフェイスにインサイドかアウトサイドを指定|※,1|
|config|ip nat inside source static <内部ローカルアドレス> <内部グローバルアドレス>|NATテーブルにアドレスの組み合わせを登録(スタティックNAT,手動)|(2)|
|#|show ip nat translations|NATテーブルの確認|SHOW|
|#|show ip nat statistics|NATによるアドレス変換の統計情報の確認|SHOW|
|#|debug ip nat|NATの変換情報のリアルタイム確認|
|config|ip nat pool <プール名> <開始アドレス> <終了アドレス> <netmask <サブネットマスク>\|prefix-length <プレフィックス>|ダイナミックNATでアドレスプールを作成する(同時に、変換の対象となる内部ローカルアドレスのリストをACLで作成しておく)|(2)|
|config|ip nat inside source list <ACL> pool <プール名>|内部ローカルアドレスのリストとアドレスプールを紐づける|3|
|config|ip nat inside source list <ACL> <pool <プール名> \| interface <インターフェイス>> overload|NAPT(PAT)で内部ローカルアドレスとアドレスプールまたは外部インターフェイスを紐づける(overloadを忘れない)|※C,3|
|#|clear ip nat translation *|NATテーブルの削除|-|

### DHCP
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|ip dhcp pool <プール名>|DHCPサーバ設定のためのアドレスプール作成.config→dhcp-config|MOVE,1|
|dhcp-config|network <ネットワーク> <サブネットマスク\|プレフィックス>|DHCPプールで使用(配布)するIPアドレスのネットワークとサブネットマスクを指定|2|
|dhcp-config|default-router <IPアドレス>|DHCPクライアントにデフォルトゲートウェイのIPアドレスを設定|3|
|dhcp-config|lease <日にち> [<時間>[<分>]]|リース期間(IPアドレスを貸し出す期間)の指定(半分を過ぎるとクライアントが延長を要求)|4|
|dhcp-config|dns-sever <DNSサーバのIPアドレス1> [<DNSサーバのIPアドレス2>]|DNSサーバの指定.左のDNSサーバから順に優先度が高い|5(not必須)|
|dhcp-config|domain-name <ドメイン名>|ドメイン名の指定|※,6(not必須)
|config-if|ip address dhcp|DHCPクライアントに設定|-|
|#|show ip dhcp pool|DHCPサーバで設定したアドレスプールの確認|SHOW|
|#|show ip dhcp binding|DHCPサーバによるIPアドレスの割当状況の確認|SHOW|
|#|show ip dhcp conflict|コンフリクトが発生したIPアドレスの一覧を確認|SHOW|
|#|clear ip dhcp conflict *|コンフリクトによって割当不可となったIPアドレスを情報を消去して再度配布できるようにする|-|
|#|show dhcp lease|DHCPサーバのIPアドレスやリース期間などのより詳しい内容を表示|SHOW|
|config-if|ip helper-address <DHCPサーのIPアドレス>|DHCPリレーエージェント(異なるネットワーク上に配置されたDHCPのメッセージを転送する機能)の設定|-|

### DNS
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|ip domain-lookup|DNSルックアップの有効化(DNSクライアントとして動作)|-|
|config|ip name-server <DNSサーバのIPアドレス1> [<DNSサーバのIPアドレス2>]|DNSサーバのIPアドレスの設定(最大6つまで)|-|
|config|ip host <ホスト名> <IPアドレス1> [<IPアドレス2>]|手動でホスト名とIPアドレスの組み合わせを登録|-|
|config|ip domain-name <ドメイン名>|手動でドメイン名の設定|-|
|#|show hosts|ルータが保持している静的・動的なDNS情報の確認|SHOW|


## 第7章 Catalystスイッチの基本設定とVLAN

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|interface vlan 1|管理インターフェイスでconfig-ifに移動するconfig→config-if.(デフォルトはvlan1が管理VLAN).IPアドレスを設定することも可.|MODE|
|config|ip default-gateway <デフォルトゲートウェイのIPアドレス>|スイッチでデフォルトゲートウェイを設定|-|
|config|vlan <VLAN番号>|VLANの作成.config→config-vlan(VLAN番号:1-4094(標準VLANなら1-1001を指定))|MODE,1|
|config-vlan|name <VLAN名>|VLANに名前を設定(指定しないとVLANxxxx(xxxxはVLAN番号))|2|
|config-vlan|<exit\|end>|config-vlanを終了することで設定を確定|3|
|config|no vlan <VLAN番号>|VLANの削除|-|
|config|interface <インターフェイス>|アクセスポートの設定のためにconfig-ifに|1|
|config-if|switchport mode access|インターフェイスのモードをアクセスに設定|※,2|
|config-if|switchport access vlan <VLAN番号>|所属するVLANを指定|-|
|config|interface <インターフェイス>|トランクポートの設定のためにconfig-ifに|1|
|config-if|switchport trunk encapsulation <dot1q\|isl>|トランキングプロトコルの種類を設定(ISLがサポートされていないなら省略)|2|
|config-if|switchport mode trunk|インターフェイスのモードをトランクに設定|3|
|config-if|switchport trunk native vlan <VLAN番号>|インターフェイスのネイティブVLAN(タグをつけずにそのまま送信する相手)を変更する(任意)|4|
|config-if|switchport trunk allowed vlan [add\|all\|none\|except\|remove] <VlAN番号>|許可VLANを変更.オプションを指定しないと指定したVLANのみ許可.オプションは現状から指定した内容に従って更新|5|
|config-if|switchport mode dynamic <auto\|desirable>|DTPのネゴシエーションで自動でアクセスかトランクか決定させる|-|
|config-if|switchport nonegotiate|DTPフレームの停止(相手が手動でtrunkモードにしているとき等)|-|
|config-if|switchport voice vlan <VLAN番号>|音声VLANの設定(アクセスポートとして設定される)|-|
|#|show vlan [brief]|現在スイッチで作成されているVLANの確認.トランクポートは表示されない|SHOW|
|#|show vlan id <VLAN番号>|特定のVLANの情報を確認.トランクポートも表示される|SHOW|
|#|show interfaces [<インターフェイス>] trunk|トランクポートとなっているインターフェイスをすべて表示|※C,SHOW|
|#|show interfaces [<インターフェイス>] switchport|現在のスイッチポートの状態を確認|SHOW|
 |#|show interfaces status|スイッチのインターフェイスの状態を確認|SHOW|
 |#|show mac address-table|スイッチのMACアドレステーブルを確認.macとaddressの間は-でも可|SHOW|
 config|vtp domain <ドメイン名>|VTPを有効化してVTPドメイン名を設定.同期を行うスイッチ間で一致の必要あり|1|
 |config|vtp mode <server\|client\|transparent>|VTPの動作モードを変更|2|
 |config|vtp version <VTPバージョン>|VTPのバージョンを指定(VTPバージョン:1-3)同期を行うスイッチ間で一致の必要あり|3|
 |#|show vtp status|VTPの設定を確認|SHOW|
 ### VTP(ルータ側)

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
 |config|interface <インターフェイス>.<論理番号>|ルータにサブインターフェイス(仮想のインターフェイス.VLAN間ルーティング等で用いる)の作成.config→config-subif|MODE,1|
 |config-subif|encapsulation <dot1q\|isl> <VLAN番号> [native]|サブインターフェイスのトラッキングプロトコルの指定.スイッチと一致させる.ネイティブVLAN用のサブインターフェイスのときはnativeを入力|2|
 ### レイヤ3スイッチ
 |モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
 |config|interface vlan <VLAN番号>|VLAN用のSVI(スイッチの仮想のインターフェイス)を作成.config→config-if.この後IPアドレスを設定する|MODE|
 |config|ip routing|ルーティングの有効化|※|
|config-if|no switchport|ルーテッドポート(レイヤ3スイッチの物理ポートをルータのインターフェイスとして動作)|-|
|config-if|switchport|ルーテッドポートをスイッチポートに戻す|-|
 

## 第8章 STP
### STP
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|#|show spanning-tree [vlan <VLAN番号>]|STPの状態を確認|SHOW|
|#|show spanning-tree interface <インターフェイス>|STPが動作しているインターフェイス(ポート)の状態を確認|-|
|#|debug spanning-tree events|STPの動作の確認を開始する|-|
|#|no debug spanning-tree evets|STPの動作の確認の停止|-|
|config|spanning-tree vlan <VLAN番号> priority <プライオリティ>|ブリッジプライオリティの変更(ルートブリッジを変更するため)(プライオリティ:4096の倍数,最小値0,表示はVLAN番号を足した数字となる)|-|
|config|spanning-tree vlan <VLAN番号> root primary|ダイナミックにルートブリッジを選出(指定したVLAN上のルートブリッジよりも4096小さい値をプライオリティに)|-|
|config-if|spanning-tree vlan <VLAN番号> cost <パスコスト値>|VLANごとにパスコスト値を指定|※|
|config|spanning-tree pathcost method <shot\|long>|パスコストのデフォルト値の変更。100Gbps以上の帯域のリンクを使用しているならロング法推奨(ショート法はすべて1になる)|-|
|config-if|spnanning-tree vlan <VLAN番号> port-priority <プライオリティ>|ポートIDのポートプライオリティの変更|

### PortFast,BPDUガード,ルートガード,タイマー,RSTP

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|spanning-tree portfast default|PortFastをアクセスポートすべてで有効にする|※|
|config-if|spanning-tree portfast [trunk]|インターフェイスごとにPortFastを有効に(トランクポートはtrunkをつける)|-|
|config|spanning-tree portfast bpduguard default|PortFastの設定が行われているポートすべてでBPDUガードを有効化|※|
|config-if|spanning-tree bpduguard <enable\|disable>|インターフェイスでBPDUガードを有効/無効にする|-|
|config-if|spanning-tree guard root|ルートガード(既存のルートブリッジよりブリッジIDが小さなスイッチが接続されてもSTPのトポロジを変えない)の有効化|※|
|config-if|no spanning-tree guard root|ルートガードの無効化|-|
|config|spanning-tree vlan <VLAN番号> hello-time <秒数>|Helloタイマー(BPDUを送信する時間)を変更.(デフォルト:2秒)|-|
|config|spanning-tree vlan <VLAN番号> max-age <秒数>|最大エージタイマー(最後に受け取ったBPDUを保持する期間)の変更(デフォルト:20秒)|-|
|config|spanning-tree vlan <VLAN番号> forward-time <秒数>|転送遅延タイマー(リスニングとラーニングの状態に留まる時間)の変更(デフォルト:15秒)|-|
|config|spanning-tree mode <pvst\|rapid-pvst>|動作モードをSTPからRSTPに変更(デフォルト:PVST+)|-|


## 第9章 EtherChannel

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config-if|channel-group <ループ番号> mode <on\|auto\|desirable\|active\|passive>|EtherChannelを形成する.グループ番号は字スイッチ内でそろえる必要はあるが、対向のスイッチと合わせる必要はない.PAgP:Auto/desirable, LACP:active/passive|-|
|config-if|channel-protocol <lacp\|pagp>|ネゴシエーションを行うプロトコルを指定.上のコマンドで自動で決まるので実行しなくてもいいが、先に実行することでミスを減らせる|-|
|config|port-channel load-balance <負荷分散方法>|EtherChannelでの負荷分散の方法を変更.<負荷分散方法>でsrcなら送信元,dstなら宛先で、後ろは何を基に負荷分散するかを表す|-|
|config|interface range <インターフェイス>|複数のポートに同じ設定をまとめて行う.config→config-if-range|MODE|
|config-if|port-channel min-links <最小ポート数>|LACPでPort-chanelインターフェイスがリンクアップするための最小リンク数を指定|-|
|#|show etherchannel [<summary\|detail>]|EtherChannelの情報の確認|SHOW|
|#|show etherchannel load-balance|現在EtherChannelで設定されている負荷分散方法の設定|SHOW|
|#|show <pagp\|lacp> neighbor|隣接するスイッチのポートの各種情報を確認|SHOW|

## 第10章 IPv6

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|ipv6 unicast-routing|ルータでIPv6ルーティングを有効化|-|
|config-if|ipv6 enable|インターフェイスでIPv6を有効にしてリンクローカルユニキャストアドレスを自動で設定|-|
|config-if|ipv6 address <アドレス> link-local|手動でリンクローカルユニキャストアドレスを設定(アドレス:FE80で始まる)|-|
|config-if|ipv6 address <アドレス>/<プレフィックス長> [eui-64]|グローバルユニキャストアドレスを設定.インターフェースIDをEUI-64形式のアドレスで設定する場合eui-64が必要|-|
|config-if|ipv6 address dhcp|ルータのインターフェイスをDHCPv6クラアントとして動作させ，DHCPv6サーバからアドレスを取得|-|
|#|show ipv6 <インターフェース>|インターフェイスのIPv6アドレスを確認|SHOW|
|config|ipv6 route <プレフィックス>/<プレフィックス長> <ネクストホップアドレス\|出力インターフェイス> [<アドミニストレーティブディスタンス値>]|IPv6でのスタティックルートの設定|-|
|config|ipv6 route ::/0 <ネクストホップアドレス\|出力インターフェイス> [<アドミニストレーティブディスタンス値>]|IPv6でデフォルトルートの設定|-|
|#|show ipv6 route|IPv6のルーティングテーブルを確認|SHOW|


## 第11章 その他のIPサービスと運用
### HSRP
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config-if|standby [<スタンバイグループ番号>] ip [<仮想IPアドレス>]|HSRPの有効化(スタンバイグループ番号:0-255,省略したら0)(仮想IPアドレスはルータに設定されているIPアドレスは設定できない)|-|
|config-if|standby [<スタンバイグループ番号>] priority <プライオリティ値>|HSRPプライオリティ値の変更(プライオリティ値:0-255,デフォルト100)|-|
|config-if|standby [<スタンバイグループ番号>] preempt|プリエンプト(常にプライオリティ値の大きいルータをアクティブルータとする機能)の有効化|-|
|config-if|standby [<スタンバイグループ番号>] track<インターフェイス> [<減少値>]|インターフェイストラッキング(インターフェイスがdownすると<減少値>だけHSRPプライオリティ値を減少させる機能)を有効化(減少値:デフォルト10)|-|
|#|show standby brief|HSRPの要約情報を確認|SHOW|
|#|show standby|HSRPの詳細情報を確認|SHOW|
|-|arp -a|WindowsでクライアントPCのARPテーブルを確認(pingコマンドとtracertコマンド実行後)|(1)|

### SNMP
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|snmp-server view <ビュー名> <MIB名> <include\|exclude>|SNMPビュー(特定のMIB情報だけをSNMPマネージャから取得させる/させない機能)を作成|-|
|config|snmp-server community <コミュニティ名> [view <ビュー名>] [ro\|rw] [<ACL>]|コミュニティ名を設定(認証のため).コミュニティ名はSNMPマネージャとSNMPエージェントで合わせておく.ACLオプションでACLのpermitで指定されているアドレスの要求のみ受け付ける|-|
|config|snmp-server host <IPアドレス> [traps\|informs] [version <1\|2c\|3 <auth \|noauth\|priv>>] <コミュニティ名\|ユーザ名>|SNMPの通知機能の設定のためにSNMPマネージャの指定.(IPアドレス:送信先).通知の種類はSNMPv1はtrapsのみ,SNMPv2cやSNMPv3はinformsも可.v3はセキュリティレベルも指定.(v1,v2c→コミュニティ名,v3→ユーザ名.)|1|
|config|snmp-server enable traps [<トラップ対象>]|SNMPトラップ、SNMPインフォームを両方有効化.(トラップ対象:省略するとすべてが対象)|2|
|config|snmp-server group <グループ名> v3 <auth\|noauth\|priv> [read <ビュー名>] [write <ビュー名>] [access <ACL>]|SNMPv3でグループ単位で設定するためにSNMPグループを作成|1|
|config|snmp-server user <ユーザ名> <グループ名> v3 [auth <md5\|sha> <パスワード> [priv <des\|3des\|aes <128\|192\|256>>]]|SNMPユーザをSNMPグループと関連付けながら作成|2|
|#|show snmp view|SNMPビューの確認|SHOW|
|#|show snmp group|SNMPグループの確認|SHOW|
|#|show snmp user|SNMPユーザの情報の確認|SHOW|


## 第12章 デバイスの管理
### システムログ
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|logging console <レベル>|コンソールに出力されるシステムログのレベルを変更(レベル:数値orキーワード,デフォルト7)|-|
|config|no logging console|コンソールに出力するシステムのログをすべて停止|-|
|#|terminal monitor|VTY接続でログインした際に仮想ターミナルラインにシステムログを出力させる|※|
|#|terminal no monitor|仮想ターミナルへのシステムのログの出力を停止|※,※C|
|config|logging monitor <レベル>|仮想ターミナルラインに出力するシステムログのレベルを変更する|-|
|config|logging buffered <サイズ>|ルータやスイッチのRAMにシステムログを保存するために、サイズを指定して保存領域を確保(サイズ:4096-,単位byte)|-|
|config|logging buffered <レベル>|出力するシステムログのレベルを変更する(レベル:0-7)|-|
|config|logging host <IPアドレス\|ホスト名> [transport <tcp\|udp>] [port <ポート番号>]|Syslogサーバにシステムログを送信するためにサーバを指定.デフォルトでUDPポート番号514だがオプションで変更できる|-|
|config|logging trap <レベル>|Syslogサーバへ送信するシステムログのレベルを変更する|-|
|config|service timestamps <debug\|log> [<dateime [localtime] [msec] [show-timezone] [year]\|uptime]|デバッグやログで表示されるタイムスタンプ(先頭の時間表記)を変更する(datetime:月/日/時/分/秒,uptime:ルータ起動からの経過時間)(localtime:機器本体に設定したタイムゾーン)|-|
|config|service sequence-numbers|表示されるメッセージにシーケンス番号を付加する|-|
|config-line|logging synchronous|ログ表示時に自動的に改行させるように設定|※|
|#|show logging|ルータやスイッチのRAMに保存しているシステムログの確認|SHOW|
|#|debug <表示したいデバッグメッセージ>|通常のシステムログを表示させるコマンドでは表示されないデバッグメッセージを表示|-|
|#|debag ip ospf hello|OSPFのHelloパケットの送受信状況を表示(上の例)|-|
|#|undebug all/no debug all|すべてのデバッグメッセージの停止|-|

### NTP
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|ntp server <IPアドレス> [prefer]|NTPクライアントとしてNTPサーバと時刻を同期|-|
|config|ntp master [<stratum値>]|他のNTPサーバと同期できない場合でもstratum値の階層のNTPサーバとして自身のハードウェアクロックを参照して時刻を提供する(stratum値:1-15,デフォルト8)|-|
|config|ntp authenticate|NTPの認証機能を有効化|1|
|config|ntp authentication-key <鍵番号> md5 <文字列>|認証鍵を定義(鍵番号:任意)(文字列:8文字まで).NTPサーバとNTPクライアント間でどちらも共通にする|2|
|config|ntp trusted-key <鍵番号>|NTP認証で使用する鍵番号の指定|3|
|config|ntp server <IPアドレス> key <鍵番号> [prefer]|時刻同期するNTPサーバに対して鍵番号を指定することで認証を行う(NTPクライアントのみで実行)|4|
|config|clock timezone <タイムゾーンの略称> <時差>|タイムゾーンの設定(日本:JST 9,デフォルト:UTC)|-|
|#|clock set <時:分:秒> <日> <月> <年>|手動で時刻を設定|-|
|#|show clock|ルータ内の現在時刻を表示|SHOW|
|#|show ntp status|自身のNTPの設定の確認|-|
|#|show ntp associations|NTPの時刻同期の状態の確認|-|

### CDP・LLDP
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|[no] cdp run|機器単位でCDPの有効化/無効化(デフォルト:有効)|-|
|config-if|[no] cdp enable|CDPインターフェイス単位で有効化/無効化.グローバルに無効化されている状態では有効化できない|-|
|#|show cdp|CDPが有効かどうか確認|SHOW|
|#|show cdp interface|CDPが動作するインターフェイスの情報を確認|SHOW|
|#|show cdp neighbors|CDPで隣接機器から受信した情報の要約情報を確認|SHOW|
|#|show cdp neighbors detail|CDPで隣接機器から受信した情報の詳細情報を確認|SHOW|
|#|show cdp entry <*\|機器名>|特定の機器のみの詳細情報を確認|SHOW|
|config|lldp run|LLDPの有効化(デフォルト:無効)|-|
|config-if|lldp <transmit\|receive>|インターフェイス単位でLLDPを有効化(transmit:LLDPフレームの送信,receive:LLDPフレームの受信)|-|
|config|lldp timer <秒数>|LLDPフレームの送信間隔の変更(デフォルト:30s)|-|
|config|lldp holdtime <秒数>|取得した情報を削除するまでの時間(デフォルト:120s)|-|
|config|lldp reinit <秒数>|再初期化実行までの遅延時間(初期化を再び実行するまでのクールタイム)(デフォルト:2s)|-|
|config|lldp tlv-select <TLV名>|送信する自身の情報(TLV形式:Type,Length,Value)の項目を変更(p642)|-|

### IOSの管理とその他の管理機能
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|#|show flash|フラッシュメモリに保存されているファイルの確認.スイッチならvlan.datも|SHOW|
|#|show version|IOSに関する情報や容量、コンフィギュレーションレジスタの値やルータについているインターフェイスなどを確認|SHOW|
|#|copy <コピー元> tftp:|TFTPサーバを利用してバックアップを行う際に、サーバにバックアップをアップロードするコマンド|※C,(1)|
|#|copy startup-config running-config|パスワードリカバリを行う際に、一度設定ファイルの内容をルータに反映させる|-|
|config|config-register <コンフィギュレーションレジスタ値>|コンフィギュレーションレジスタ値の変更.通常起動は0x2102,startup-configを読み込まないようにするには0x2142|(2)|
|#|copy running-config startup-config|変更した設定の保存|(3)|
|#|telnet <IPアドレス\|ホスト名>|ルータやスイッチから他の機器にTELNET接続|-|
|#|ssh -l <ユーザ名> <IPアドレス\|ホスト名>|ルータやスイッチから他の機器にSSH接続|-|
|#|exit/logout|リモート接続の終了|-|
|-|Ctrl + Shift + 6 → X|接続を維持したまま元のデバイスに戻る|-|
|#|show session|現在存在するリモートセッションの確認|SHOW|
|#|resume [<セッション番号>]|中断しているセッションを再開(セッション番号:入力しなければ直前に中断したセッション)|-|
|#|disconnect <セッション番号>|中断しているセッションを切断|-|
|#|show users|ログインしてきているユーザを確認|SHOW|
|config|banner motd <区切り文字>|バナーメッセージを設定(区切り文字:メッセージを終了する際に入力する文字)|-|


## 第13章 ネットワークアーキテクチャ

|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config-if|power inline <auto\|never\|static>|PoEの動作モードの変更.autoはネゴシエーションで自動で電力を判断.staticはあらかじめ設定した範囲内で即座に電力値を決定|-|


## 第14章 セキュリティ
### ネットワークデバイスの保護
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|enable algorithm-type <md5\|scrypt\|sha256> secret <パスワード>|イネーブルパスワードの暗号化のアルゴリズムを指定.(md5:従来のenable secret)|-|
|config|username <ユーザ名> [privilege <特権レベル>] secret <パスワード>|ローカル認証に使用するユーザアカウントパスワードの暗号化(MD5アルゴリズム).ユーザ名1つに対して平文形式か暗号化形式かどちらかしか設定できない(同じ形式のパスワードを上書きは可能)|-|
|config|username <ユーザ名> [privilege <特権レベル>] algorithm-type <md5\|scrypt\|sha256> secret <パスワード>|ユーザアカウントパスワードをアルゴリズムを指定して暗号化|-|

### スイッチのセキュリティ機能
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config-if|switchport port-security|ポートセキュリティの有効化(事前に手動でアクセスポートかトランクポートに設定する必要あり)|(1)|
|config-if|switchport port-security maximum <最大値>|インターフェイスに登録できるセキュアMACアドレスの最大数の変更(デフォルト:1)|-|
|config-if|switchport port-security mac-address <MACアドレス>|セキュアMACアドレスを手動で登録(登録可能な最大数に達していない場合に限り)(デフォルトで自動的に学習するようにはなっている)|-|
|config-if|switchport port-security mac-address sticky|スティッキーラーニング(自動登録されたセキュアMACアドレスをrunning-configに残るように)を有効化|-|
|config-if|switchport port-security violation <restrict\|protect\|shutdown>|セキュリティ違反が起きたときのモードを設定(デフォルト:shutdown)
|#|show port-security|ポートセキュリティの設定(登録されたセキュアMACアドレスは×)の確認|SHOW|
|#|show port-security address|登録されたセキュアMACアドレスを確認|※,SHOW|
|#|show port-security interface <インターフェス>|インターフェイスごとにポートセキュリティの設定を確認|※,SHOW|
|config|ip dhcp snooping|DHCPスヌーピング(スイッチの各ポートをtrustとuntrustに分ける)の有効化(VLAN単位では無効となっている)|-|
|config|ip dhcp snooping vlan <VLAN番号>|DHCPスヌーピングを有効にするVLANの指定(,や-でまとめて指定可)|-|
|config-if|[no] ip dhcp snooping trust|信頼できる/できないポートの指定(デフォルト:untrust)|-|
|config|[no] ip dhcp snooping information option|リレーエージェント情報オプション(オプション82)(特定のポートに接続された機器に特定のIPアドレスの割当などの制御を可能にする)の有効化/無効化(デフォルト:有効)|-|
|config-if|ip dhcp snooping limit rate <レート>|DHCPパケットの最大受信レート(1s当たりのDHCPパケットの最大数)を指定|-|
|#|show ip dhcp snooping|DHCPスヌーピングの設定情報確認|SHOW|
|#|show ip dhcp snooping binding|DHCPスヌーピングバインディングデータベース(PCに割り当てられたIPアドレスなどの情報が格納されている)の確認|SHOW|
|config|ip arp  inspection vlan <VLAN番号>|ダイナミックARPインスペクション(スイッチのポートをtrustとuntrustに分ける)を有効化(すべてのポートがuntrustに)|1|
|config-if|ip arp inspection trust|信頼できるポートに設定|(2)|
|config|vlan dot1q tag native|ネイティブVLANでもタグを付加されるように設定(ダブルタギング攻撃対策)|-|

### AAA
|モード|コマンド|説明|分類、補足|
|-----|--------|---|---------|
|config|aaa new-model|AAAを有効化|1|
|config|aaa authentication login <default\|リスト名> <認証方式1> [<認証方式2>]|ログイン時の認証方式の設定(方式によってはあらかじめパスワードを設定したり、サーバを事前構築しておく必要あり)(default:すべてのログイン絶族で使用される)|2|
|config-line|login authentication <default\|リスト名>|認証方式リストを適用|※,(3)|

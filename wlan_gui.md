# WLANs
* 初めにWLANに割り当てるProfile NameとSSIDを設定
## General
* Status : WLANの有効化/無効化
* Radio Policy : 使用するIEEE 802.11規格を選択
* Interface/Interface Group(G) : 紐づけるインターフェイスを選択
* Broadcast SSID : SSIDブロードキャストを有効化(チェックを外すとステルスモードに)

## Security
### Layer2
* AutoConfig iPSK : iPSK(同一SSIDに対して複数の異なる事前共有鍵を作成)の有効化(RADIUSサーバとWLCを連携させておく必要あり)
* MAC Filtering : MACフィルタリングの有効化(WLANごとに設定可能)

#### Protected Management Frame
正当なクライアントの関連付けが解除されてしまう問題を防ぐ
* PMF
    * Required : PMFをサポートしていないクライアントの接続を許可しない
    * Optional : PMFをサポートしていないクライアントの接続を許可する
* Comebackタイマー：関連付け要求を拒否されたクライアントが再度要求を試行するまでに待機する時間
* SAクエリタイムアウト：WLCがSAクエリプロセス(なりすましのアソシエーション要求を拒否するために正当なクライアントとWLCの間で行われるやり取り)の応答を待機する時間

#### WPA + WPA2 Parameter
* WPA [WPA2]の設定
    1. Layer 2 Security選択ボックスから「WPA+WPA2」を選択
    1. WPA+WPA2 Parametersの「WPA Policy」 [「WPA2 Policy」]にチェック
#### Authentication Key Management
* PSK Format:事前共有鍵
    * ASCII(8-63文字の英数字:default) or 
    * HEX(64桁の16進数)

* 802.1X : RADIUSサーバなどの認証サーバでキーを管理する
* CCKM(Cisco Centralized Key Management) : ローミング時間の短縮を実現する、Cisco独自のキーの再生成技術。802.1Xと組み合わせて使用することが可能
* FT 802.1X, FT PSK : FT(Fast Transition)を有効にする場合に選択

### Layer3
* Layer3セキュリティ : Web PolicyにすることでWeb認証の有効化

### AAA Severs
* WLANとRADIUSサーバを連携

## QoS
* QoSの指定

## Advanced
DHCPなどの設定
* FlexConnect Local Switching : FlexConnectモードでローカルスイッチング(アクセスポートが独立して動作)の有効化
* Allow AAA Override : RADIUSサーバが持つユーザ情報に基づいてユーザを特定のVLANに動的に割り当てる
* Enable Session Timeout : セッションのタイムアウトするまでの時間の設定(単位:s)
* Maximum Allowed Clients : 無線LANクライアントの最大同時接続数の設定
* Maximum Allowed Clients Per AP Radio : APの周波数帯ごとの同時接続数の設定
* Wi-Fi Direct Clients Policy : デバイス同士が無線LANルータやアクセスポイントを介さずに直接接続（P2P接続）するための通信方式に関する設定
    * Disabled : ポリシーを無効化. 判定なしで参加許可
    * Allow : デバイスがWi-Fi Directの機能を使用していることを判定した上でWLANに参加することを許可
    * Not-Allow : すべてのP2P対応デバイスがWLANに参加することを拒否する

### Load Balancing and Band Select
* Load Balancing : 
* Client Band Select : デュアルバンド対応のクライアントがアクセスしてきた場合は、5GHz帯の周波数を優先的に使用する

# Controller
## General
* LAG Mode on next reboot : リンクアグリゲーションの有効化


## Interface
* Newを押してDynamicインターフェイスを追加できる(Vlan番号、使用するポート、IPアドレスなどを設定)

# WIRELESS
## General
* AP Mode : LAPのモード(BridgeやSnifferなど)を変更

# SECURITY
## AAA
### Radiusサーバの指定
#### Authentication
* Shared Secret Format : 事前共有キーをASCIIかHexから指定
* Network User : 無線LANクライアントが接続する際にRADIUSによる認証を行うように
* Managemaet : ワイヤレスLANコントローラ事態への管理ログイン時にRADIUSによる認証を行うように
### Local Net Users
ローカルEAP認証に使用するユーザの作成。クライアントがAPに接続するとき、ここで設定したユーザ名とパスワードで認証を行う
* Guest UserクライアントとAPの接続に制限時間を設けることができる

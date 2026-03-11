# WLANs
## General
* 全般情報の確認と WLANの有効化/無効化、WLANとインターフェースのマッピング

## Security
### Layer2
* AutoConfig iPSK : iPSK(同一SSIDに対して複数の異なる事前共有鍵を作成)の有効化(RADIUSサーバとWLCを連携させておく必要あり)

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
* Web認証の有効化/無効化

### AAA Severs
* WLANとRADIUSサーバを連携

## QoS
* QoSの指定

## Advanced
* DHCPなどの設定


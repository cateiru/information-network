# 1回目

## WireShake how to

**詳しくはggれ！！！**

### キャプチャフィルタ

```text
[プロトコル名] [ディレクション] [タイプ] [値]
```

- プロトコル名
  - プロトコルやパケットの種類
- ディレクション
  - 送信元/宛先
- タイプ
  - IPアドレスやネットワーク、ポート
- 値
  - プロトコル名やディレクション、タイプに応じた値

#### 例

1. `ether src 00:0c:29:ce:90:da`
   - 送信元MACアドレスが「00:0c:29:ce:90:da」のEthernetフレーム
2. `ether host 00:0c:29:ce:90:da`
   - 送信元MACアドレスか宛先MACが「00:0c:29:ce:90:da」のEthernetフレーム
3. `ether proto 0x0800`
   - イーサネットタイプが「0x0800」のEthernetフレーム
4. `arp`
   - ARPフレーム
5. `ip src host 192.168.1.1`
   - 送信元IPアドレスが「192.168.1.1」のIPv4パケット
6. `ip host 192.168.1.1`
   - 送信元IPアドレスか宛先IPアドレスが「192.168.1.1」のIPv4パケット。
   - `host 192.168.1.1`も同じ意味
7. `ip src net 192.168.1.0/24`
   - 送信元IPアドレスが「192.168.1.0/24 (192.168.1.0 - 192.168.1.255)」のIPv4パケット。
   - `ip src net 192.168.1.0 255.255.2255.0`も同じ意味
...

## 導入

- 人間が決めるのでベストであるかどうかわからない
  - BCP (Best Current Practice)
- インターネットを学ぶ上で第一次資料は**RFC標準** と **(参照)実装**
- 規約を標準にするための参照実装(Reference implementation (code))
  - `Rough Consensus, Running Code`
  - ひどい規約もある

## OSI 7層モデルとTCP/IP概要

### インターネットとは

- inter-network
  - *"異なる種類の**物理NW**相互接続*
  - Open Systems Interconnection (OSI: 開放型システム間相互接続)の一例

#### RFC640, 606, 675 internetworking

- OSI標準
  - ISOとITU-Tで議論された
  - 仕様は公開
  - 仕様は肥大化
  - 仕様がシンプルなTCP/IPに負けた

### OSIとTCP/IP

- OSI(Open System Interconnection)
  - ISOとITU-Tによって規格化された国際標準化プロトコル
    - ISO: International Standardization Organization
    - ITU-T: International Telecommunication Union Telecommunication Standardization Sector
- TCP/IP
  - 米国国防総省の標準プロトコルとして規格化
  - デファクトスタンダード

### OSI

- 異種ネットワーク同士の接続を可能とする枠組み
- 階層化されたプロトコル構造
  - プロトコルは各層（各モジュール）ごとに独立
  - 上位の階層より下位の階層のサービスを利用できるインターフェイスが規定
  - →これらのアイデアがTCP/IPを中心とする現在のインターネット技術でも引き継がれている

### 階層化とは

- 各機能をその働きごとに分割する: 役割分担の一種
  - 責任範囲の限定
    - 各階層は自分の階層の仕事のみに責任をもつ
    - 自分の上下の階層とのみやりとりを行う
  - 各階層が独立
    - 共通インターフェイス
    - 同一レベルの階層同士を交換できる

### OSI7層モデル

- 第7層: アプリケーション層
  - 具体的な通信サービス
- 第6層: プレゼンテーション層
  - データの表現方法
  - フォーマット、変換(圧縮、暗号化)
- 第5層: セッション層
  - コネクション（データが流れる論理的な通信路）の開始から終了までの手順
- 第4層: トランスポート層
  - 両端のノード間の通信管理（エラー訂正、再送制御等）
  - 信頼性を閣法する仕組みを提供
- 第3層: ネットワーク層
  - 論理アドレスに基づき、2つのホスト間の通信経路を決定
- 第2層: データリンク層
  - 物理アドレスに基づき、セグメント（隣接するノードたち）内での通信を確保
- 第1層: 物理層
  - 電気的、機械的に定義された"物理リンク"を提供

## TCP/IP概要

### Internet

- (Defense) Advanced Research Projects Agency((D)ARPA)から資金
  - DARPA: 国防総省計画局
  - ARPA: 高等研究計画局
- 大規模な資金
  - NSF: National Science Foundation
  - DoD: the U.S. Department of Defence
  - DoE: the U.S. Department of Energy
- 呼び方: ARPA/NSF Internet, TCP/IP Internet, global internet, 単にInternet

### TCP/IP技術の概要

- インターネットのプロトコルは標準として記述
  - 構文規則、解釈規則、メッセージフォーマット、処理
- 高いレベルの抽象化、下位レベルの仕組みと独立に記述
  - 下位レベルを知る必要なし→生産性向上
  - 特定のネットワークハードウェアに依存しない、独立→ハードウェア変更可能

### TCP/IP技術の特徴

- コネクションレス、パケット配送サービス（connectionless packet derivery service）
  - **パケット**交換ネットワーク
  - パケット内アドレス情報に基づき経路制御
  - 信頼性なし、効率的
- 信頼性のある**ストリーム**配送サービス（reliable stream transport service）
  - 計算機上のアプリケーション間で**コネクション**を確立
  - 大量のデータ送信

--

- 物理ネットワーク技術と独立
  - 特定のハードウェアと独立
  - 小規模ネットワークから大規模ネットワークまで可能
  - **データグラム**というデータ転送単位
- 普遍的相互接続
  - すべてのマシン間
  - すべてのマシンに**アドレス**を割り当てる
  - 各データグラムに始点アドレス、終点アドレスを持つ
- End to endの**応答可能**
  - 異なるネットワークでも可能
  - 始点と終点間のみ（途中ではやらない）

## パケット交換ネットワーク

### 回線交換とパケット交換

- 初期の電話
  - マイクとスピーカー、電源が直列接続
  - 複数人と通話では、それぞれの相手まで回線を敷設する必要
- 回線交換: 電話のアーキテクチャ
  - 中央の交換機が回線を切り替える

### 回線交換

- 通信が開始から終了まで回線を確保
- 利点
  - 通信終了まで回線確保が保証される
  - 常に一定の帯域幅が保証される
- 欠点
  - 転送データがなくても回線が占有される
  - 回線数の上限

### パケット交換

- データをパケットという単位に分割して、複数マシンが中継して相手に届ける通信モデル
  - データをある程度の大きさに分割
  - それぞれに宛先を書いて送信
  - 途中にいるマシンが中継
- 利点
  - 資源を効率的に利用できる
  - 複数地点間で同時に通信できる
  - 複数の種類のデータを流せる
- 欠点
  - 混雑状況により到着が遅くなる可能性
  - パケットが失われる可能性
  - パケットの順番が変わる可能性

## インターネットの歴史と展望

- 1970年代ARPAによるインターネット技術の研究
  - パケット交換のアイデア
    - ARPANETで実現
  - 相互接続

- インターネットリサーチグループ (Internet Research Group)
  - ARPA資金、ARPANETで実験
  - 後にInternet Control COnfiguraation Board (ICCB) → IAB

- 1980年代 global internetの研究
  - ARPAがARPANETのプロトコルをTCP/IPプロトコルに変更
  - TCP/IPの実験
  - ARPANETの役割
    - 研究用: ARPANET
    - 軍事用: MILNET

- BSD UNIX
  - California大学のBerkeley Software Distribution (BSD)のUNIXクローンOS
  - TCP/IPの低コストな利用
  - アプリケーションプログラムも
  - ソケット（socket）の概念

- NSF NET
  - NSFがComputer Science NETwork(CSNET)プロジェクトに資金提供
  - NSF NET長距離バックボーンNW

- 急激な拡張
  - 大学、政府、企業の研究所、政府出資プロジェクト→一般企業
  - 予想されなかった**規模**の問題
    例: 分散資源技術の必要性 → hostsファイルアドレス管理 → dns(Domain Name System)

### Internet Architecture Board (IAB)

- IAB [https://www.iab.org](https://www.iab.org)
  - Internetプロトコル標準を決定、発行
  - ICCB→IAB
- IAB
  - Internet Rearch Task FOrch(IRTF) --- IRS(tering)G --- WGs
  - Internet Engineering Task Force(IETF) --- IESG --- WGs [https://www.ietf.org](https://www.ietf.org)

### インターネット学会(ISOC)

- 政府からはなれている目的で設立: 米国政府によるInternetの普及
- 組織
  - ISOC
    - IAB
      - IRTF
        - IRSG
      - IETF
        - IESG
      - ICANN
        - InternetNIC
        - RIPE
        - APNIC
          - JPNIC

- IANA(Internet Assigned Numbers Authority) = Jon Postel
  - RFC1700 Assigned Numbers
    - ICANN(Internet Corporation for Assigned Name and Numbers)
      - IPアドレス、ドメイン名、ポート番号など番号、記号の標準化登録管理

1. インターネットの3つの識別子の割り振り・割当を全世界的かつ一意に行うシステムの調整
    - ドメイン名
    - IPアドレスおよび自律システム（AS）番号
    - プロトコルポート番号およびパラメータ番号
2. 13個あるNDSルートサーバーシステムの運用および展開の調整
3. これらの技術的業務に関連するポリシー策定の調整

### Internet Request for Comments(RFC)

- Internet関連の文書
- IETFが標準化処理に責任、文書管理、編集
- RFC番号

- RFC標準化(Standard Track)
  - IESG承認
    1. PS: Proposal Standard RFC番号が割り振られstandardtrackの間使われる  提案 → 2つ以上の実装
    2. S: Standard STD番号が割り振られる 標準

- 標準化処理以外
  - Informational
  - Experimental
  - Historic

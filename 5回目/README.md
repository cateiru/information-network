# 5回目

## (IP)データグラム

- ヘッダとデータ領域（ペイロード）

  ```text
  ヘッダ 始点アドレス 終点アドレス
  ```

## データグラムサイズ、ネットワークMTU、フラグメント化

- 最大転送単位（Maximum Transfer Unit(MTU))
  - 物理フレームないで転送できるデータの大きさの上限

    ```text
    イーサネット: 1500octets
    FDDI: 約4770octets
    ```

- フラグメント化
  - データグラムが小さなMTUのNWを通るとき、データグラムを分割すること。

### フラグメント再構成

- 再構成はNWをを通った直後or最後のホスト？
- IPでは最後のホストが再構成する
  - デメリット
    - フラグメント後の小パケットは大MTUを活用できない
    - 分割することによりデータを失う可能性が大
    - フラグメント（断片）がひとつでも失われると再構成不可→再送
  - メリット
    - 各フラグメントが独立に経路制御
    - 中間ルータが蓄積、再構成する必要がない

### フラグメント化制御フィールド

- Identification 16bits
  - もとのデータグラムを識別
  - フラグメント後、各フラグメントで同一値
- Fragment Offset 13bits
  - もとのデータグラム内のオフセット（相対的位置）
  - 8オクテット（64bit）単位
  - 先頭のフラグメントのオフセットは0
- Flags 3bits
  - DFフラグメント不可ビット Don't Fragment
    - DFが0: May Fragment
    - DFが1: Don't Fragment
    - 小MTUでfragmentが必要（fragment needed）しかし、Don't fragment
      - ICMPエラーメッセージ Type3, Code4
      - RFC1191経路MTU探索（Path MTU Discovery）で利用
  - Next-Hop MTU
    - Path MTU Discovery Black Hole（経路MTU探索ブラックホール）
    - TCPのMaximum Segment Size(MSS) オプション
  - MF フラグメント継続ビット More Fragment
    - MFが0: Last Fragment
    - MFが1: More Fragment
    - フラグメントの再構成のため継続パケットを持ったがタイムアウト
      - ICMPエラーメッセージ Type11 Code1 Fragment Reassembly Time Exceeded

### 生存時間（TTL）

- Time to Live 8bits 生存時間フィールド
  - インターネット上存在できる時間
  - ルータが1つ減らす
    - TTL=0のときはデータグラムを捨てる
      - ICMPエラーメッセージを送る

かつては、遅延の予想（秒単位）として使われていたがルータの処理速度向上→各ルータ滞在時間が1秒未満→役に立たなくなった→今はポップリミット

## Internetプロトコル: エラーおよびコントロールメッセージ（ICMP）

- RFC792
- IP: 信頼性のない、コネクションレスなパケット配送サービス
  - 異常→始点に報告

- ICMPはエラー報告。（エラー訂正はしない）
- 中間ルータへの報告には使えない
- ICMPはIPの一部

# 要件定義レビュー結果

`sample-en.md` を SKILL.md の手順でレビューした出力例。レビューの説明は日本語、書き換え案は元の要件文と同じ英語で書く。

## サマリー
- 対象: `sample-en.md`（要件 6 件、言語: 英語）
- NG がある要件: 5 件 / 全 6 件
- 主な問題: 検証できない語 (2 件)、will / should の誤用 (2 件)、実装・運用の混入 (2 件)
- 判定できなかった項目と、その判定に必要な情報: 上位要件（C4-TR-02）、要件の階層（C4-CM-01）、用語集（C4-CS-02）が与えられていない

## 要件ごとの結果

### WSN-001: The sensor node shall measure air temperature with an accuracy of ±0.5 °C over the range -40 °C to +60 °C.
NG なし（根拠の記載がない点は文書全体の指摘にまとめる）

### WSN-002: The node will transmit data to the gateway quickly.
| 項目 | 判定 | 理由 |
|---|---|---|
| C1-02 will の使い方 | NG | 義務を will で書いている。要件なら shall にする |
| C4-VE-03 検証できない語 | NG | "quickly" は測定できない |
| C2-P-03 許容範囲 | NG | 送信の周期・遅延の上限がない |
| C2-P-02 用語の一貫性 | NG | "node" と、他の要件の "sensor node" / "device" が混在 |

**書き換え案**
> WSN-002: The sensor node shall transmit each measurement to the gateway within [TBR: seconds] seconds of acquisition.

### WSN-003: The device shall be lightweight and robust, and shall use a LoRa radio.
| 項目 | 判定 | 理由 |
|---|---|---|
| C4-CL-03 / C4-CL-04 1 文に 1 要件 | NG | 質量、耐久性、無線方式の 3 要件 |
| C4-VE-03 検証できない語 | NG | "lightweight", "robust" は測定できない |
| C2-P-04 実装が書かれていない | NG | "use a LoRa radio" は HOW。通信距離や消費電力など、必要な性能を書く |
| C2-P-02 用語の一貫性 | NG | "device" は "sensor node" と同じものか不明 |

**書き換え案**
> WSN-003a: The sensor node shall have a mass of no more than [TBR: grams] g, including the battery.
> WSN-003b: The sensor node shall meet its performance requirements after exposure to [TBR: environmental test profile, e.g. IEC 60068-2-6 vibration].
> WSN-003c: The sensor node shall communicate with the gateway at a line-of-sight distance of at least [TBR: km] km.

### WSN-004: The operator shall replace the battery every 6 months.
| 項目 | 判定 | 理由 |
|---|---|---|
| C2-P-05 / C4-CM-03 運用の説明 | NG | "The operator shall…" は運用の説明で、製品の要件ではない |
| C4-CS-01 矛盾 | NG | WSN-005 の電池寿命 12 か月と矛盾する |

**書き換え案**
> （要件から外し、ConOps / 保守手順に移す。製品側の要件が必要なら次のように書く）
> WSN-004: The sensor node shall allow battery replacement without tools in no more than [TBR: minutes] minutes.

### WSN-005: The sensor node should operate on battery power for at least 12 months.
| 項目 | 判定 | 理由 |
|---|---|---|
| C1-03 should の使い方 | 要確認 | should は目標。要件として扱うなら shall にする |
| C2-P-03 許容範囲 | NG | 送信周期や温度条件など、12 か月を満たす前提条件がない |

**書き換え案**
> WSN-005: The sensor node shall operate on a single battery for at least 12 months when transmitting one measurement every [TBR: minutes] minutes at an ambient temperature of 20 °C ± 5 °C.

### WSN-006: The sensor node shall not lose data when the gateway is unavailable.
| 項目 | 判定 | 理由 |
|---|---|---|
| C3-04 肯定形 | NG | shall not。肯定形で、保持すべきデータ量・期間を書ける |
| C2-P-03 許容範囲 | NG | ゲートウェイが何時間使えない場合まで保証するかが不明 |

**書き換え案**
> WSN-006: The sensor node shall store at least [TBR: hours] hours of measurements while the gateway is unavailable and transmit the stored measurements when the connection is restored.

## 文書全体の指摘

### 完全性（要件の分類ごとの有無）
| 分類 | 有無 | 備考 |
|---|---|---|
| 機能 | あり | 測定、送信、データ保持 |
| 性能 | 一部 | 温度精度はある。送信遅延、測定周期がない |
| インターフェース | 不足 | ゲートウェイとのプロトコル・データ形式が未定義 |
| 環境 | 不足 | 動作温度範囲は WSN-001 の測定範囲のみ。防水・防塵（IP 等級）、輸送・保管の環境がない |
| 施設 | 対象外の可能性 | |
| 輸送 | なし | 梱包・輸送条件 |
| 訓練・人員 | なし | 設置作業者の要件 |
| 運用性 | 不足 | 設置、電池交換、故障の検知 |
| 安全 | なし | 電池（リチウム電池の輸送規制など） |
| セキュリティ | なし | 通信の暗号化・認証 |
| 外観・物理的特性 | 不足 | 寸法、取り付け方法 |
| 設計 | 一部 | WSN-003 の LoRa は設計上の制約として扱う |

### 一貫性・追跡性
- "sensor node" / "node" / "device" が混在している（C2-P-02, C4-CS-03）。"sensor node" に統一し、用語集に定義する
- WSN-004（6 か月で電池交換）と WSN-005（電池寿命 12 か月）が矛盾している（C4-CS-01）
- 全要件に根拠（rationale）がない（C3-06）
- 上位要件（観測ミッションのニーズ）との紐づけがない（C4-TR-02）

### TBD / TBR 一覧
- 原文に TBD / TBR はない。書き換え案で追加した TBR は、担当と期限を決める必要がある

### 信頼性
- センサー故障の検出・報告の要件がない（C4-RL-02）

## 次にやること
1. WSN-004 と WSN-005 の矛盾を解消し、WSN-004 を保守手順に移す
2. 製品の呼び方を "sensor node" に統一する
3. "quickly", "lightweight", "robust" を数値の基準に置き換える
4. セキュリティ、環境（IP 等級）、インターフェースの要件を追加する
5. 各要件に根拠を付ける

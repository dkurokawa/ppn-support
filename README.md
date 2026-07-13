# PPN Hub API Support

PPN Hub API に関するサポートリクエストはこちらの Issue で受け付けています。

## How to get help

1. **[Issue を作成](../../issues/new/choose)** してください
2. AI が一次対応を行い、必要に応じてエンジニアが対応します
3. 対応はベストエフォートで行います（応答時間の保証はありません）

## Before filing an issue

- [API Documentation](https://docs.ppn-hub.com) を確認してください
- [Status Page](https://status.ppn-hub.com) で障害情報を確認してください
- 既存の [Issue](../../issues) で同様の報告がないか検索してください

## Issue types

| Type | Description |
|------|-------------|
| Bug Report | API の不具合報告 |
| Feature Request | 機能リクエスト |
| Question | API の使い方に関する質問 |

## Support policy

- サポートは GitHub Issue のみで提供します（メール・電話・チャットは非対応）
- AI による自動一次対応を行う場合があります
- 対応言語: 日本語 / English
- [利用条件 (SLA)](https://docs.ppn-hub.com/legal/sla)

## Links

- [PPN Hub Console](https://console.ppn-hub.com)
- [API Documentation](https://docs.ppn-hub.com)
- [Status Page](https://status.ppn-hub.com)

---

## この API 群について（開発・運用の体制）

PPN Hub は、個人で設計・構築・運用しているサービス群です。窓口だけでなく、どう作って回しているかを簡単に記しておきます。

### クラウドは AWS と Cloudflare を「用途」で使い分けています

片方に寄せるのではなく、レイテンシ・状態・計算量で置き場所を選んでいます。同じシステムの中で両者を役割分担させる設計を、個人の運用で日常的に回しています。

| 置き場所 | 何を置くか | 選ぶ理由 |
|---|---|---|
| **AWS** | ML の学習・推論、データ基盤（収集→構造化→配信）、状態を持つ処理、重い計算 | 計算資源・マネージドサービス・データの集約に向く。EC2 / Lambda / S3 / Athena などを用途で選ぶ |
| **Cloudflare Workers** | エッジで完結する軽量 API、常時グローバル配信、低レイテンシ応答 | 世界中のエッジで低遅延に出せる。状態を持たない配信層に向く |

- **ML の学習・推論は AWS 側**。自前学習したモデルの推論をサーバ側で回し、別途 API 化には Lambda を使うなど、ワークロードの性質で AWS のサービスを選び分けています。
- **データ基盤も AWS 側**。生のドキュメントを収集 → 構造化 → 配信する、いわゆるメダリオン構成のデータレイクを自分で設計・構築しています。
- **配信・軽量 API は Cloudflare Workers**。全世界エッジに **120 以上の Workers** を本番稼働させ、**50 以上の API** を本番ドメインで公開しています。
- クラウドを支える土台として、EC2 の本番運用、DB（master / slave / standby 構成）や分散 KVS の自前運用、物理サーバ・仮想化の実地経験があります。マネージドサービス（RDS / ElastiCache 等）の「中身」を読める立ち位置を、最新化のため AWS SAP（Solutions Architect Professional）の学習で体系に接続し直している最中です。

### AI 駆動開発を運用の中核に据えています

モノレポ設計、ドメイン横断の CI/CD、課金・リリース管理までを一人で構築・運用しています。その効率を支えているのが AI 駆動開発です。AI コーディングを単に使うのではなく、開発手順の CLI/CI 化や、複数セッションの開発を協調させる仕組みの自作など、**開発の進め方そのものを作り変えている**のが特徴です。

- LLM 連携（OpenAI / Gemini / Claude）を組み込んだ処理を複数のサービスで実運用しています。
- AI をレビューフローに溶かし込み、コード点検を自動化しています。
- 外部 API に依存せず、ローカル LLM だけで文書を構造化するパイプラインも本番で回しています。

### 一言で

「使えるものを作って本番で回す」を、企画から実装・運用まで一人で縦断しています。42 年プログラミングを続けてきた幅（低レベルのアセンブラから現代のクラウドまで）と、AI による効率化を組み合わせ、未知の領域でも高速に開拓していけるのが強みです。

> 詳細な公開アウトプット（フリーウェア・技術同人誌・各種プロダクト）は [作者ページ](https://www.vector.co.jp/vpack/browse/person/an009351.html) 等でも確認いただけます。

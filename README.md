# AI を活用した Grafana Cloud — ハンズオンワークショップ

**Grafana Assistant** と **Assistant Investigations** を使って、リアルなクラウドネイティブアプリケーションを探索・障害注入・調査します — アプリケーション、Kubernetes、AI オブザーバビリティの全レイヤーにわたって。

> [!IMPORTANT]
> 環境はすでに用意されています。[ログインして概要を把握する](./00-login-and-orient.md)だけで始められます — セットアップは不要です。

## 何をするか

天文学系の EC サイト（"ecommerce"）を監視するライブの Grafana Cloud スタックで作業します — Kubernetes 上で動作する約 20 の OpenTelemetry 計装済みマイクロサービスで構成されており、これ自体が**アジェンティック AI アプリ**（Claude + GPT を使ったショッピングエージェント）でもあります。クエリを書かずに探索し、自然言語で Assistant を操作し、**k6 ロードテスト**で障害を注入して確認し、マルチエージェントの **Investigation** で根本原因を特定し、アプリ自身の **AI オブザーバビリティ** を調べます — PromQL なしのオンコールループです。

## デモアプリについて（OpenTelemetry Astronomy Shop）

アプリは [**OpenTelemetry Demo**](https://opentelemetry.io/docs/demo/) をベースにしています — オブザーバビリティ学習のためのコミュニティ標準リファレンス分散システムです。望遠鏡や天文グッズを販売するウェブストアで、**ポリグロット設計**が特徴です。各マイクロサービスは異なる言語で書かれており、エコシステム全体での OpenTelemetry 計装をテストできます。サービス間の通信は **gRPC と HTTP** が混在しています。

代表的なサービス（括弧内は使用言語）：

| サービス | 役割 | 言語 |
|---------|------|------|
| `frontend` / `frontend-proxy` | Web UI + Envoy エッジプロキシ | TypeScript / Envoy |
| `cartservice` | ショッピングカート（Valkey/Redis バックエンド） | .NET / C# |
| `productcatalogservice` | 商品一覧 | Go |
| `checkoutservice` | 注文処理の調整 | Go |
| `paymentservice` | 注文の決済処理 | JavaScript / Node.js |
| `shippingservice` | 配送料の計算 | Rust |
| `currencyservice` | 通貨換算 | C++ |
| `emailservice` | 注文確認メール | Ruby |
| `recommendationservice` | 商品レコメンデーション | Python |
| `adservice` | コンテキスト広告 | Java |
| `frauddetectionservice` | 不正検知（Kafka 経由） | Kotlin |
| `accountingservice` | 注文会計（Kafka 経由） | .NET |
| `flagd` | フィーチャーフラグサービス（障害シナリオを制御） | Go / OpenFeature |
| `load generator` | リアルなトラフィックを生成 | Python / Locust |

**アーキテクチャの概要:** `frontend`（`frontend-proxy` 配下）はダウンストリームサービスを gRPC/HTTP で呼び出します。`checkoutservice` は payment、shipping、email、currency、cart に展開されます。**Kafka** は accounting と fraud detection への非同期イベントを運び、**Valkey/Redis** はカートのバックエンドです。**flagd** は障害注入フィーチャーフラグを切り替えます。バンドルされた**ロードジェネレーター**がトラフィックを継続的に生成します。全体図: [OTel Demo アーキテクチャ](https://opentelemetry.io/docs/demo/architecture/)。

> [!NOTE]
> このワークショップのスタックは標準デモを**拡張**しています。ショッピング AI エージェントを持つ `chatservice`（モジュール 6）、データベース層、およびデモのフィーチャーフラグ UI の代わりに障害シナリオを起動する **k6 テスト**が追加されています。

## 形式

約 2.5 時間のハンズオン形式です。ライブテレメトリを持つプロビジョニング済みの Grafana Cloud スタックを使用します。Grafana や PromQL の事前知識は不要です。

## アジェンダ

| # | モジュール | 時間 | 習得内容 |
|---|--------|------|---------|
| 0 | [ログインして概要を把握する](./00-login-and-orient.md) | 10 分 | ログイン済み、シグナル確認済み、Assistant の場所を把握 |
| 1 | [クエリなしで探索 — Drilldown と相関](./Lab/01-explore-drilldown.md) | 15 分 | クエリなしの探索；トレース → ログ → メトリクスのピボット |
| 2 | [Application Observability、ナレッジグラフ、RCA](./Lab/02-application-observability.md) | 20 分 | サービスインベントリ（RED）、サービスマップ、エンティティグラフ、RCA ワークベンチ |
| 3 | [Kubernetes Observability + Assistant](./Lab/03-kubernetes-observability.md) | 15 分 | クラスター/ワークロードの状態確認；Assistant にポッドとノードについて質問 |
| 4 | [Assistant を使いこなす — チャット、クエリ、ダッシュボード](./Lab/04-assistant-chat.md) | 20 分 | 自然言語でクエリとダッシュボードを構築 |
| 5 | [カスタマイズと自動化 — Memories、Rules、Automations](./Lab/05-automations.md) | 15 分 | 環境を学習させ、標準を設定し、繰り返し作業をスケジュール |
| 6 | [障害注入と調査 — k6 + Assistant Investigations](./Lab/06-investigations.md) | 30 分 | k6 で障害を注入し、ディープ調査を実行してレポートを読む |
| 7 | [AI Observability — アジェンティックアプリを観察する](./Lab/07-ai-observability.md) | 20 分 | ショッピングエージェントの会話、エージェント、トークンコスト、評価 |
| 8 | [チャレンジ: 実践的に役立つプロンプト](./Lab/08-assistant-challenge.md) | 15 分 | 最良のオンコールプロンプトを発表 |


## 使用する機能

- **Grafana Assistant**（GA）— Grafana Cloud 内のコンテキスト認識型 AI コパイロット。
- **Assistant Investigations**（パブリックプレビュー）— メトリクス、ログ、トレース、プロファイルを横断するマルチエージェントスウォームで、構造化された RCA レポートを生成します。
- **Drilldown アプリ** — クエリなしでメトリクス、ログ、トレースを探索。
- **Application Observability** — OTel ネイティブのサービスインベントリ、RED メトリクス、サービスマップ、**エンティティグラフ**（Asserts ナレッジグラフ）、RCA ワークベンチ。
- **Kubernetes Observability** — クラスター、ワークロード、ノード、コスト、アラートを Assistant と組み合わせて確認。
- **AI Observability** — アプリ自身の LLM エージェントの会話、エージェント、モデル、トークンコスト、オンライン評価。
- **k6 パフォーマンステスト** — トラフィックを生成し、障害シナリオを注入するロードテスト。
- **Automations** — 自然言語で繰り返しのエージェント作業をスケジュール。

## ブックマーク推奨ドキュメント

- [Grafana Assistant を使い始める](https://grafana.com/docs/grafana-cloud/machine-learning/assistant/get-started/)
- [Investigation を実行する](https://grafana.com/docs/grafana-cloud/machine-learning/assistant/guides/investigation/)
- [Application Observability](https://grafana.com/docs/grafana-cloud/monitor-applications/application-observability/)
- [Kubernetes Monitoring](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/kubernetes-monitoring/)
- [AI Observability](https://grafana.com/blog/ai-observability-for-agents-in-grafana-cloud/)
- [Grafana Cloud k6](https://grafana.com/docs/grafana-cloud/testing/k6/)

# 0 — ログインして概要を把握する

*所要時間: 約 10 分。環境はすでに構築・起動済みです。ログインしてひと通り確認しましょう。*

## 観察するアプリについて

**Kubernetes** 上で動作するクラウドネイティブな**天文学系 EC サイト**（"ecommerce"）です。約 20 のマイクロサービス（`frontend`、`cartservice`、`checkoutservice`、`paymentservice`、`productcatalogservice`、`recommendationservice`、`currencyservice` など）が `ecommerce-prod` Namespace で稼働しており、OpenTelemetry のメトリクス・ログ・トレースを完全に出力しています。

また、これは**アジェンティック AI アプリ**でもあります。`chatservice` には Claude と GPT モデルを使ったショッピングエージェント（`general_agent`、`product_agent`、`cart_agent`）が含まれています。今日はリアルなマイクロサービスアプリ*と*リアルな AI エージェントアプリを同時に観察し、さらに AI を使ってそれを行います。

## 1. ログインする

1. ワークショップの Grafana Cloud URL（例: `https://<your-stack>.grafana.net`）を開きます。
2. ファシリテーターから提供された認証情報でサインインします。

## 2. Assistant を開く

**トップバー**の **Open Grafana Assistant**（スパークル ✦）をクリックします。右側にチャットパネルが開きます。挨拶してみて — 返答があれば準備完了です。左ナビにも **Assistant** の項目があります。

## 3. マップを把握する（左ナビ）

今日はこれらのエリアを行き来します。

- **Drilldown** — クエリなしでメトリクス、ログ、トレースを探索。
- **Observability** — スタックの中心:
  - **Application** — EC サービスのサービスインベントリ、RED メトリクス、サービスマップ。
  - **Kubernetes** — クラスター、Namespace、ワークロード、ノード、コスト、アラート。
  - **AI** — *AI Observability*: ショッピングエージェントの会話、エージェント、モデル、トークン、評価。
  - **Entity graph** / **RCA workbench** — ナレッジグラフ（Asserts）と根本原因分析ワークベンチ。
  - **Frontend**、**Database** — フロントエンドと DB のオブザーバビリティ。
- **Testing & synthetics → Performance** — **k6** プロジェクト（`ecommerce`）。ここのロードテストがトラフィックを生成し、障害を注入します（モジュール 6 でファシリテーターが操作します）。
- **Assistant** と **AI & machine learning** — AI コパイロットと ML/Sift 機能。

## 4. 簡単な動作確認

- **Observability → Application** → `ecommerce-prod` Namespace のサービスが表示されることを確認します。
- **Observability → Kubernetes** → クラスターが表示されることを確認します。
- **Drilldown → Traces** → `frontend` / `checkoutservice` のスパンが表示されることを確認します。

何も表示されない場合はファシリテーターに声をかけてください（バックグラウンドのロードテストを開始する必要があるかもしれません）。

## 5. Investigations へのアクセス確認

モジュール 6 でマルチエージェントの Investigation を実行します。これには **Assistant Investigation User** ロールが必要です — ワークショップのログインに事前付与されています。確認するには: Assistant を開いて **Investigation** モードに切り替えられることを確認します。

**[モジュール 1 — クエリなしで探索する →](./Lab/01-explore-drilldown.md)** に進みましょう。

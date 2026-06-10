# 8 — 🧠 チャレンジ: 実践的に役立つプロンプト

*所要時間: 約 15 分。フィナーレです。*

ここまでで以下を行いました。

- Drilldown でクエリなしにメトリクス、ログ、トレースを探索し、シグナル間を相関させた
- Application Observability を確認 — サービスインベントリ、サービスマップ、エンティティグラフ（ナレッジグラフ）、RCA ワークベンチ
- Kubernetes Observability を確認し、Assistant にポッド/ノードの状態を説明してもらった
- Assistant を使ってクエリを実行し、シグナルを相関させ、ダッシュボードを構築した
- k6 テストで注入した実際の障害に対してマルチエージェントの Investigation を実行した
- アプリ自身の AI Observability を調査 — 会話、エージェント、トークンコスト、評価
- Memories と Rules で Assistant をカスタマイズし、Automation をスケジュールした

## 目標

*自分の*日常業務で Assistant が最も役に立てることを見つけましょう。1 人でも、ペアでも構いません。

プロンプトを考えて回答を記録します。最後に、**ベストな 1〜2 個のプロンプト**と、その回答、そして「なぜ役立つか / 実際にいつ使うか」を一言で共有できるよう準備してください。

> [!TIP]
> オンコール担当者の視点で考えましょう。優れたプロンプトは次のことをする傾向があります。
> - 本当に重要なエラーを見つける（ただのノイズではなく）
> - インシデントになる*前に*パターンを察知する
> - *次に*何を確認すべきかを教える
> - タスクを実行することで手作業を減らす（説明するだけでなく）

## プロンプトのアイデア

```
直近 1 時間でサービス全体にわたる最も一般的なエラーの原因は何ですか？
```
```
今日のリクエストパターン、レイテンシー、ステータスコードに異常なスパイクはありますか？
```
```
今 @checkoutservice でアラートが鳴ったとしたら、最初に何を調査すべきですか？その理由も教えてください。
```
```
今週のエラーパターンを先週と比較してください。新しいものや悪化しているものはありますか？
```
```
frontend サービスの SLO スタイルのビューを作成し、エラーバジェットを消費しているかどうか教えてください。
```
```
トレースをもとに、新しいエンジニアに checkout フローがどのように機能するかを説明してください。
```
```
会話あたりのコストが最も高いショッピングエージェントはどれですか？そのトークンコストを削減するにはどうしますか？
```
```
ecommerce-prod のポッドに異常があるものはありますか？それがユーザー向けサービスに影響していますか？
```

## ボーナスラウンド

- **Investigation プロンプト対決:** ファシリテーターに別の k6 障害テスト（Broken DB Connection vs. Slow DB Query）を実行してもらい、誰のディープ調査が最速で根本原因を特定できるか競いましょう。
- **教えてモード:** Assistant が書いた PromQL または TraceQL クエリを、次回は自分で書けるくらいに説明してもらいましょう。
- **カスタマイズ:** Custom Rule を書いて（例: 「常に影響を受けているサービスと次のステップを答えること」）プロンプトを再実行し、動作の変化を確認しましょう。

## 発表

ベストプロンプトと回答をワークショップチャンネルに投稿（または室内で発表）しましょう。最も*実際に役立つ*プロンプトを投票で決めます。🏆

---

探索し、壊し、調査してくださった皆さん、ありがとうございました — アプリケーション、Kubernetes、AI オブザーバビリティを横断して。

ドキュメント: [Grafana Assistant](https://grafana.com/docs/grafana-cloud/machine-learning/assistant/) · [Investigations](https://grafana.com/docs/grafana-cloud/machine-learning/assistant/guides/investigation/) · [Application Observability](https://grafana.com/docs/grafana-cloud/monitor-applications/application-observability/) · [Kubernetes Monitoring](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/kubernetes-monitoring/) · [AI Observability](https://grafana.com/blog/ai-observability-for-agents-in-grafana-cloud/)

# 3 — Kubernetes Observability + Assistant

*所要時間: 約 15 分。目標: EC アプリが動作するクラスターを確認し、Assistant に説明してもらう。*

EC サービスは Kubernetes 上で動作しています。Grafana の Kubernetes Monitoring は、ダッシュボードを構築しなくてもフリートからポッドまでのビューを提供します。

## A. クラスターとワークロードの状態確認

1. 左ナビ → **Observability → Kubernetes**。
2. **Overview** には、`cluster` と `namespace` フィルターが上部にある状態で、**Availability**、**Stability**、**Infrastructure**、**Efficiency** が一目で表示されます。
3. **namespace = `ecommerce-prod`** に設定してアプリにスコープを絞ります。
4. 左ナビのサブページを確認します。
   - **Workloads** — EC サービスの Deployment/StatefulSet。再起動、クラッシュループ、Pending のポッドを確認します。
   - **Nodes** — ノードの CPU/メモリ圧迫状況。
   - **Cost** — この Namespace/ワークロードのコスト。
   - **Alerts** — 発火中の Kubernetes アラート。

**試してみましょう:** 最も再起動回数が多いか、メモリ使用量が最も高いワークロードを見つけ、ポッドレベルの詳細を開きます。

## B. Assistant に説明させる

Kubernetes ページを表示した状態で **Assistant**（✦）を開きます。コンテキスト認識型なので、今見ているページをすでに把握しています。

```
ecommerce-prod Namespace の現在の状態を教えてください。
```
```
ecommerce-prod のポッドで最近再起動したものはありますか？その理由も教えてください。
```
```
CPU またはメモリに圧迫があるノードはありますか？どのワークロードが原因ですか？
```
```
このクラスター内で checkoutservice のデプロイが依存しているものを説明してください。
```

> [!TIP]
> Kubernetes テレメトリ、App Observability サービスマップ、Asserts エンティティグラフはすべて同じナレッジグラフを共有しているため、Assistant は「ポッドが再起動している」→「サービスのエラーレートが上昇している」→「checkout が失敗している」という、インフラからユーザー影響までの縦断的なつながりを把握できます。

## C. RCA へのつなぎ

Kubernetes ページのヘッダーにある **RCA Workbench** リンクを確認しましょう。インシデント時には、*ユーザーに何が起きているか*（Application）、*その下で何が異常か*（Kubernetes）、*その原因は何か*（RCA ワークベンチ + Assistant）を行き来することになります。

## ✅ チェックポイント

アプリの下で動くクラスターを確認し、Assistant にポッド/ノードの状態を平易な言葉で解説してもらいました。次は Assistant を本格的に使いこなします — クエリ、相関、ダッシュボードです。

次: **[モジュール 4 — Assistant を使いこなす →](./04-assistant-chat.md)**

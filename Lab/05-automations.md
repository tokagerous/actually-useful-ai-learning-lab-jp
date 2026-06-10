# 5 — Assistant のカスタマイズと自動化

*所要時間: 約 15 分。目標: Assistant を自分仕様にする — 環境を教え込み（Memories）、基準を設定し（Rules）、スケジュールで動かす（Automations）。*

ここで扱う設定はすべて **Assistant → Settings**（左ナビ → **Assistant → Settings**、または Assistant パネルの歯車アイコン）にあります。Settings には: Custom rules、Skills、Quickstart prompts、**Assistant memories**、**Automations**、SQL table discovery、Python Sandbox、Integrations、External connections が含まれます。

## A. Assistant memories — インフラを学習させる

1. **Assistant → Settings → Assistant memories**。
2. これは*データソースから発見されたインフラに関する AI 生成のメモリ*です。**Start Discovery Scan** をクリックすると、Assistant がテレメトリをスキャンしてサービス、Namespace、依存関係を学習します。
3. 構築完了後、Assistant は自然言語（「checkout の状態は？」）を*あなたの*環境の正しいメトリクスとラベルにマッピングできるようになり、より速く、より正確な回答が得られるようになります。

> [!TIP]
> 前のモジュールで Assistant が `checkoutservice` が `ecommerce-prod` に存在し、何に依存しているかを「自然に知っていた」のは Memories のおかげです。

## B. Custom rules — 組織の標準を設定する

1. **Assistant → Settings → Custom rules** — 「すべての会話での Assistant の応答と動作を誘導する」機能です。
2. このスタックにはすでに確認用のサンプルがあります: **Executive Overview**、**Platform Overview**、**E-commerce Dashboard Guidelines**、**Default GitHub Repo**、**Use Profiles when investigating OOMs**。
3. **Edit** でいずれかを開き、ルールがどのように記述されているかを確認します。スコープ（**Just me** / **Everybody** と、どの **Applications** に適用されるか）にも注目しましょう。Rules は、すべてのエージェントが*あなたの*基準を満たすようにするための仕組みです。

ここで **Skills** と **Quickstart prompts** も確認しましょう — チーム向けにキュレーションできる再利用可能な機能とワンクリックの起動プロンプトです。

## C. Automations — スケジュールで動くエージェント

*繰り返しのエージェント作業をスケジュールして、インサイトを受動的に受け取る — 「朝のコーヒーと一緒に届く朝報」。*

1. **Assistant → Settings → Automations** — 「スケジュールに従って自動実行する、または手動でトリガーするプロンプトを設定する」機能です。
2. 入力ボックスに平易な言葉で Automation を説明し、**Generate** をクリックします。例:

```
毎週平日の午前 8 時に、ecommerce-prod サービス全体の直近 24 時間のエラーをまとめてください。
エラーレートの上位 3 つと、昨日からの新しいエラーパターンを報告してください。
```

3. または **Suggested automation** から始めてカスタマイズします。**Incident**、**Alerts**、**Reports**、**Personal** に分類されています。例:
   - *Active incident status summary* — 1 日 2 回、アクティブな Sev1/Sev2 インシデントのステークホルダー向け要約を作成。
   - *Weekly postmortem digest* — 先週の解決済みインシデント、再発テーマ、未解決のアクションアイテムをまとめる。
   - *Morning incident context briefing* — アクティブなインシデントごとに、関連するアラート、デプロイ、最近の状況をまとめる。
4. 保存すると **Your automations** 一覧に表示されます（スコープ: **Just me** / **Everybody**）。

> [!TIP]
> 同じエージェントへの 3 つのアクセス方法: **Automations** で*スケジュール*し、**Investigations** でオンデマンドに*起動*し、**IRM webhooks** でアラートから*自動トリガー*します。

## ✅ チェックポイント

Assistant に環境を学習させ、組織全体に標準を適用する Rules を確認し、繰り返しのエージェント作業をスケジュールしました。次は何かを壊してエージェントが群れる様子を観察します。

次: **[モジュール 6 — 障害注入と調査 →](./06-investigations.md)**

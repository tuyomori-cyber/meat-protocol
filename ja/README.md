# Meat Protocol

> **人間は選択者であり、運搬者ではない。**

**Meat Protocol** は、人間がシステム間の情報または操作の経路の一部を担うワークフローを、真顔でまじめに集めた小さなカタログです。

重要なのは人間がいるかどうかではなく、人間が何をしているか、そしてどの部分がシステムが安全に引き受けられる機械的作業になったかです。

## 一枚でわかる考え方

```text
経路内の人間
├─ Selection   何を対象にするか
├─ Routing     どこへ送るか
├─ Transform   どう変換するか
├─ Judgment    何を意味するか判断する
├─ Approval    実行・採用を承認する
└─ Transport   どう運ぶか
```

> この経路で人は、選択、経路指定、変換、判断、承認、あるいは単なる情報運搬のどれをしているのか？

> それらの役割のうち、まだ人間を必要とするものはどれで、どれが機械的になったのか？

## 分類と自動化は別問題

ワークフローを Meat Protocol と呼んでも、全工程を自動化すべき、人を排除すべき、判断が不要、という意味ではありません。まず役割を分解し、技術的に自動化できるか、価値があるか、安全か、人間に残すべきかを別に検討します。

**Mechanical Human Work** は、人間が行う作業のうち、なお人間を必要とする判断・意図・承認から分離できる機械的な操作部分です。

## まずはこちら

- [定義](docs/definition.md) — 対象範囲、用語、候補を判定するテスト。
- [原則](docs/principles.md) — このプロジェクトの視点と非目標。
- [Protocol カタログ](examples/README.md) — 最初の6つの例。
- [参照実装](implementations/README.md) — Meat-to-Auto の事例。
- [コントリビュート](CONTRIBUTING.md) — カタログを苦情掲示板にせず事例を追加する方法。

## ステータスの語彙

`Common` · `Workaround available` · `Partially automated` · `Solved` · `Historical`

「Solved」と「Historical」の項目も、UX問題がどう Meat-to-Auto 化されたかを記録します。

## ライセンス

リポジトリのコンテンツは [MIT License](LICENSE) の下で利用できます。

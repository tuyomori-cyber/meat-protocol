# Meat Protocol 現行リポジトリ修正・追加メモ

作成日: 2026-09-22
対象: `tuyomori-cyber/meat-protocol`

## 1. 結論

現行リポジトリの基本思想と構成は良い。ただし今回の議論により、現在の定義は少し狭いことが分かった。

現行では主に「人間がすでに選択・判断した情報を、次のシステムへ機械的に運ぶ」状態を Meat Protocol としている。しかし現実の人力橋渡しでは Transport だけでなく Routing / Transform / Judgment / Approval が混在する。

今後は次を分離して扱う。

- Meat Protocol かどうかの分類
- 人間が経路内で担っている役割の分解
- どの役割を自動化できるか
- どの役割を人間に残すか

## 2. 基本的な問い

ジョーク的な入口:

> 「それ、技術的には自動化できるよね？ なんで人力してるの？」

正式には:

> 技術的にシステム間で処理・伝達できる情報や操作について、人間が経路の一部を担っている箇所を発見し、その役割を分解する。

人間が含まれていること自体を問題視しない。

## 3. Human as selector, not transporter

このフレーズは維持する。ただし、これだけでは全ケースを説明できないため、人間の役割を分解する。

```text
Human in the path
├─ Selection   何を対象にするか選ぶ
├─ Routing     どこへ送るか決める
├─ Transform   内容や形式を変換する
├─ Judgment    内容を判断する
├─ Approval    実行・採用を承認する
└─ Transport   情報・参照・状態を運ぶ
```

Transport は典型的な自動化候補だが、それ以外も条件次第で自動化可能。重要なのは役割ごとに分解して考えること。

## 4. 新Principle: Classification and automation are separate questions

> **Classification and automation are separate questions.**

Meat Protocol と分類されることは「全工程を自動化すべき」「人間を排除すべき」「判断が不要」という意味ではない。

各役割について個別に、

- 技術的に自動化可能か
- 自動化する価値があるか
- 人間を残したいか
- 安全性・責任・承認のため人間が必要か

を検討する。

## 5. Mechanical Human Work

「必要な人力」と俗にいう「脳筋人力」を区別する。ただし `脳筋人力` はジョークとしては使えても、Issue/PR投稿者への正式な分類語には向かない。

正式名称候補:

**Mechanical Human Work**

暫定定義:

> Human-performed work whose mechanical steps can be separated from the judgment, intent, or approval that may still require a human.

日本語:

> 人間が行っている作業のうち、必要な判断・意図・承認などから分離可能な、機械的な操作部分。

判断を含む工程でも Meat Protocol になり得る。判断と機械的作業を分離できれば機械的部分だけ自動化できる。判断自体もルール化できるなら、さらに自動化できる場合がある。

## 6. 発見の問い: 「その操作、毎回本当に考えてる？」

実用的な発見方法として追加候補。

> **Are you actually making a new decision every time you do this?**

同じ「ファイルコピー」でも意味は異なる。

```text
Downloads
↓
Human: 「これはどのプロジェクト？」
↓
適切な保存先
```

これは Routing 判断を含む。

一方、

```text
ChatGPT specification
↓
毎回 Dropbox/project
↓
毎回同じローカルprojectへコピー
```

のように運用が固定化すると、当初必要だった判断がルール化され、残ったコピー操作が Mechanical Human Work になりやすい。

> 人間の判断が反復によってルール化・固定化されたとき、残された手作業が Meat Protocol として浮かび上がることがある。

## 7. Primitive IO の実例を拡張

現行 `implementations/README.md` は Dropbox → ChatGPT の File Path Relay を中心に説明している。成立前のさらに古いworkflowも記録価値がある。

### Before

```text
ChatGPT
↓
「ここまでをMarkdownにまとめて」
↓
ChatGPT画面にMarkdown出力
↓
Human: 全文選択
↓
Human: copy
↓
Human: ローカルエディタへ移動
↓
Human: ファイル作成
↓
Human: paste
↓
Human: 保存
```

保存先決定には判断が必要な場合がある。しかし「この種類の仕様書は Dropbox/project に保存」と運用が固定されると、残る選択・copy・アプリ切替・ファイル作成・paste・save は Mechanical Human Work として分離しやすい。

## 8. 現在見えている次の Meat

現在の開発workflow:

```text
ChatGPT
↓
仕様をまとめる
↓
Dropbox
↓
Human がローカルproject folderへコピー
↓
Codex が読む
↓
実装
↓
Git push
```

`Dropbox → Human → Local Project` が次の候補。

ただしコピー先projectを決める Routing 判断が含まれるため、単純なfolder watcherだけで解決するとは限らない。

さらに Codex にDropbox全体を読ませるだけでは、

```text
Dropbox/project/
├─ spec-A.md
├─ spec-B.md
├─ primitive-io-next.md
├─ otomieru-next.md
└─ ...
```

から「どれだっけ？」問題が起きる。

ローカルproject内の `.gitignore` 対象領域、たとえば `.work/current-spec.md` に置けば、「この仕様はこのprojectのもの」という位置自体がcontextになる。

したがってこれは単なるStorage/Transport問題ではなく **Routing + Transport** の問題。

## 9. Meat-to-Auto を第二の軸として明示

Protocol catalog が問題パターン集なら、Implementation は解決事例集として育てる。

```text
Meat Protocol
問題パターン
      ↓
Meat-to-Auto
人力橋渡しを実際にどう減らしたか
```

各解決事例に以下を記録できる構造がよい。

```text
Before

Human Roles:
- Selection
- Routing
- Transform
- Judgment
- Approval
- Transport

Mechanical Human Work:
- ...

Automation:
- ...

Preserved Human Roles:
- ...

After
```

これにより「面倒なUX集」ではなく、**人力橋渡しがどう自動化されたかの実例集**になる。

## 10. Status vocabulary

現行の以下は維持する。

`Common · Workaround available · Partially automated · Solved · Historical`

`Solved` / `Historical` も残す。Meat-to-Autoの歴史記録として価値がある。

## 11. CONTRIBUTING.md の修正

現行 Good candidate test の

- person has already selected or understood the relevant thing
- next step is largely mechanical transfer

は今回の定義では少し狭い。

Routing / Judgment と Transport が混在する事例も投稿可能にする。

新しい考え方:

1. Human が system-to-system workflow の経路にいるか。
2. Human が何を担当しているか分解できるか。
3. その中に機械的に分離・自動化できそうな部分があるか。
4. 自動化すべき範囲は投稿時点で決まっていなくてもよい。

Issue投稿者に「あなたの作業は無駄」と評価しない。workflowを分解すること自体に価値がある。

## 12. Issue / PR の境界判定

将来、外部からIssue/PRが来ると「Meat / Not Meat」の二択判定は難しい。

まずworkflowを分解する。

```text
System A
↓
Human
├─ Selection
├─ Routing
├─ Judgment
├─ Transform
├─ Approval
└─ Transport
↓
System B
```

その上で、

```text
Transport → automation candidate
Routing   → rules/context次第
Judgment  → may or may not be automated
Approval  → intentionally human の場合もある
```

と記述する。

曖昧な境界事例も資料として価値がある。

## 13. examples/TEMPLATE.md の追加候補

```markdown
## Situation

## Current Flow

## Human Roles

- Selection:
- Routing:
- Transform:
- Judgment:
- Approval:
- Transport:

## Mechanical Human Work

## Why Is a Human Here?

## What Could Be Automated?

## What Should Remain Human?

## Possible Improvements

## After / Meat-to-Auto

## Status

## History

## Comic
```

全項目を必須にはしない。

## 14. README.md の修正

維持:
- `Human as selector, not transporter.`
- serious-with-a-straight-face のトーン
- Human-in-the-loop を否定しない
- Status vocabulary
- comic

修正:
現行の

> Is the person making a decision here, or merely moving already-selected information?

は二択すぎる。

候補:

> What is the human doing in this path: selecting, routing, transforming, judging, approving, or merely transporting information?

さらに:

> Which of those roles still require a human, and which have become mechanical?

のような問いを置く。

## 15. docs/definition.md の修正

現行 Candidate test の `mostly transport` 条件を広げる。

候補:

> Meat Protocol candidates are workflows where a human forms part of an information or action path between systems, and at least part of that human role could plausibly be separated into mechanical work.

その後でHuman Rolesを分類する。

## 16. docs/principles.md への追加

最低限:

### Classification and automation are separate questions
分類と、どこまで自動化するかは別問題。

### Separate judgment from mechanics
判断と機械的操作が混ざる場合、workflowを分解する。

### Automation can move the boundary
今日の「必要な判断」が、明日はrule / metadata / contextにより自動化可能になることがある。

### Automate transport, preserve judgment where it matters
ただし「judgmentは常に人間」と固定しない。必要性を個別判断する。

## 17. 新規Protocol候補

### Translation Relay

```text
English PR
↓
Human copies text
↓
Translation system / ChatGPT
↓
Human reads Japanese
↓
GitHubへ戻る
↓
Merge
```

将来GitHub Actions等で翻訳・semantic checkを自動化すれば Meat-to-Auto 実例になる。

### Cloud-to-Local Spec Relay

```text
ChatGPT
↓
Dropbox spec
↓
Human copy
↓
Local project (.gitignored work file)
↓
Codex
```

Transport と Routing の混在例。

## 18. 英語正本 + 日本語版

英語正本を即座に読みづらいため日本語参照版を作る。

```text
meat-protocol/
├── en/
│   ├── README.md
│   ├── docs/
│   ├── examples/
│   └── implementations/
└── ja/
    ├── README.md
    ├── docs/
    ├── examples/
    └── implementations/
```

英語を canonical source とし、日本語は意味的に同一の参照版。

最初は手動同期でもよい。Translation Relayを実際に経験・記録した後で自動化するのも Meat-to-Auto の実例になる。

将来候補:
- paired file check
- headings / links / images / IDs / code blocks の構造比較
- LLMによるsemantic equivalence check
- drift時にIssue作成
- translation Action

## 19. Primitive IO の今後との関係

Primitive IO はDropbox専用UIから **ChatGPTに対する reference-source selector** へ抽象化できる可能性がある。

```text
Source: [Dropbox ▼]

Dropbox
└─ project/

GitHub
├─ meat-protocol/
├─ primitive-io/
└─ otomieru-boyer/
```

Human はsource/repo/fileを選ぶ。Primitive IOはreferenceをChatGPTへ渡す。ChatGPTは公式Dropbox/GitHub integrationから実データを読む。

Primitive IO自身はファイル本文を運ばず、参照先だけを運ぶ思想を維持する。

これにより「GitHubのPrimitive IO repoを読んで。これについて議論したい」という自然言語によるreference handoffも減らせる。

## 20. 優先度

### P0: 定義の修正
- `docs/definition.md`
- `docs/principles.md`
- `CONTRIBUTING.md`
- `examples/TEMPLATE.md`

特に「判断済みpayloadのtransportだけがMeat Protocol」という狭い定義を修正する。

### P1: README更新
- Human Rolesの分解を簡潔に紹介
- Classification ≠ Automation
- Mechanical Human Workを短く紹介

READMEは長文化しすぎない。

### P1: 日本語版作成
英語正本と1:1対応する日本語版を作る。

### P2: Meat-to-Auto構造
Implementation/solution pages に Before / Human Roles / Automation / Preserved Roles / After を持たせる。

### P2: 新規Protocol
- Translation Relay
- Cloud-to-Local Spec Relay

実体験を記録してから正式IDを振ってもよい。

### P3: 自動翻訳・semantic check
最初から作り込みすぎず、手動運用で問題を観測してから実装する。

## 21. 今回の議論の中心

Meat Protocol は、

> 「人間がやっていることは全部自動化しよう」

ではない。

また、

> 「判断は人間、輸送は機械」

だけでもない。

より一般化すると、

> **人間がシステム間の経路で担っている役割を分解し、技術的・運用的に機械へ移せる部分を見つける。**

そして、

> **人間を残すべき場所と、残す必要がなくなった Mechanical Human Work を区別する。**

その境界は固定ではない。ルール化、metadata、context、API、AI、integration の進歩によって、昨日まで判断だったものが明日は機械化可能になることもある。

この「境界が移動していく過程」まで Before / After / History として蓄積することが Meat-to-Auto catalog の価値になる。

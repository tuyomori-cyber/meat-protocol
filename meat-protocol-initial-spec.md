# Meat Protocol — 初期仕様書

## 1. 概要

**Meat Protocol** は、本来システム間で受け渡せる参照・状態・コンテキストなどを、**人間が手作業で運搬している状態**を収集・分類するための、半分ジョーク・半分真面目な概念／カタログプロジェクトである。

LLMやAIエージェントはファイルシステム、クラウドストレージ、リポジトリなどへアクセスできるようになってきた。一方で、人間が「どの対象をAIに渡すか」を指定する部分では、コピー＆ペースト、ファイルパスの転記、ZIP化、再説明などの手作業が依然として多い。

このプロジェクトでは、そのような「人間が transport layer になっている」事例を **Meat Protocol** と呼び、GitHub上で事例を収集する。

中心となる考え方は次の一文で表す。

> **Human as selector, not transporter.**

人間が「何を渡すか」を判断することは重要である。しかし、決めた対象を実際に別システムへ運ぶだけの作業は、可能なら機械側で処理できる。

---

## 2. 目的

### 2.1 主目的

- AI／LLM利用時に発生する人力の情報輸送を、包括的なUX問題として可視化する。
- 個々の製品固有の不満として散在している事例を、共通概念として整理する。
- Meat Protocolの具体例と、その改善方法をカタログ化する。
- 技術的に大げさになりすぎず、漫画やユーモアを使って問題を直感的に伝える。

### 2.2 このプロジェクトが目指さないもの

- Human-in-the-loopそのものを否定しない。
- 人間の判断をすべてAIへ委任することを目的としない。
- 特定のLLM、ベンダー、MCP、プラグイン方式を唯一の正解としない。
- すべてを完全自動化することを目的としない。

---

## 3. 基本概念

### 3.1 Human as Selector

人間が対象を選ぶ。

例:

- 「この仕様書を見てほしい」
- 「この3ファイルだけ比較してほしい」
- 「このエラー部分だけ別のAIにも見せたい」
- 「この議論をこのプロジェクトへ保存したい」

これは人間の意図・判断であり、必ずしも排除すべき作業ではない。

### 3.2 Human as Transporter

対象を決めた後、人間が情報を機械的に運搬する。

例:

1. ファイルパスをコピーする。
2. 別のチャット画面へ移動する。
3. パスを貼り付ける。
4. 「このファイルを読んで」と入力する。

この部分を Meat Protocol の主要な対象とする。

### 3.3 判定の目安

次の問いが有効。

> 「人間はここで判断しているのか、それとも単に運んでいるだけなのか？」

後者であり、かつシステム側で代替可能なら、Meat Protocolの候補とする。

---

## 4. 代表例

### MP-001 — Clipboard Relay

あるAI／システムの出力をコピーし、別のAI／システムへ貼り付ける。

例:

```text
ChatGPT
   ↓
 Copy
   ↓
 Human
   ↓
 Paste
   ↓
Codex
```

特に、上流の調査・要件定義・設計をChatGPT等で行い、その結果をCodexへ渡して実装させるワークフローで発生する。

---

### MP-002 — File Path Relay

人間がGUI上ですでに目的のファイルを発見しているにもかかわらず、そのパスをコピーしてAIのチャット欄へ貼る。

例:

```text
VSCode Explorer
      ↓
player.rs を発見
      ↓
Copy Relative Path
      ↓
Codex Chat
      ↓
Paste
      ↓
「このファイルを見て」
```

Codex自身は同じローカルリポジトリへアクセスできるため、運搬されているのはファイル本文ではなく **参照情報** だけである。

改善UIの例:

```text
player.rs
  右クリック
    └─ Send reference to Codex
```

---

### MP-003 — ZIP Shuttle

複数ファイルやプロジェクトをAIへ渡すため、人間がZIP化し、アップロードする。

```text
Project
  ↓
Human creates ZIP
  ↓
Upload
  ↓
LLM
```

---

### MP-004 — Screenshot Relay

別システムに存在する状態・エラー・画面情報を伝えるため、人間がスクリーンショットを作成し、別のAIへアップロードする。

スクリーンショット自体が意図的な情報選択である場合もあるため、すべてをMeat Protocolとみなすのではなく、「単なる輸送か」という観点で分類する。

---

### MP-005 — AI-to-AI Relay

AI Aの回答を、人間がAI Bへコピーし、AI Bの回答を再びAI Aへ戻す。

```text
AI A
 ↓
Human
 ↓
AI B
 ↓
Human
 ↓
AI A
```

人間がレビュー・編集・選別している場合はHuman-in-the-loopであり、単純な往復輸送部分のみをMeat Protocolとして考える。

---

### MP-006 — Re-explanation Protocol

別のAIや別セッションへ移った際、人間が既に決定済みの背景・仕様・制約を再説明する。

例:

> 「前のChatGPTではこういう理由でこの仕様に決めていて……」

データのコピーではなく、**人間自身がコンテキストを再構成して輸送する**タイプ。

---

## 5. Primitive IOとの関係

Primitive IOは、Meat Protocolを減らす小さな実装例として扱える。

現在のPrimitive IOはFirefox版ChatGPT上にDropboxファイルツリーを表示し、人間が選択したMarkdown／テキストファイルのパスをChatGPT入力欄へ渡す。

```text
Before

Dropbox
  ↓
Human searches
  ↓
Copy path
  ↓
Return to ChatGPT
  ↓
Paste / type instruction


Primitive IO

Dropbox tree in ChatGPT
  ↓
Human selects file
  ↓
Primitive IO transports reference
  ↓
ChatGPT
  ↓
Official Dropbox integration reads file
```

重要なのは、Primitive IO自身がDropbox本文を取得・送信するのではなく、**人間が選択した参照を運ぶ部分だけを自動化する**ことである。

これは、

```text
Before:
Human = selector + transporter

After:
Human = selector
Primitive IO = transporter
```

という Meat Protocol 改善例になる。

将来的にDropbox以外の正式なI/O API、MCP、プラグイン等が利用可能になった場合でも、「人間が対象を視覚的に選択し、その参照をLLMへ渡す」というUX自体は別の問題として残りうる。

---

## 6. MCPとの区別

MCPは主に、

```text
LLM <-> External System
```

を接続するプロトコルである。

Meat Protocolが扱う問題には、

```text
Human <-> LLM
```

の対象指定UXも含まれる。

したがって、LLMがFilesystem MCP等を通じてファイルへアクセス可能になっただけでは、

> 「人間がすでに見つけているこのファイルを、どう簡単に指し示すか」

という問題が自動的に解決するとは限らない。

MCPとMeat Protocolは競合概念ではなく、異なるレイヤーを扱う。

---

## 7. GitHubリポジトリ案

仮称:

```text
meat-protocol
```

初期構成案:

```text
meat-protocol/
├── README.md
├── LICENSE
├── docs/
│   ├── definition.md
│   └── principles.md
├── examples/
│   ├── MP-001-clipboard-relay.md
│   ├── MP-002-file-path-relay.md
│   ├── MP-003-zip-shuttle.md
│   ├── MP-004-screenshot-relay.md
│   ├── MP-005-ai-to-ai-relay.md
│   └── MP-006-re-explanation.md
└── comics/
```

GitHub Wikiを併用する案もある。

### README

短く理解できる入口とする。

含める候補:

- Meat Protocolとは何か
- Human as selector, not transporter.
- 代表的な図
- 代表例数件
- Wiki / Catalogへの導線
- Contribution案内

### Wiki / Catalog

事例を継続的に追加する。

ページ例:

```text
Home
├─ What is Meat Protocol?
├─ Principles
├─ Protocol Catalog
│   ├─ MP-001 Clipboard Relay
│   ├─ MP-002 File Path Relay
│   ├─ MP-003 ZIP Shuttle
│   ├─ MP-004 Screenshot Relay
│   ├─ MP-005 AI-to-AI Relay
│   └─ MP-006 Re-explanation Protocol
├─ Implementations
│   └─ Primitive IO
└─ Comics
```

---

## 8. Meat Protocolページのテンプレート案

各事例は、できるだけ同じ形式で記録する。

```markdown
# MP-XXX — Name

## Situation

どのような状況で発生するか。

## Current Flow

現在の人間を含むデータフロー。

## What the Human Decides

人間が本当に判断している部分。

## What the Human Transports

人間が単に運搬している部分。

## Why It Exists

なぜ現在この手作業が必要なのか。

## Possible Improvement

UI、API、MCP、共有ストレージ、プラグイン等による改善案。

## Status

- Common
- Workaround available
- Partially automated
- Solved
- Historical

## Comic

対応する漫画があれば掲載。
```

---

## 9. 漫画

漫画は単なる装飾ではなく、Meat Protocolの不自然さを直感的に説明する役割を持たせる。

例: MP-002 File Path Relay

1. Codex「リポジトリ全体にアクセスできます！」
2. 人間「すげぇ！」
3. 人間「player.rsを見てほしいな」
4. VSCodeで右クリック → Copy Relative Path
5. Codex Chatへ移動 → Ctrl+V
6. **MEAT PROTOCOL**

漫画は各WikiページまたはCatalogページへ添付し、SNSで単体共有しても意味が通じる構成を目指す。

---

## 10. プロジェクトのトーン

完全なジョークプロジェクトにはしないが、堅苦しい標準規格にも寄せすぎない。

狙うバランス:

- 名前はふざけている。
- 問題の観察は真面目。
- 定義は明確。
- 実例は現実のUXに基づく。
- 特定企業・製品を攻撃する目的にはしない。
- 「こんな原始的なことをまだやっている」という自虐・観察型のユーモアを中心にする。

---

## 11. 初期MVP

最初のリリースでは大規模な仕組みを作らない。

最低限:

1. GitHub repositoryを作成する。
2. READMEでMeat Protocolを定義する。
3. `Human as selector, not transporter.` を原則として記載する。
4. MP-001〜MP-006程度の初期事例を登録する。
5. 少なくとも1本、Meat Protocolを説明する漫画を掲載する。
6. Primitive IOを改善例／reference implementationの一例として紹介する。
7. 新しい事例を追加しやすいテンプレートを用意する。

---

## 12. 今後検討すること

- 既存概念・既存用語との重複調査
- Meat Protocolの厳密な判定条件
- Protocol IDの命名規則
- Status分類
- GitHub Wiki中心にするか、repo内Markdownを正本にするか
- GitHub Issuesから事例を募集する仕組み
- Contributionガイドライン
- ライセンス
- 漫画画像のライセンス／生成AI利用表記
- 英語を正本にするか、日本語／英語併記にするか
- Primitive IO以外の改善実装例の収集
- 「既に解決されたMeat Protocol」を歴史資料として残すか

---

## 13. Codexへの初期依頼の方向性

Codexにはまず、上記の思想を維持したままGitHubリポジトリの初期構成を作成してもらう。

重要事項:

- 最初からWebサイトや複雑なアプリを作らない。
- Markdown中心の軽量なrepoとする。
- READMEだけ読んでも概念が伝わるようにする。
- Wikiへ展開しやすい構造にする。
- ユーモアを消さない。
- 「自動化こそ正義」とせず、人間の判断と単純輸送を区別する。
- 将来技術で解決された事例も、UX史として記録できる構造を考慮する。

この仕様書は初期案であり、Codexとの議論・実装を通じて更新する。

# requirements-review

NASA Systems Engineering Handbook の [Appendix C: How to Write a Good Requirement — Checklist](https://www.nasa.gov/reference/appendix-c-how-to-write-a-good-requirement/)（C.1〜C.4）を基準に要件定義をレビューし、要件ごとの合否・理由・書き換え案と文書全体の指摘を出す Skill。日本語・英語の要件定義に対応する。

## インストール

### npx skills を使う

```sh
# プロジェクト用（<プロジェクト>/.claude/skills/requirements-review/ にインストール）
npx skills add utaminuk/skills --skill requirements-review -a claude-code

# 個人用（~/.claude/skills/requirements-review/ にインストール）
npx skills add utaminuk/skills --skill requirements-review -a claude-code -g
```

### 更新・削除

```sh
# 更新する（プロジェクト用は -p、個人用は -g）
npx skills update requirements-review -p
npx skills update requirements-review -g

# 削除する（個人用は -g を付ける）
npx skills remove requirements-review -a claude-code
npx skills remove requirements-review -a claude-code -g
```

### 手動でコピーする

```sh
git clone https://github.com/utaminuk/skills.git
cp -r skills/skills/requirements-review ~/.claude/skills/
```

プロジェクト用にする場合は、コピー先を `<プロジェクト>/.claude/skills/` にする。

## 使い方

Claude Code で次のように依頼すると起動する。

- 「`docs/requirements.md` の要件定義をレビューして」
- 「この要求仕様を確認して」（要件定義のテキストを貼り付ける）
- 「この要件は良い書き方か見て」
- 「shall の使い方を確認して」
- "Review these requirements"

### 入力

- 要件定義のテキスト（Markdown などのファイル、またはチャットへの貼り付け）
- 日本語・英語のどちらでもよい。混在していてもよい
- 用語集、スタイル規則、上位要件があれば一緒に渡す。ない場合、それが必要な項目は「判定不可」になる

### 出力

- サマリー（NG がある要件の件数、主な問題）
- 要件ごとの結果（NG の項目、理由、書き換え案）
- 文書全体の指摘（要件の分類ごとの漏れ、矛盾、追跡性、TBD / TBR の一覧）
- 次にやること

元のファイルは書き換えない。頼んだ場合だけ、書き換え案をファイルに反映する。

### 出力例

| 入力（サンプル要件定義） | 出力（レビュー結果） |
|---|---|
| [examples/sample-ja.md](examples/sample-ja.md) | [examples/review-ja.md](examples/review-ja.md) |
| [examples/sample-en.md](examples/sample-en.md) | [examples/review-en.md](examples/review-en.md) |

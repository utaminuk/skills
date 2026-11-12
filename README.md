# skills

Claude Code 用の Skill 集。

## Skills

| Skill | 概要 |
|---|---|
| [requirements-review](skills/requirements-review/README.md) | NASA Systems Engineering Handbook Appendix C のチェックリストで要件定義をレビューし、要件ごとの合否・理由・書き換え案と文書全体の指摘を出す（日本語・英語対応） |

各 Skill の使い方は、表のリンク先（各 Skill の README）を参照。

## インストール

### npx skills を使う（推奨）

[vercel-labs/skills](https://github.com/vercel-labs/skills) の CLI を使うと、リポジトリを clone せずにインストールできる。Node.js が必要。v1.8.1 で動作を確認している。

```sh
# インストールできる Skill の一覧を表示する（インストールはしない）
npx skills add utaminuk/skills --list

# すべての Skill をインストールする
npx skills add utaminuk/skills --skill '*' -a claude-code

# Skill を指定してインストールする
npx skills add utaminuk/skills --skill requirements-review -a claude-code
```

`-a claude-code` は、Claude Code にインストールするための指定。`-y` を付けると確認なしでインストールする。

#### インストール先

| 指定 | 用途 | インストール先 |
|---|---|---|
| `--global`（`-g`）を付ける | 個人用（すべてのプロジェクトで使う） | `~/.claude/skills/<skill名>/` |
| 付けない | プロジェクト用（コマンドを実行したプロジェクトだけで使う） | `<プロジェクト>/.claude/skills/<skill名>/` |

```sh
# 個人用にインストールする
npx skills add utaminuk/skills --skill requirements-review -a claude-code -g
```

#### 更新・削除

```sh
# 更新する（プロジェクト用は -p、個人用は -g）
npx skills update requirements-review -p
npx skills update requirements-review -g

# 削除する（個人用は -g を付ける）
npx skills remove requirements-review -a claude-code
npx skills remove requirements-review -a claude-code -g
```

### 手動でコピーする

Node.js がない環境や、Skill を手元で書き換えて使う場合は、使いたい Skill のディレクトリを `~/.claude/skills/`（個人用）または `<プロジェクト>/.claude/skills/`（プロジェクト用）にコピーする。

```sh
git clone https://github.com/utaminuk/skills.git
cp -r skills/skills/requirements-review ~/.claude/skills/
```

削除するときは、コピーしたディレクトリを削除する。

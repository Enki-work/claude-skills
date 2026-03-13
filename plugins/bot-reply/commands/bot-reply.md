---
description: "PR上のcoderabbitai・chatgpt-codex-connector・copilotによる未返信コメントを判断し、必要なら修正してcommit/pushした上で返信する"
argument-hint: "<PR_URL>"
allowed-tools: Read, Edit, Glob, Grep, mcp__plugin_github_github__pull_request_read, mcp__plugin_github_github__add_reply_to_pull_request_comment, mcp__plugin_github_github__add_issue_comment, Bash(gh api *), Bash(git add *), Bash(git commit *), Bash(git push *)
---

# ボットコメントへの対応と返信

指定されたPRの coderabbitai・chatgpt-codex-connector・copilot による未返信コメントを1件ずつ精査し、対応が必要かを判断した上で修正・返信します。

**引数:** $ARGUMENTS

## 処理フロー

### 1. 実行前確認

```
PR URL: <PR_URL> のボットコメントを確認・対応します。続行しますか？ (y/n)
```

「n」の場合は中断する。

### 2. PR情報の解析

PR_URL から owner・repo・pullNumber を抽出する:
- 例: `https://github.com/myorg/myrepo/pull/123` → owner=myorg, repo=myrepo, pullNumber=123

### 3. 未返信ボットコメントの取得（MCP優先）

**MCP使用:**

- `mcp__plugin_github_github__pull_request_read` (method: `get_review_comments`) → インラインレビューコメント
- `mcp__plugin_github_github__pull_request_read` (method: `get_comments`) → 一般コメント

**絞り込み条件（両方を満たすもののみ対象）:**

1. `user.login` が以下のいずれかにマッチ（大文字小文字を区別しない）:
   - `coderabbitai`
   - `chatgpt-codex-connector`
   - `copilot` / `github-copilot`

2. 未返信である:
   - インラインコメント: `in_reply_to_id` が null（スレッドの起点）かつ、そのスレッドに返信コメントが存在しない
   - 一般コメント: コメント一覧の順序・内容から返信済みでないと判断できるもの

MCPが使えない場合のフォールバック:
```bash
gh api repos/{owner}/{repo}/pulls/{pullNumber}/comments \
  --jq '[.[] | select(.user.login | test("coderabbitai|chatgpt-codex-connector|copilot"; "i")) | select(.in_reply_to_id == null)]'

gh api repos/{owner}/{repo}/issues/{pullNumber}/comments \
  --jq '[.[] | select(.user.login | test("coderabbitai|chatgpt-codex-connector|copilot"; "i"))]'
```

未返信コメントが存在しない場合は「未返信のボットコメントはありません」と伝えて終了する。

### 4. コメントごとに判断・対応・返信（1件ずつ繰り返す）

取得した未返信コメントを1件ずつ処理する。各コメントに対して以下を実行する:

#### 4.1 コメント内容の理解と判断

コメントが指摘している内容を正確に把握し、以下のいずれかに分類する:

**対応不要と判断する場合:**
- 誤検知（ボットの誤り）
- すでに別の形で対処済み
- プロジェクトの方針・設計上の意図的な実装
- 指摘の優先度が低く、このPRのスコープ外

**対応必要と判断する場合:**
- 実際のバグや不具合
- コード品質・可読性の明確な改善点
- セキュリティ上の問題
- プロジェクトの規約違反

#### 4.2-A 対応不要の場合 → 理由を添えて返信

対応しない理由を明確に説明した返信を生成し、投稿する。

#### 4.2-B 対応必要の場合 → 修正 → commit/push → 返信

**修正:** 該当ファイルをRead・Edit等のツールで修正する

**commit（1コメントにつき1コミット）:**
```bash
git add <修正したファイル>
git commit -m "fix: <コメントの指摘内容を簡潔に表した説明>

Addressed bot review comment: <コメントの要点>"
```

**push:**
```bash
git push
```

**返信:** 修正内容と対応方針を伝える返信を生成し、投稿する。

#### 4.3 返信の投稿（MCP優先）

返信内容をユーザーに提示し、確認を取ってから投稿する。

**インラインコメントへの返信（MCP）:**
`mcp__plugin_github_github__add_reply_to_pull_request_comment`

**一般コメントへの返信（MCP）:**
`mcp__plugin_github_github__add_issue_comment`

MCPが使えない場合のフォールバック:
```bash
gh api repos/{owner}/{repo}/pulls/{pullNumber}/comments/{commentId}/replies \
  -X POST -f body="返信内容"
```

返信はコメントの言語（日本語/英語）に合わせる。

1件の処理が完了したら次のコメントへ進む。

### 5. 完了報告

全件処理後、以下をまとめて報告する:

```
## 対応完了

- 対応して修正・push: N件
- 対応不要として返信: N件
- 合計: N件
```

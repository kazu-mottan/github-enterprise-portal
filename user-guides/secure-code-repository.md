# セキュアコードリポジトリ 運用ガイド

社内の GitHub Enterprise におけるセキュアなリポジトリ運用のためのガイドラインです。
すべての開発者・リポジトリ管理者を対象としています。

---

## 目次

1. [リポジトリ作成時のチェックリスト](#リポジトリ作成時のチェックリスト)
2. [ブランチ保護ルール](#ブランチ保護ルール)
3. [シークレット・機密情報の管理](#シークレット機密情報の管理)
4. [依存関係とサプライチェーンセキュリティ](#依存関係とサプライチェーンセキュリティ)
5. [コードスキャン](#コードスキャン)
6. [アクセス権限管理](#アクセス権限管理)
7. [インシデント対応フロー](#インシデント対応フロー)

---

## リポジトリ作成時のチェックリスト

新規リポジトリを作成したら、以下を必ず設定してください。

- [ ] Visibility を **Private** または **Internal** に設定
- [ ] `README.md` にリポジトリの目的・オーナー・連絡先を記載
- [ ] `.gitignore` を適切なテンプレートで作成
- [ ] `CODEOWNERS` ファイルを追加し、コードオーナーを指定
- [ ] デフォルトブランチに **ブランチ保護ルール** を設定（後述）
- [ ] **Dependabot** を有効化
- [ ] **Secret Scanning** を有効化
- [ ] **Code Scanning（CodeQL）** を有効化（対応言語のみ）

---

## ブランチ保護ルール

`main`（または `master`）ブランチには以下の保護ルールを設定してください。

```
Settings > Branches > Branch protection rules
```

| 設定項目 | 推奨値 |
|---|---|
| Require a pull request before merging | ✅ 有効 |
| Required number of approvals | 1 名以上 |
| Dismiss stale pull request approvals | ✅ 有効 |
| Require review from Code Owners | ✅ 有効（CODEOWNERS がある場合） |
| Require status checks to pass | ✅ 有効（CI が設定されている場合） |
| Require branches to be up to date | ✅ 有効 |
| Restrict who can push to matching branches | 管理者のみ直接プッシュ可 |
| Allow force pushes | ❌ 無効 |
| Allow deletions | ❌ 無効 |

---

## シークレット・機密情報の管理

### やってはいけないこと

```bash
# ❌ 絶対にコードに直接書かない
API_KEY = "sk-1234567890abcdef"
DB_PASSWORD = "mysecretpassword"
```

### 正しい方法

**① GitHub Actions Secrets を使う**

```yaml
# .github/workflows/deploy.yml
steps:
  - name: Deploy
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: ./deploy.sh
```

**② `.env` ファイルを使い、`.gitignore` に追加する**

```bash
# .gitignore
.env
.env.local
.env.*.local
*.pem
*.key
```

**③ 万が一コミットしてしまったら**

```bash
# 1. まず即座にシークレットを無効化・ローテーション
# 2. 履歴から削除
git filter-repo --path .env --invert-paths
# 3. 管理者・セキュリティチームに報告
```

---

## 依存関係とサプライチェーンセキュリティ

### Dependabot の設定

`.github/dependabot.yml` をリポジトリに追加します。

```yaml
version: 2
updates:
  - package-ecosystem: "npm"      # npm / pip / maven / gradle など
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10
    reviewers:
      - "your-team"
```

### サードパーティアクションのバージョン固定

```yaml
# ❌ タグ指定はハイジャックされる可能性がある
uses: actions/checkout@v4

# ✅ コミット SHA で固定する
uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683  # v4.2.2
```

---

## コードスキャン

### CodeQL の設定（GitHub Actions）

```yaml
# .github/workflows/codeql.yml
name: CodeQL Analysis

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * 1'   # 毎週月曜 2:00 UTC

jobs:
  analyze:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
      contents: read
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683
      - uses: github/codeql-action/init@v3
        with:
          languages: javascript, python   # 対象言語を指定
      - uses: github/codeql-action/autobuild@v3
      - uses: github/codeql-action/analyze@v3
```

### Secret Scanning のカスタムパターン例

Organization 設定 > Code security > Custom patterns から追加できます。

| パターン名 | 正規表現の例 |
|---|---|
| 社内 API キー | `MYCO_[A-Z0-9]{32}` |
| 内部 DB 接続文字列 | `jdbc:oracle://internal-db` |

---

## アクセス権限管理

### 権限レベルの使い分け

| ロール | 権限 | 用途 |
|---|---|---|
| Read | 閲覧のみ | 外部レビュアー・参照専用メンバー |
| Triage | Issue・PR の管理 | QA・プロジェクトマネージャー |
| Write | コードのプッシュ | 一般開発者 |
| Maintain | 設定の一部変更 | テックリード |
| Admin | 全設定変更・削除 | リポジトリオーナー・管理者 |

### 定期棚卸し（四半期ごと推奨）

1. `Settings > Collaborators and teams` でメンバー一覧を確認
2. 退職・異動者のアクセスを削除
3. 不要な Admin 権限を下位ロールへ変更
4. Outside Collaborator の継続必要性を確認

---

## インシデント対応フロー

```
インシデント発生
      │
      ▼
① 影響範囲の特定
   - 漏えいしたシークレットの種類・スコープ
   - アクセスされた可能性のあるリポジトリ
      │
      ▼
② 即座に無効化・ローテーション
   - PAT / API キー / SSH キーを失効
   - パスワードを変更
      │
      ▼
③ セキュリティチームへ報告
   連絡先: security@example.com
   Slack: #github-security
      │
      ▼
④ 履歴の削除（必要な場合）
   git filter-repo を使用
   ※ 強制プッシュが必要なため管理者と連携
      │
      ▼
⑤ ポストモーテム
   - 原因分析
   - 再発防止策の策定・実施
```

---

## 関連リンク

- [セキュリティ基準](../security-standards.html)
- [サポートメール](../support-email.html)
- [Chat 問い合わせ](../chat-support.html)
- [GitHub 公式: セキュリティベストプラクティス](https://docs.github.com/ja/code-security)

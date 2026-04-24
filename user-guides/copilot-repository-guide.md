# リポジトリにおける GitHub Copilot 取り扱いガイド

> **対象読者:** リポジトリの管理者・開発者（全員）  
> **目的:** 各リポジトリで Copilot を安全かつ効果的に活用するための設定・運用方法を解説  
> **関連規程:** [Copilot ガバナンス規程 第22条9号](../copilot-governance-policy.md)

---

## このガイドで分かること

1. リポジトリ単位で Copilot をどう設定するか
2. 何をリポジトリから除外すべきか（Content Exclusion）
3. Copilot が生成したコードの取り扱い・PR運用
4. リポジトリ管理者がやるべきこと

---

## 1. リポジトリの分類（Copilot 利用可否）

まずリポジトリの性質を分類し、Copilot の利用方針を決めます。

| カテゴリ | Copilot 利用 | Content Exclusion | 例 |
|---|---|---|---|
| **標準プロジェクト** | 許可 | 必要最小限 | 通常の業務アプリ・内部ツール |
| **機密プロジェクト** | 条件付き許可 | 機密ディレクトリを除外 | 顧客データ処理、認証基盤 |
| **極秘プロジェクト** | 原則禁止 | リポジトリ全体を除外 | 国防・暗号鍵管理、M&A 関連 |
| **顧客契約で禁止** | 全面禁止 | リポジトリ全体を除外 | AI利用禁止条項のある案件 |

リポジトリ作成時に、プロジェクト責任者がこの分類を決定し、README または SECURITY.md に明記してください。

---

## 2. リポジトリ作成時のチェックリスト

新しいリポジトリを作る際に必ず確認する項目です。

### 作成時（Day 0）

- [ ] リポジトリの可視性を **Private** または **Internal** に設定（Public 禁止）
- [ ] リポジトリのカテゴリ（上記分類）を決定・記録
- [ ] `.gitignore` に `.env`, `credentials.json`, `secrets/*` 等を追加
- [ ] 顧客案件の場合、Copilot 利用の契約確認（[クライアントチェックリスト](../copilot-client-checklist.md)）

### 初期設定（Day 1-7）

- [ ] **ブランチ保護ルール** を設定（`main` への直接 push 禁止）
- [ ] **CODEOWNERS** ファイルを作成
- [ ] **Copilot Code Review** を有効化
- [ ] **Secret Scanning** を有効化
- [ ] **Dependabot** を有効化
- [ ] **CodeQL**（Code Scanning）を有効化
- [ ] 必要に応じて **Content Exclusion** を設定

---

## 3. ブランチ保護ルール（必須設定）

Enterprise Rulesets で自動強制されますが、リポジトリ管理者も理解しておいてください。

### 最低限の設定（`main` ブランチ）

```
Settings → Branches → Branch protection rules → Add rule
```

| 設定 | 値 | 理由 |
|---|---|---|
| Require a pull request before merging | ✅ | 直接 push 禁止 |
| Required approving reviews | **1名以上**（重要PJは 2名） | レビュー必須 |
| Dismiss stale pull request approvals | ✅ | 承認後の改ざん防止 |
| Require review from Code Owners | ✅ | CODEOWNERS 必須 |
| Require conversation resolution | ✅ | 指摘の取りこぼし防止 |
| Require status checks to pass | ✅（CodeQL, CI, Secret Scanning） | 品質ゲート |
| Include administrators | ✅ | 管理者も例外なし |

---

## 4. CODEOWNERS の書き方

`.github/CODEOWNERS` ファイルを必ず配置してください。

### 基本形

```
# 全ファイルのデフォルトレビュアー
*                          @org/platform-team

# ドキュメント
/docs/**                   @org/docs-team

# セキュリティ関連コード（2名承認推奨）
/src/auth/**               @org/security-team @org/platform-team
/src/crypto/**             @org/security-team
/src/payment/**            @org/security-team @org/finance-team

# インフラ・デプロイ
/infra/**                  @org/sre-team @org/security-team
/.github/workflows/**      @org/sre-team

# AI生成コード・Copilot Agent 生成物が入る可能性が高い場所
/generated/**              @org/tech-lead @org/quality-team
```

### リスク別の承認ルール

Copilot ガバナンス規程 第22条9号に基づき、以下を目安にしてください。

| リスク | 必要承認 | 具体例 |
|---|---|---|
| 通常 | 1名 | ビジネスロジック、UI |
| セキュリティ関連 | 2名（セキュリティチーム必須） | 認証・認可・暗号・決済 |
| 本番デプロイ | 2名（SRE + Tech Lead） | インフラ・デプロイ設定 |
| AI生成コード（Coding Agent 等） | 2名（人間レビュアー） | Agent が作成した PR |

---

## 5. Content Exclusion（機密ファイルの除外）

Copilot の参照対象から特定のファイル・ディレクトリを除外する機能です。

### いつ使うか

- 機密情報を含むファイル（認証基盤、暗号処理など）
- 顧客固有の機密ロジック
- 学習させたくない社内独自実装

### 設定場所

**Organization レベル**（推奨）:
```
Organization Settings → Copilot → Content exclusion
```

**Repository レベル**:
```
Repository Settings → Copilot → Content exclusion
```

### 除外パターン例

```
# 機密ディレクトリ
/secrets/**
/config/production/**
/src/auth/keys/**

# 特定ファイル
**/credentials.*
**/private-key.*
**/*.pem
**/*.key

# 顧客固有コード
/clients/acme-corp/**
```

> **注意:** 除外設定を変更しても、反映には最大30分かかります。

---

## 6. Copilot Code Review の活用

2026年4月時点で GA の機能。PR に対して自動レビューを実行します。

### 有効化方法

```
Repository Settings → Code review → Copilot code review → Enable auto-review
```

### 運用ポイント

1. **一次スキャンとして活用** — 人間レビューの前に Copilot が自動コメント
2. **人間レビューは必ず実施** — 自動レビューは人間レビューの代替ではない（[第15条3項](../copilot-governance-policy.md)）
3. **指摘の優先度を判断** — AI の指摘がすべて正しいとは限らない。レビュアーが採否を判断
4. **レビュー記録は PR に残す** — 監査証跡として保管

### 人間レビューで特に確認すること

Copilot Code Review では拾いきれない観点:

- ビジネスロジックの正確性
- 既存コードベースとの整合性
- パフォーマンスへの影響
- 顧客契約・業界規制への適合

---

## 7. AI生成コードの取り扱い

### コミット時

- AI生成コードを**特別扱いしない**（通常コードと同じルールで扱う）
- ただし、PR 説明に「Copilot / Agent を使用した箇所」を明記するとレビュー効率が上がる
- 以下のような記載を推奨:

```markdown
## このPRについて

- Copilot Chat を使用してテストコードを生成（`tests/user_test.py`）
- Copilot 提案をベースに実装（`src/user.py`）
- レビュー時は特に以下を確認してほしい:
  - バリデーションロジックの正確性
  - エラーハンドリングの網羅性
```

### 禁止されるコミット

以下のコミットは**してはいけません**:

- [ ] レビューなしで AI 生成コードを main にマージ
- [ ] Copilot が提案した認証情報をそのままコミット
- [ ] 存在しない API を呼ぶコード（ビルド確認前のコミット）
- [ ] AI が生成した OSS ライセンス違反となり得るコード
- [ ] 顧客機密情報を含むコード（入力禁止情報を含んだまま生成されたもの）

---

## 8. Coding Agent（自動PR作成）の取り扱い

2026年4月から GA。Issue を Agent にアサインすると自動で PR を作成します。

### 利用条件

- **Organization レベルでの有効化が必須**（デフォルト無効）
- 利用許可のあるリポジトリでのみ使用
- プロジェクト責任者の事前承認を得ること

### Agent が作成した PR の扱い

1. **必ず2名以上の人間レビュー**が必要（[第15条4項](../copilot-governance-policy.md)）
2. Agent が実行した変更の妥当性を確認
3. CodeQL・Secret Scanning の結果を必ず確認（Agent はデフォルトで実行するが無効化禁止）
4. Agent のセッション履歴は Audit Log で確認可能

### 推奨ワークフロー

```
Issue 作成 → ラベル「copilot-agent」を付与 → Agent にアサイン
          → Agent が PR 作成 → 自動でCI/CD実行
          → 人間レビュー2名 → マージ
```

---

## 9. シークレット・機密情報の管理

### やること

- **GitHub Secrets** を使う（リポジトリ / Organization / Environment レベル）
- **環境変数** で機密値を渡す（ハードコード禁止）
- **Secret Scanning** を有効化して自動検知
- **プレコミットフック**（gitleaks, git-secrets 等）を導入

### やってはいけないこと

- [ ] ソースコードに API キーを直書き
- [ ] `.env` ファイルをコミット
- [ ] Copilot Chat にパスワードを貼り付け
- [ ] ログに機密情報を出力

### 万が一コミットしてしまったら

1. **絶対に削除コミットで隠さない**（履歴に残る）
2. 即座に該当のシークレットを無効化・ローテーション
3. `git filter-repo` で履歴から削除
4. [管理者へ報告](../support-email.html)

---

## 10. リポジトリ管理者のチェック項目（月次）

リポジトリ管理者は月1回、以下を確認してください。

- [ ] ブランチ保護ルールが解除されていないか
- [ ] CODEOWNERS が陳腐化していないか（退職者・異動者の削除）
- [ ] Content Exclusion 設定が適切か
- [ ] Copilot Code Review が有効か
- [ ] Secret Scanning の検出結果に対応したか
- [ ] Dependabot のアラートに対応したか
- [ ] 不要になった Collaborator を削除したか
- [ ] AI生成コード起因の不具合・インシデントがないか

---

## 11. よくある質問

### Q1. 個人のリポジトリ（personal account）で Copilot を使ってもいい？

業務目的では**禁止**です。必ず Organization のリポジトリで、会社の Business / Enterprise ライセンスを使ってください。

### Q2. フォークしたOSSリポジトリで Copilot を使ってもいい？

Organization 配下の Private / Internal リポジトリなら使用可能です。ただし上流の OSS ライセンスに注意してください。

### Q3. テスト用の使い捨てリポジトリでも設定が必要？

**必要です。** テストリポジトリに機密情報を入れてしまう事故が多いため、最低限 Private 化と Secret Scanning は有効化してください。

### Q4. Content Exclusion を設定したら Copilot が全く提案しなくなった

除外パターンが広すぎる可能性があります。`/**` を避け、具体的なパスを指定してください。また反映には最大30分かかります。

### Q5. 顧客からコードレビューを見せるよう言われた

CODEOWNERS・PR レビュー履歴・Audit Log は共有可能です。個別の Copilot 提案ログは原則として GitHub 側に保持されていないため、共有できません。

---

## 関連ドキュメント

| ドキュメント | 内容 |
|---|---|
| [Copilot 利用ガイド（開発者向け）](copilot-user-guide.md) | Copilot の基本的な使い方 |
| [Copilot ガバナンス規程](../copilot-governance-policy.md) | 全社ガバナンス規程（第22条9号に詳細） |
| [Copilot セキュリティ基準](../copilot-security-standards.html) | セキュリティルール |
| [クライアント確認チェックリスト](../copilot-client-checklist.md) | 顧客案件での事前確認 |
| [セキュリティ基準（全般）](../security-standards.html) | GitHub Enterprise 全般 |

---

## 困ったら

- 技術的な質問: [Chat サポート](../chat-support.html)
- 設定・ライセンス: [管理者へ問い合わせ](../support-email.html)
- ガバナンス関連: AI-CoE（AI推進委員会）

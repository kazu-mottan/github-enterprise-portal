# ユーザーガイド

GitHub Enterprise / Copilot を業務で利用する開発者・リポジトリ管理者向けのガイド集です。

## ガイド一覧

| ガイド | 対象 | 内容 |
|---|---|---|
| [Copilot 利用ガイド](copilot-user-guide.md) | 全利用者 | ライセンス・入力禁止情報・セットアップ・便利な使い方 |
| [リポジトリ取り扱いガイド](copilot-repository-guide.md) | リポジトリ管理者・開発者 | Rulesets / Copilot Code Review / CODEOWNERS / Content Exclusion / AI生成コードの扱い |
| [クライアント確認チェックリスト](copilot-client-checklist.md) | クライアント案件担当 | 顧客案件で Copilot を使う際の事前確認・トークスクリプト |
| [セキュアコードリポジトリガイド](secure-code-repository.md) | 全開発者 | ブランチ保護・CodeQL・Dependabot の基本設定 |

## 初めて Copilot を使う場合

1. [Copilot 利用ガイド](copilot-user-guide.md) を読む
2. [Responsible Use 同意書](../copilot-responsible-use.html) を提出
3. 管理者から Business ライセンスの割り当てを受ける
4. エディタに拡張をインストール

## リポジトリを新規作成する場合

[リポジトリ取り扱いガイド](copilot-repository-guide.md) および [セキュアコードリポジトリガイド](secure-code-repository.md) を参照し、以下を設定してください:

- [ ] Private / Internal リポジトリ
- [ ] Rulesets（ブランチ保護・Copilot 自動レビュー）
- [ ] CODEOWNERS
- [ ] Secret Scanning / Dependabot / CodeQL
- [ ] 必要に応じて Content Exclusion

## クライアント案件を始める場合

1. [クライアント確認チェックリスト](copilot-client-checklist.md) を実施
2. [Copilot ガバナンス規程](../policies/copilot-governance-policy.md) 第22条（クライアント案件）を確認

## 関連ドキュメント

- [Copilot ガバナンス規程](../policies/copilot-governance-policy.md)
- [Copilot データプライバシーポリシー](../policies/copilot-data-privacy.md)
- [Copilot セキュリティ基準](../copilot-security-standards.html)
- [Responsible AI ガバナンス設計書テンプレート](../templates/governance-ai-design-template.md)

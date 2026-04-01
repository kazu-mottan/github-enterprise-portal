# GitHub Enterprise ポータル

社内の GitHub Enterprise を利用するメンバー向けのポータルサイトです。

🌐 **GitHub Pages:** https://kazu-mottan.github.io/github-enterprise-portal/

---

## 構成

```
github-enterprise-portal/
├── index.html                      # ポータルのメインページ
├── style.css                       # 共通スタイルシート
│
├── github-copilot.html             # GitHub Copilot — ライセンス・使い方・データプライバシー
├── copilot-responsible-use.html    # Responsible Use of GitHub Copilot（同意書フォーム）
├── copilot-data-privacy.md         # Copilot データプライバシーポリシー
├── copilot-client-checklist.md     # Copilot クライアントとの確認事項チェックリスト
│
├── support-email.html              # サポートメール問い合わせ先・対応時間
├── security-standards.html         # セキュリティ基準
├── chat-support.html               # Chat 問い合わせ（Slack / Teams）
│
├── secure-code-repository.md       # セキュアコードリポジトリガイド
└── project-draft.md                # 企画ドラフト
```

## コンテンツ

### ポータル（HTML）
- **トップページ** — GitHub Enterprise の概要・機能紹介・はじめかた・FAQ・支援リンク
- **GitHub Copilot** — ライセンス比較・データプライバシー・使い方・セットアップ
- **Responsible Use 同意書** — Copilot 利用前に全員が提出する同意フォーム
- **サポートメール** — 問い合わせ先・優先度別の対応時間
- **セキュリティ基準** — PAT 管理・リポジトリ設定・シークレット管理ルール
- **Chat 問い合わせ** — Slack / Teams でのサポートチャンネル案内

### ドキュメント（Markdown）
- **Copilot データプライバシー** — Business ライセンスのデータ保護方針の詳細
- **Copilot クライアントチェックリスト** — クライアント案件で Copilot を使う際の確認事項・トークスクリプト
- **セキュアコードリポジトリ** — ブランチ保護・CodeQL・Dependabot 設定ガイド
- **企画ドラフト** — GitHub Enterprise 導入企画書

## GitHub Pages

本リポジトリは GitHub Pages で公開されています。

| ページ | URL |
|---|---|
| トップ | https://kazu-mottan.github.io/github-enterprise-portal/ |
| GitHub Copilot | https://kazu-mottan.github.io/github-enterprise-portal/github-copilot.html |
| 同意書フォーム | https://kazu-mottan.github.io/github-enterprise-portal/copilot-responsible-use.html |
| サポートメール | https://kazu-mottan.github.io/github-enterprise-portal/support-email.html |
| セキュリティ基準 | https://kazu-mottan.github.io/github-enterprise-portal/security-standards.html |
| Chat 問い合わせ | https://kazu-mottan.github.io/github-enterprise-portal/chat-support.html |

`master` ブランチへの push で自動的にデプロイされます。

## 更新・管理

内容の変更は Pull Request 経由で行ってください。
お問い合わせは社内チケットシステムより管理者にご連絡ください。

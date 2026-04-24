# GitHub Enterprise ポータル

社内の GitHub Enterprise を利用するメンバー向けのポータルサイトです。

🌐 **GitHub Pages:** https://kazu-mottan.github.io/github-enterprise-portal/

---

## 構成

```
github-enterprise-portal/
├── index.html                             # ポータルのメインページ
├── style.css                              # 共通スタイルシート
│
├── 🌐 ポータルページ（HTML・ルート維持）
├── github-copilot.html                    # GitHub Copilot — ライセンス・使い方・データプライバシー
├── copilot-responsible-use.html           # Responsible Use of GitHub Copilot（同意書フォーム）
├── copilot-security-standards.html        # セキュリティ基準（Copilot版）
├── security-standards.html                # セキュリティ基準（GitHub Enterprise 全般）
├── support-email.html                     # サポートメール問い合わせ先・対応時間
├── chat-support.html                      # Chat 問い合わせ（Slack / Teams）
│
├── 📋 policies/                           # 全社規程・ポリシー
│   ├── copilot-governance-policy.md       #   Copilot 利用ガバナンス・運用規程（JDLA準拠・全39条14章）
│   └── copilot-data-privacy.md            #   Copilot データプライバシーポリシー
│
├── 📝 user-guides/                        # 実務者向けガイド
│   ├── README.md                          #   ガイド一覧・初期セットアップ
│   ├── copilot-user-guide.md              #   Copilot 利用ガイド（開発者向け）
│   ├── copilot-repository-guide.md        #   リポジトリ取り扱いガイド（Rulesets / Copilot Code Review / CODEOWNERS）
│   ├── copilot-client-checklist.md        #   クライアント案件での確認チェックリスト
│   └── secure-code-repository.md          #   セキュアコードリポジトリ運用ガイド
│
├── 📐 templates/                          # 他部門でも流用可能なテンプレート
│   └── governance-ai-design-template.md   #   Responsible AI ガバナンス設計書テンプレート
│
└── 📝 drafts/                             # 企画書・作業中ドキュメント
    └── project-draft.md                   #   GitHub Enterprise 導入企画書
```

## コンテンツ

### 🌐 ポータル（HTML）
| ページ | 内容 |
|---|---|
| [index.html](index.html) | GitHub Enterprise の概要・機能紹介・はじめかた・FAQ・支援リンク |
| [github-copilot.html](github-copilot.html) | ライセンス比較・データプライバシー・使い方・セットアップ |
| [copilot-responsible-use.html](copilot-responsible-use.html) | Copilot 利用前に全員が提出する同意フォーム |
| [copilot-security-standards.html](copilot-security-standards.html) | Copilot 固有のセキュリティルール（入力保護・コード検証・MCP/Agent・監査） |
| [security-standards.html](security-standards.html) | PAT 管理・リポジトリ設定・シークレット管理ルール |
| [support-email.html](support-email.html) | 問い合わせ先・優先度別の対応時間 |
| [chat-support.html](chat-support.html) | Slack / Teams でのサポートチャンネル案内 |

### 📋 policies/ — 全社規程
| ドキュメント | 内容 |
|---|---|
| [Copilot ガバナンス・運用規程](policies/copilot-governance-policy.md) | JDLA準拠の全39条14章（禁止用途・利用範囲・データ入力・知的財産・品質管理・研修・インシデント対応） |
| [Copilot データプライバシーポリシー](policies/copilot-data-privacy.md) | Business ライセンスのデータ保護方針の詳細 |

### 📝 user-guides/ — 実務者向けガイド
| ドキュメント | 対象 | 内容 |
|---|---|---|
| [Copilot 利用ガイド](user-guides/copilot-user-guide.md) | 全開発者 | ライセンス・入力禁止情報・セットアップ・便利な使い方 |
| [リポジトリ取り扱いガイド](user-guides/copilot-repository-guide.md) | リポジトリ管理者 | Rulesets / Copilot Code Review / CODEOWNERS / Content Exclusion |
| [クライアント確認チェックリスト](user-guides/copilot-client-checklist.md) | クライアント案件担当 | 顧客案件で Copilot を使う際の事前確認・トークスクリプト |
| [セキュアコードリポジトリガイド](user-guides/secure-code-repository.md) | 全開発者 | ブランチ保護・CodeQL・Dependabot の基本設定 |

### 📐 templates/ — 設計テンプレート
| ドキュメント | 対象 | 内容 |
|---|---|---|
| [Responsible AI ガバナンス設計書テンプレート](templates/governance-ai-design-template.md) | 情シス・管理者 | ISO 42001 / NIST AI RMF / EU AI Act 対応の設計テンプレート |

### 📝 drafts/ — 企画・作業中
| ドキュメント | 内容 |
|---|---|
| [企画ドラフト](drafts/project-draft.md) | GitHub Enterprise 導入企画書 |

---

## GitHub Pages 公開URL

本リポジトリは GitHub Pages で公開されています。HTML ファイルはルートに配置されているため、既存URLと互換性があります。

| ページ | URL |
|---|---|
| トップ | https://kazu-mottan.github.io/github-enterprise-portal/ |
| GitHub Copilot | https://kazu-mottan.github.io/github-enterprise-portal/github-copilot.html |
| 同意書フォーム | https://kazu-mottan.github.io/github-enterprise-portal/copilot-responsible-use.html |
| セキュリティ基準（Copilot版） | https://kazu-mottan.github.io/github-enterprise-portal/copilot-security-standards.html |
| セキュリティ基準（全般） | https://kazu-mottan.github.io/github-enterprise-portal/security-standards.html |
| サポートメール | https://kazu-mottan.github.io/github-enterprise-portal/support-email.html |
| Chat 問い合わせ | https://kazu-mottan.github.io/github-enterprise-portal/chat-support.html |

`main` ブランチへの push で自動的にデプロイされます。

---

## 用途別の入口

### 🆕 初めて Copilot を使う人
1. [Copilot 利用ガイド](user-guides/copilot-user-guide.md) を読む
2. [Responsible Use 同意書](copilot-responsible-use.html) を提出
3. 管理者から Business ライセンスの割り当てを受ける

### 🛠️ リポジトリを新規作成する管理者
1. [リポジトリ取り扱いガイド](user-guides/copilot-repository-guide.md) を参照
2. [セキュアコードリポジトリガイド](user-guides/secure-code-repository.md) で基本設定を確認
3. Rulesets / CODEOWNERS / Secret Scanning / Dependabot / CodeQL を設定

### 🏢 クライアント案件を始める担当者
1. [クライアント確認チェックリスト](user-guides/copilot-client-checklist.md) を実施
2. [Copilot ガバナンス規程](policies/copilot-governance-policy.md) 第22条（クライアント案件）を確認

### 🏛️ AI ガバナンスを設計する管理者
1. [Copilot ガバナンス規程](policies/copilot-governance-policy.md) を読む
2. [Responsible AI ガバナンス設計書テンプレート](templates/governance-ai-design-template.md) をコピーして組織向けに編集

---

## 更新・管理

内容の変更は Pull Request 経由で行ってください。
お問い合わせは社内チケットシステムより管理者にご連絡ください。

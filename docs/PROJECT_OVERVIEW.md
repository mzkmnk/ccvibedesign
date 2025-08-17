# CC Vibe Design - プロジェクト概要

## プロジェクトの目的
Claude Codeを活用した設計駆動開発（Design-Driven Development）ツールの開発。AWSのKiroのようなアプローチで、設計と実装の一貫性を保ちながら効率的な開発を実現します。

## 主要ドキュメント

### 📋 要件定義
- [要件定義書](./requirements/requirements-definition.md) - 機能要件、非機能要件、ユースケース

### 🏗 アーキテクチャ
- [技術スタック選定書](./architecture/tech-stack.md) - 使用技術とその選定理由
- [アーキテクチャ設計書](./architecture/architecture-design.md) - システム構成、API設計、データフロー

### 📅 計画
- [プロジェクトロードマップ](./planning/roadmap.md) - 開発フェーズ、マイルストーン、リリース計画

## 技術スタック概要

### フロントエンド
- **Framework**: Next.js 14 (App Router)
- **UI Library**: shadcn/ui + Tailwind CSS
- **State Management**: Zustand
- **Terminal**: xterm.js

### バックエンド
- **Runtime**: Node.js + Express
- **Terminal Control**: node-pty
- **WebSocket**: Socket.io
- **AI Integration**: Anthropic SDK

### 開発ツール
- **Package Manager**: pnpm
- **Build Tool**: tsup
- **Test Framework**: Vitest
- **E2E Test**: Playwright

## クイックスタート（実装後）

```bash
# グローバルインストール
npm install -g ccvibedesign

# または npx で実行
npx ccvibedesign init my-project

# サーバー起動
ccvibedesign start
```

## プロジェクト構造

```
ccvibedesign/
├── docs/                 # ドキュメント
│   ├── requirements/     # 要件定義
│   ├── architecture/     # アーキテクチャ設計
│   └── planning/         # 計画資料
├── packages/             # モノレポパッケージ（今後実装）
│   ├── cli/              # CLIツール
│   ├── core/             # コアロジック
│   ├── server/           # バックエンドサーバー
│   ├── web/              # Webフロントエンド
│   └── shared/           # 共有型定義
└── templates/            # プロジェクトテンプレート
```

## 主要機能

1. **設計ドキュメント生成** - 対話形式での要件定義から設計書を自動生成
2. **コード生成** - 設計書からコードスケルトンを生成
3. **設計検証** - ベストプラクティスとの照合と改善提案
4. **インタラクティブUI** - ブラウザベースの直感的な操作環境
5. **ターミナル統合** - ブラウザ内での開発作業実行

## 開発ステータス

- ✅ 要件定義完了
- ✅ 技術選定完了
- ✅ アーキテクチャ設計完了
- ✅ ロードマップ策定完了
- ⏳ 実装開始準備中

## 次のアクション

1. プロジェクト基盤のセットアップ
   - package.json作成
   - TypeScript設定
   - モノレポ構造の構築

2. 基本機能の実装
   - CLIエントリーポイント
   - Expressサーバー
   - Next.jsアプリケーション

## コントリビューション
現在は初期開発フェーズのため、コントリビューションガイドラインは準備中です。

## ライセンス
（検討中）

## お問い合わせ
（準備中）

---
*最終更新: 2025年1月17日*
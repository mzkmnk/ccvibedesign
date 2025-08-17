# 技術スタック選定書

## 1. 選定基準

### 1.1 必須要件
- TypeScriptのみでの実装
- npx/npm globalでの実行対応
- ローカル環境での動作
- 高い開発効率とメンテナンス性

### 1.2 評価軸
- 学習コスト
- エコシステムの充実度
- パフォーマンス
- 開発者体験（DX）
- コミュニティサポート

## 2. フロントエンド技術スタック

### 2.1 フレームワーク選定: **Angular v20** (2025年5月リリース)

#### 選定理由
- **エンタープライズグレード**: 大規模アプリケーションに最適な構造
- **完全なTypeScript統合**: TypeScript 5.8必須、ファーストクラスのサポート
- **包括的なフレームワーク**: ルーティング、フォーム、HTTPクライアント等が標準装備
- **強力なCLI**: コード生成とプロジェクト管理の自動化
- **Signals API (Stable)**: v20で安定版となったリアクティブ状態管理
- **Zoneless Change Detection**: Developer Previewで利用可能な新しい変更検知
- **Incremental Hydration**: v20で安定版、パフォーマンス向上

#### 他の選択肢との比較
| フレームワーク | メリット | デメリット | 採用判断 |
|--------------|---------|-----------|---------|
| Angular v20 | フル機能、型安全、企業向け | 学習曲線あり | ✓ |
| React | 柔軟性、大規模エコシステム | 追加ライブラリが必要 | ✗ |
| Vue 3 | シンプル、段階的採用可能 | TypeScript統合がやや劣る | ✗ |

### 2.2 UIコンポーネントライブラリ: **PrimeNG v20 + Tailwind CSS v3.4**

#### 選定理由
- **PrimeNG**:
  - Angularネイティブの包括的なコンポーネントライブラリ
  - 80以上の高品質UIコンポーネント
  - アクセシビリティ標準準拠
  - テーマカスタマイズ機能
  - データテーブル、チャート、フォーム等の高度なコンポーネント

- **Tailwind CSS**:
  - ユーティリティファーストのアプローチ
  - 高速なスタイリング
  - PrimeNGコンポーネントの細かいカスタマイズ
  - レスポンシブデザインの簡易実装

### 2.3 状態管理: **NgRx v20 (SignalStore)**

#### 選定理由
- **SignalStore成熟**: v20で本格的な状態管理ソリューションに
- **Angular統合**: フレームワークとの完全な統合
- **Signals完全対応**: Angular v20の安定版Signals API活用
- **DevTools**: 強力なデバッグツール
- **エフェクト管理**: 副作用の明確な分離
- **Zoneless対応**: Angular v20のZoneless変更検知に対応

## 3. バックエンド技術スタック

### 3.1 ランタイム: **Node.js + Express**

#### 選定理由
- **統一言語**: フロントエンドと同じTypeScript
- **軽量で柔軟**: 必要最小限の機能から開始可能
- **成熟したエコシステム**: 豊富なミドルウェア

### 3.2 Claude Code統合: **Child Process + Command Line Interface**

#### 選定理由
- **直接統合**: ユーザーのClaude Codeインスタンスを活用
- **child_process**: Node.jsの標準機能でClaude Codeコマンド実行
- **ストリーム処理**: リアルタイムでの出力取得
- **セッション管理**: 複数のClaude Codeコンテキストの管理

### 3.3 WebSocket通信: **Socket.io**

#### 選定理由
- **信頼性**: 自動再接続、フォールバック機能
- **使いやすさ**: シンプルなイベントベースAPI
- **TypeScript対応**: 型定義の充実
- **Angular統合**: ngx-socket-ioによる簡単な統合

## 4. 開発ツール・ビルドツール

### 4.1 パッケージマネージャ: **pnpm**

#### 選定理由
- **高速**: npm/yarnより高速なインストール
- **ディスク効率**: ハードリンクによる重複排除
- **厳格な依存関係**: より安全な依存関係管理
- **ワークスペース**: モノレポ対応

### 4.2 ビルドツール: **Angular CLI + Vite**

#### 選定理由
- **Angular CLI**: 標準的なAngularビルドパイプライン
- **Vite統合**: 開発サーバーの高速化（Angular v17+）
- **esbuild**: 高速なTypeScriptトランスパイル
- **最適化**: Tree-shaking、コード分割の自動化

### 4.3 CLIフレームワーク: **Commander.js + Inquirer.js**

#### 選定理由
- **Commander.js**: 業界標準のCLI構築ライブラリ
- **Inquirer.js**: 対話的なプロンプトの実装
- **TypeScript対応**: 完全な型定義

## 5. データ管理

### 5.1 ローカルストレージ: **LowDB**

#### 選定理由
- **シンプル**: JSONベースの軽量データベース
- **TypeScript対応**: 型安全なデータ操作
- **依存関係なし**: 追加のランタイム不要

### 5.2 設定管理: **Cosmiconfig**

#### 選定理由
- **柔軟な設定ファイル**: 複数形式対応（JSON, YAML, JS）
- **標準的**: 多くのツールで採用される規約
- **カスケーディング**: 階層的な設定の読み込み

## 6. チャットUI実装

### 6.1 メッセージング: **Angular Material CDK**

#### 選定理由
- **仮想スクロール**: 大量メッセージの効率的な表示
- **アクセシビリティ**: ARIA標準準拠
- **カスタマイズ性**: 独自のチャットUIコンポーネント構築

### 6.2 差分表示: **Monaco Editor + diff2html**

#### 選定理由
- **Monaco Editor**: VS Codeと同じエディタエンジン
- **diff2html**: Git差分の美しい表示
- **シンタックスハイライト**: 多言語対応
- **インタラクティブ**: 差分の詳細確認機能

## 7. ドキュメント処理

### 7.1 Markdown処理: **marked + highlight.js**

#### 選定理由
- **marked**: 高速で軽量なMarkdownパーサー
- **highlight.js**: 多言語対応のコードハイライト
- **Angular統合**: ngx-markdownによる簡単な統合

### 7.2 図表生成: **Mermaid.js**

#### 選定理由
- **テキストベース**: バージョン管理しやすい
- **豊富な図表**: フローチャート、シーケンス図、ER図など
- **Angular対応**: ngx-mermaidによる統合

## 8. テスト・品質管理

### 8.1 ユニットテスト: **Vitest**

#### 選定理由
- **高速実行**: Viteベースによる圧倒的な速度
- **Angular対応**: @angular-vitest/builderによる完全統合
- **モダンなAPI**: Jest互換でより直感的
- **HMR対応**: テストのホットリロード対応
- **並列実行**: デフォルトで並列テスト実行
- **スナップショット**: UIコンポーネントのスナップショットテスト対応

### 8.2 E2Eテスト: **Playwright**

#### 選定理由
- **クロスブラウザ**: 主要ブラウザ全対応
- **高速・安定**: 自動待機機能
- **TypeScript対応**: 完全な型サポート
- **Angular統合**: @playwright/test による統合

### 8.3 リンター/フォーマッター

- **ESLint**: 静的解析（Angular ESLint）
- **Prettier**: コードフォーマット
- **Stylelint**: CSS/SCSS のリンティング

## 9. 技術スタックサマリー

```typescript
// package.json dependencies (概要)
{
  "dependencies": {
    // Core
    "@angular/animations": "^20.0.0",
    "@angular/common": "^20.0.0",
    "@angular/compiler": "^20.0.0",
    "@angular/core": "^20.0.0",
    "@angular/forms": "^20.0.0",
    "@angular/platform-browser": "^20.0.0",
    "@angular/platform-browser-dynamic": "^20.0.0",
    "@angular/router": "^20.0.0",
    "typescript": "^5.8.0",
    
    // UI
    "primeng": "^20.0.0",
    "@primeuix/themes": "^20.0.0",
    "@primeuix/styles": "^20.0.0",
    "primeicons": "^7.0.0",
    "tailwindcss": "^3.4.0",
    
    // State Management
    "@ngrx/store": "^20.0.0",
    "@ngrx/effects": "^20.0.0",
    "@ngrx/entity": "^20.0.0",
    "@ngrx/signals": "^20.0.0",
    
    // Backend
    "express": "^4.21.0",
    "socket.io": "^4.8.0",
    "socket.io-client": "^4.8.0",
    
    // CLI
    "commander": "^12.0.0",
    "inquirer": "^10.0.0",
    
    // Utils
    "lowdb": "^7.0.0",
    "cosmiconfig": "^9.0.0",
    "rxjs": "^7.8.0",
    "zone.js": "^0.15.0",
    
    // Documentation & Diff
    "marked": "^14.0.0",
    "highlight.js": "^11.10.0",
    "mermaid": "^11.0.0",
    "monaco-editor": "^0.52.0",
    "diff2html": "^3.4.0"
  },
  "devDependencies": {
    "@angular-devkit/build-angular": "^20.0.0",
    "@angular/cli": "^20.0.0",
    "@angular/compiler-cli": "^20.0.0",
    "@types/node": "^20.0.0",
    "vitest": "^2.0.0",
    "@vitest/ui": "^2.0.0",
    "@angular-vitest/builder": "^1.0.0",
    "jsdom": "^25.0.0",
    "@playwright/test": "^1.48.0",
    "eslint": "^9.0.0",
    "prettier": "^3.3.0",
    "stylelint": "^16.0.0"
  }
}
```

## 10. アーキテクチャの利点

### 10.1 開発効率
- TypeScript統一による型安全性
- Angularの強力なCLIによる高速開発
- PrimeNGの豊富なコンポーネントで実装時間短縮

### 10.2 保守性
- Angularの明確な構造とベストプラクティス
- 依存性注入による疎結合
- テスタビリティの確保

### 10.3 拡張性
- モジュラーアーキテクチャ
- スタンドアロンコンポーネント（Angular v14+）
- 遅延ロードによる最適化

## 11. 実装優先順位

### Phase 1: 基盤構築
- Angularプロジェクトの初期設定
- 基本的なレイアウト実装（サイドバー、チャット画面）
- Express サーバーの構築

### Phase 2: Claude Code統合
- child_processによるClaude Code実行
- WebSocketによるリアルタイム通信
- セッション管理の実装

### Phase 3: UI実装
- チャットインターフェース
- 差分表示機能
- 要件定義・設計・TODOビュー

### Phase 4: 品質向上
- テスト実装
- エラーハンドリング
- パフォーマンス最適化
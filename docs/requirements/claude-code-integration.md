# Claude Code 統合仕様

## 1. 概要

CC Vibe Designは、ユーザーが既に保有しているClaude Codeサブスクリプションを活用して、設計駆動開発を実現するツールです。

## 2. 前提条件

### 2.1 必要な環境
- Claude Codeのアクティブなサブスクリプション
- Claude Codeがローカル環境で実行可能
- Node.js 18.x以上

### 2.2 動作原理
- ユーザーのClaude Codeインスタンスをバックエンドとして利用
- CC Vibe DesignはClaude Codeへのインターフェースとして機能
- APIキーや認証情報の管理は不要（ユーザーのClaude Code経由）

## 3. 統合アーキテクチャ

```
┌─────────────────────────────────────┐
│         CC Vibe Design UI           │
│     (Angular + PrimeNG + Tailwind)  │
└─────────────────┬───────────────────┘
                  │
                  ▼
┌─────────────────────────────────────┐
│      Local Express Server           │
│     (コマンド変換・管理層)          │
└─────────────────┬───────────────────┘
                  │
                  ▼
┌─────────────────────────────────────┐
│   ユーザーのClaude Codeインスタンス │
│    (実際のAI処理・コード生成)       │
└─────────────────────────────────────┘
```

## 4. 通信方式

### 4.1 コマンド実行
- CC Vibe DesignからClaude Codeへコマンドラインインターフェース経由で指示
- 子プロセスとしてClaude Codeコマンドを実行
- 出力をパースして構造化データとして返却

### 4.2 セッション管理
- 各対話セッションを独立したClaude Codeコンテキストとして管理
- 履歴の保持とコンテキストの切り替え
- 複数セッションの並行処理サポート

## 5. 主要機能の実装方法

### 5.1 設計ドキュメント生成
```typescript
interface DesignGenerationRequest {
  sessionId: string;
  requirements: string;
  context?: string;
}

// Claude Codeへのプロンプト変換
function generateDesignPrompt(req: DesignGenerationRequest): string {
  return `
    以下の要件に基づいて設計ドキュメントを生成してください：
    ${req.requirements}
    
    出力形式：
    - システム設計書
    - API仕様
    - データモデル
  `;
}
```

### 5.2 コード生成
```typescript
interface CodeGenerationRequest {
  sessionId: string;
  design: DesignDocument;
  targetLanguage: string;
  framework?: string;
}

// 設計書からコード生成プロンプトへの変換
function generateCodePrompt(req: CodeGenerationRequest): string {
  return `
    以下の設計書に基づいて${req.targetLanguage}のコードを生成してください：
    ${JSON.stringify(req.design)}
    
    フレームワーク: ${req.framework || 'なし'}
  `;
}
```

## 6. データフロー

### 6.1 チャット対話フロー
1. ユーザーがチャットUIで要件を入力
2. CC Vibe Designがプロンプトを構築
3. Claude Codeコマンドを実行
4. レスポンスをパース・構造化
5. UIに差分表示形式で表示

### 6.2 成果物管理フロー
1. 生成された設計・コードを一時保存
2. Git差分形式で変更を可視化
3. ユーザーの承認後にファイルシステムへ反映
4. バージョン管理との統合

## 7. エラーハンドリング

### 7.1 Claude Code未インストール
- 明確なエラーメッセージ表示
- インストールガイドへのリンク提供

### 7.2 サブスクリプション期限切れ
- Claude Codeからのエラーメッセージを解析
- ユーザーへの適切な通知

### 7.3 接続エラー
- リトライ機構の実装
- オフラインモードの提供（閲覧のみ）

## 8. セキュリティ考慮事項

### 8.1 データ保護
- すべての処理はローカル環境で完結
- 外部サーバーへのデータ送信なし
- ユーザーのClaude Code設定を尊重

### 8.2 権限管理
- ファイルシステムへのアクセスは最小限
- ユーザー承認なしでのファイル変更禁止

## 9. パフォーマンス最適化

### 9.1 レスポンスストリーミング
- Claude Codeからの出力を逐次処理
- チャンク単位でのUI更新

### 9.2 キャッシング
- 生成済み設計のローカルキャッシュ
- セッション情報の永続化

## 10. 拡張性

### 10.1 プラグインシステム
- カスタムプロンプトテンプレート
- 追加の検証ルール
- 出力フォーマッタ

### 10.2 他AIツールとの連携可能性
- 抽象化されたAIインターフェース
- 将来的な他ツール対応の余地
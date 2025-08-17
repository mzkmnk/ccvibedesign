# UIデザイン仕様書

## 1. 概要

CC Vibe Designは、チャット形式の対話を通じて設計駆動開発を支援するWebアプリケーションです。左側にセッション管理、中央にチャット画面、右側に成果物表示エリアを配置した3ペインレイアウトを採用します。

## 2. レイアウト構成

### 2.1 全体レイアウト

```
┌─────────────────────────────────────────────────────────────────┐
│                         ヘッダー (60px)                          │
├──────────────┬──────────────────────┬──────────────────────────┤
│              │                      │                          │
│   サイドバー  │    チャットエリア     │    コンテンツビュー      │
│    (280px)   │      (flex: 1)       │        (400px)          │
│              │                      │                          │
│              │                      │                          │
└──────────────┴──────────────────────┴──────────────────────────┘
```

### 2.2 レスポンシブ対応

- **デスクトップ**: 3ペイン表示（1440px以上）
- **タブレット**: サイドバー折りたたみ可能（768px-1439px）
- **モバイル**: シングルペインで切り替え（767px以下）

## 3. コンポーネント詳細

### 3.1 ヘッダー

#### 構成要素
- **ロゴ・タイトル**: 左端に「CC Vibe Design」
- **ステータス表示**: Claude Code接続状態インジケーター
- **ユーザーメニュー**: 設定、ヘルプ、情報

#### スタイル
```scss
.header {
  height: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
```

### 3.2 サイドバー（セッション管理）

#### 構成要素

##### セッションリスト
```typescript
interface Session {
  id: string;
  title: string;
  createdAt: Date;
  lastModified: Date;
  status: 'active' | 'completed' | 'archived';
  summary?: string;
}
```

##### UI要素
- **新規セッションボタン**: 上部に配置、プライマリカラー
- **検索バー**: セッションのフィルタリング
- **セッションカード**: 
  - タイトル（編集可能）
  - 最終更新時刻
  - ステータスバッジ
  - 削除・アーカイブボタン（ホバー時表示）

##### スタイル
```scss
.sidebar {
  width: 280px;
  background: #f8f9fa;
  border-right: 1px solid #e0e0e0;
  overflow-y: auto;
  
  .session-card {
    padding: 12px 16px;
    margin: 8px;
    background: white;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
    
    &:hover {
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      transform: translateY(-2px);
    }
    
    &.active {
      border-left: 3px solid #667eea;
      background: #f0f4ff;
    }
  }
}
```

### 3.3 チャットエリア

#### 構成要素

##### メッセージ表示
```typescript
interface ChatMessage {
  id: string;
  type: 'user' | 'assistant' | 'system';
  content: string;
  timestamp: Date;
  status?: 'sending' | 'delivered' | 'error';
  attachments?: Attachment[];
}
```

##### UIコンポーネント
- **メッセージバブル**:
  - ユーザーメッセージ: 右寄せ、青系背景
  - アシスタントメッセージ: 左寄せ、白背景
  - システムメッセージ: 中央寄せ、グレー背景

- **入力エリア**:
  - 複数行対応のテキストエリア
  - ファイル添付ボタン
  - 送信ボタン（Enterでも送信可）
  - 文字数カウンター

- **タイピングインジケーター**: Claude Code処理中の表示

##### スタイル
```scss
.chat-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  background: white;
  
  .messages-container {
    flex: 1;
    overflow-y: auto;
    padding: 20px;
    
    .message {
      margin-bottom: 16px;
      display: flex;
      
      &.user {
        justify-content: flex-end;
        
        .bubble {
          background: #667eea;
          color: white;
          border-radius: 18px 18px 4px 18px;
        }
      }
      
      &.assistant {
        justify-content: flex-start;
        
        .bubble {
          background: #f1f3f5;
          color: #333;
          border-radius: 18px 18px 18px 4px;
        }
      }
      
      .bubble {
        max-width: 70%;
        padding: 12px 16px;
        word-wrap: break-word;
      }
    }
  }
  
  .input-area {
    border-top: 1px solid #e0e0e0;
    padding: 16px;
    background: #fafafa;
    
    .input-container {
      display: flex;
      align-items: flex-end;
      gap: 12px;
      
      textarea {
        flex: 1;
        min-height: 44px;
        max-height: 120px;
        padding: 10px 14px;
        border: 1px solid #d0d0d0;
        border-radius: 22px;
        resize: none;
        
        &:focus {
          outline: none;
          border-color: #667eea;
        }
      }
    }
  }
}
```

### 3.4 コンテンツビュー

#### タブ構成
1. **要件定義タブ**
2. **設計タブ**
3. **TODOリストタブ**

#### 3.4.1 要件定義タブ

##### 表示内容
- 構造化された要件リスト
- 機能要件・非機能要件の分類
- 優先度表示
- 編集モード切り替え

##### UIコンポーネント
```typescript
interface Requirement {
  id: string;
  category: 'functional' | 'non-functional';
  priority: 'high' | 'medium' | 'low';
  title: string;
  description: string;
  status: 'draft' | 'approved' | 'implemented';
}
```

#### 3.4.2 設計タブ

##### 表示内容
- システムアーキテクチャ図（Mermaid.js）
- API仕様書
- データモデル定義
- シーケンス図

##### 差分表示機能
```typescript
interface DesignDiff {
  type: 'added' | 'modified' | 'deleted';
  path: string;
  oldValue?: any;
  newValue?: any;
  diff?: string; // Git形式の差分
}
```

##### UIコンポーネント
- **ツリービュー**: 設計ドキュメントの階層表示
- **差分ビューア**: Monaco Editorベースの差分表示
- **承認/却下ボタン**: 変更の適用制御

#### 3.4.3 TODOリストタブ

##### 表示内容
```typescript
interface TodoItem {
  id: string;
  title: string;
  description?: string;
  status: 'pending' | 'in-progress' | 'completed';
  priority: 'high' | 'medium' | 'low';
  assignee?: string;
  dueDate?: Date;
  tags?: string[];
}
```

##### UIコンポーネント
- **カンバンビュー**: ドラッグ&ドロップ対応
- **リストビュー**: 詳細表示モード
- **フィルター**: ステータス、優先度、タグ別
- **進捗バー**: 完了率の視覚化

##### スタイル
```scss
.content-view {
  width: 400px;
  background: #fafbfc;
  border-left: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  
  .tabs {
    display: flex;
    border-bottom: 1px solid #e0e0e0;
    background: white;
    
    .tab {
      flex: 1;
      padding: 12px;
      text-align: center;
      cursor: pointer;
      border-bottom: 2px solid transparent;
      transition: all 0.2s;
      
      &:hover {
        background: #f8f9fa;
      }
      
      &.active {
        border-bottom-color: #667eea;
        color: #667eea;
        font-weight: 600;
      }
    }
  }
  
  .tab-content {
    flex: 1;
    overflow-y: auto;
    padding: 16px;
  }
}
```

## 4. カラーパレット

### プライマリカラー
- **メイン**: #667eea
- **ライト**: #818cf8
- **ダーク**: #4f46e5

### セカンダリカラー
- **メイン**: #764ba2
- **ライト**: #9061b8
- **ダーク**: #5a3a7e

### ニュートラルカラー
- **背景**: #ffffff
- **サーフェス**: #f8f9fa
- **ボーダー**: #e0e0e0
- **テキスト**: #333333
- **サブテキスト**: #666666

### ステータスカラー
- **成功**: #10b981
- **警告**: #f59e0b
- **エラー**: #ef4444
- **情報**: #3b82f6

## 5. タイポグラフィ

### フォントファミリー
```scss
$font-primary: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
$font-mono: 'JetBrains Mono', 'Courier New', monospace;
```

### フォントサイズ
- **見出し1**: 24px / 32px
- **見出し2**: 20px / 28px
- **見出し3**: 16px / 24px
- **本文**: 14px / 20px
- **キャプション**: 12px / 16px

## 6. アニメーション

### トランジション
```scss
$transition-fast: 150ms ease-in-out;
$transition-base: 250ms ease-in-out;
$transition-slow: 350ms ease-in-out;
```

### ローディング状態
- **スケルトンスクリーン**: コンテンツ読み込み中
- **スピナー**: アクション処理中
- **プログレスバー**: 長時間処理

## 7. アクセシビリティ

### キーボードナビゲーション
- Tab: フォーカス移動
- Enter: アクション実行
- Escape: モーダル・ポップアップを閉じる
- Ctrl/Cmd + Enter: メッセージ送信

### ARIA属性
- 適切なrole属性の設定
- aria-labelの提供
- フォーカス管理

### カラーコントラスト
- WCAG AAA基準準拠
- 最小コントラスト比 7:1

## 8. コンポーネントライブラリ（PrimeNG）活用

### 使用コンポーネント
- **p-button**: ボタン全般
- **p-dialog**: モーダルダイアログ
- **p-tree**: ツリービュー
- **p-tabView**: タブコンテンツ
- **p-virtualScroller**: 大量データの仮想スクロール
- **p-splitButton**: アクション選択
- **p-badge**: ステータス表示
- **p-tooltip**: ツールチップ
- **p-progressBar**: 進捗表示
- **p-skeleton**: ローディング状態

### カスタマイズ
```scss
// PrimeNGテーマのカスタマイズ
:root {
  --primary-color: #667eea;
  --primary-color-text: #ffffff;
  --surface-a: #ffffff;
  --surface-b: #f8f9fa;
  --surface-c: #e9ecef;
  --surface-d: #dee2e6;
}
```

## 9. レスポンシブブレークポイント

```scss
$breakpoints: (
  'xs': 0,
  'sm': 576px,
  'md': 768px,
  'lg': 1024px,
  'xl': 1440px,
  'xxl': 1920px
);
```

## 10. パフォーマンス最適化

### 仮想スクロール
- メッセージリストで実装
- セッションリストで実装

### 遅延ロード
- タブコンテンツの動的ロード
- 画像・添付ファイルの遅延読み込み

### デバウンス
- 検索入力: 300ms
- 自動保存: 1000ms

## 11. エラー状態

### エラー表示
- トースト通知（一時的エラー）
- インラインエラー（フォーム検証）
- エラーページ（重大エラー）

### リトライ機能
- 自動リトライ（ネットワークエラー）
- 手動リトライボタン表示
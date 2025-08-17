# テスト戦略ドキュメント

## 1. 概要

CC Vibe Designのテスト戦略は、Vitestを中心とした高速で効率的なテスト環境を構築し、品質の高いコードベースを維持することを目的とします。

## 2. テストフレームワーク選定

### 2.1 Vitest採用の理由

#### パフォーマンス
- **Viteベース**: 開発サーバーと同じビルドパイプラインを使用
- **高速実行**: ESMネイティブサポートによる高速化
- **並列実行**: デフォルトで並列テスト実行
- **インクリメンタルテスト**: 変更ファイルのみテスト実行

#### 開発体験
- **HMR対応**: テストファイルの変更を即座に反映
- **ウォッチモード**: ファイル変更を監視して自動実行
- **UI モード**: ブラウザベースのインタラクティブなテスト実行環境

#### Angular統合
- **@angular-vitest/builder**: Angular CLI との完全統合
- **設定最小化**: ゼロコンフィグでの動作
- **既存プロジェクト移行**: Karma/Jasmineからの簡単な移行パス

## 3. テスト構成

### 3.1 ユニットテスト

#### コンポーネントテスト
```typescript
import { describe, it, expect, beforeEach } from 'vitest';
import { TestBed } from '@angular/core/testing';
import { ChatComponent } from './chat.component';

describe('ChatComponent', () => {
  let component: ChatComponent;
  
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [ChatComponent]
    }).compileComponents();
    
    const fixture = TestBed.createComponent(ChatComponent);
    component = fixture.componentInstance;
  });
  
  it('should create', () => {
    expect(component).toBeTruthy();
  });
  
  it('should send message on enter key', () => {
    // テスト実装
  });
});
```

#### サービステスト
```typescript
import { describe, it, expect, vi } from 'vitest';
import { TestBed } from '@angular/core/testing';
import { ClaudeCodeService } from './claude-code.service';

describe('ClaudeCodeService', () => {
  let service: ClaudeCodeService;
  
  beforeEach(() => {
    TestBed.configureTestingModule({});
    service = TestBed.inject(ClaudeCodeService);
  });
  
  it('should execute claude code command', async () => {
    const spy = vi.spyOn(service, 'executeCommand');
    await service.generateDesign('requirements');
    expect(spy).toHaveBeenCalled();
  });
});
```

### 3.2 統合テスト

#### API統合テスト
```typescript
import { describe, it, expect } from 'vitest';
import { HttpClientTestingModule } from '@angular/common/http/testing';

describe('API Integration', () => {
  it('should integrate with backend services', async () => {
    // WebSocket接続テスト
    // Express APIテスト
  });
});
```

### 3.3 E2Eテスト（Playwright）

```typescript
import { test, expect } from '@playwright/test';

test('complete chat flow', async ({ page }) => {
  await page.goto('http://localhost:4200');
  
  // 新規セッション作成
  await page.click('[data-testid="new-session"]');
  
  // メッセージ送信
  await page.fill('[data-testid="chat-input"]', 'Create a REST API design');
  await page.press('[data-testid="chat-input"]', 'Enter');
  
  // 応答確認
  await expect(page.locator('[data-testid="assistant-message"]')).toBeVisible();
});
```

## 4. Vitest設定

### 4.1 vitest.config.ts

```typescript
import { defineConfig } from 'vitest/config';
import { angular } from '@angular-vitest/builder';

export default defineConfig({
  plugins: [angular()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['src/test-setup.ts'],
    include: ['**/*.spec.ts'],
    coverage: {
      provider: 'v8',
      reporter: ['text', 'json', 'html'],
      exclude: [
        'node_modules/',
        'dist/',
        '**/*.spec.ts',
        '**/*.config.ts'
      ],
      thresholds: {
        branches: 80,
        functions: 80,
        lines: 80,
        statements: 80
      }
    },
    reporters: ['default', 'html'],
    outputFile: {
      html: './test-results/index.html'
    }
  }
});
```

### 4.2 Angular設定（angular.json）

```json
{
  "projects": {
    "ccvibedesign": {
      "architect": {
        "test": {
          "builder": "@angular-vitest/builder:test",
          "options": {
            "tsConfig": "tsconfig.spec.json",
            "polyfills": ["zone.js", "zone.js/testing"]
          }
        }
      }
    }
  }
}
```

## 5. テストカテゴリー

### 5.1 単体テスト範囲

#### コンポーネント
- 入力/出力のバインディング
- イベントハンドリング
- ライフサイクルフック
- テンプレートロジック

#### サービス
- ビジネスロジック
- データ変換
- エラーハンドリング
- 状態管理

#### パイプ・ディレクティブ
- データ変換ロジック
- DOM操作
- バリデーション

### 5.2 統合テスト範囲

- コンポーネント間の連携
- サービス間の依存関係
- ルーティング
- 状態管理（NgRx）

### 5.3 E2Eテスト範囲

- ユーザーフロー全体
- クリティカルパス
- エラーシナリオ
- レスポンシブ動作

## 6. モック戦略

### 6.1 Claude Code モック

```typescript
import { vi } from 'vitest';

export const mockClaudeCodeService = {
  executeCommand: vi.fn().mockResolvedValue({
    output: 'Mocked response',
    exitCode: 0
  }),
  
  streamResponse: vi.fn().mockImplementation(() => {
    return new Observable(observer => {
      observer.next('Chunk 1');
      observer.next('Chunk 2');
      observer.complete();
    });
  })
};
```

### 6.2 WebSocket モック

```typescript
import { vi } from 'vitest';

export const mockSocketService = {
  connect: vi.fn(),
  disconnect: vi.fn(),
  emit: vi.fn(),
  on: vi.fn((event, callback) => {
    // モックイベント処理
  })
};
```

## 7. テスト実行コマンド

```bash
# ユニットテスト実行
npm run test

# ウォッチモード
npm run test:watch

# カバレッジ付き実行
npm run test:coverage

# UIモード
npm run test:ui

# E2Eテスト
npm run e2e

# 特定ファイルのテスト
npm run test -- chat.component.spec.ts
```

## 8. CI/CD統合

### 8.1 GitHub Actions設定

```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '20'
          
      - run: pnpm install
      - run: pnpm test:coverage
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/coverage-final.json
```

## 9. パフォーマンス目標

### テスト実行時間
- ユニットテスト: < 30秒
- 統合テスト: < 1分
- E2Eテスト: < 5分

### カバレッジ目標
- 全体: 80%以上
- コアロジック: 90%以上
- UIコンポーネント: 70%以上

## 10. ベストプラクティス

### 10.1 テスト記述

1. **AAA パターン**: Arrange, Act, Assert
2. **明確な命名**: テストが何を検証するか明確に
3. **独立性**: 各テストは独立して実行可能
4. **DRY原則**: 共通処理はヘルパー関数化

### 10.2 テストデータ

```typescript
// test-data/factory.ts
export const createMockSession = (overrides = {}) => ({
  id: 'test-session-1',
  title: 'Test Session',
  createdAt: new Date(),
  status: 'active',
  ...overrides
});
```

### 10.3 スナップショットテスト

```typescript
it('should match snapshot', () => {
  const component = render(ChatComponent);
  expect(component).toMatchSnapshot();
});
```

## 11. トラブルシューティング

### 一般的な問題と解決策

| 問題 | 原因 | 解決策 |
|------|------|--------|
| テストが遅い | 不要な実DOM操作 | jsdomの最適化、モック活用 |
| フレーキーテスト | 非同期処理の競合 | waitFor、fakeTimers使用 |
| カバレッジ不足 | エッジケース未検証 | 境界値テスト追加 |

## 12. 移行ガイド

### Karma/JasmineからVitestへ

1. **依存関係の更新**
```bash
# 削除
npm uninstall karma karma-* jasmine-core @types/jasmine

# 追加
npm install -D vitest @vitest/ui @angular-vitest/builder jsdom
```

2. **設定ファイルの更新**
- karma.conf.js → vitest.config.ts
- angular.json のテストビルダー変更

3. **テストコードの修正**
- `describe`、`it`、`expect` はそのまま使用可能
- `beforeEach` → `beforeEach` (import from vitest)
- `spyOn` → `vi.spyOn`

## 13. 今後の拡張

- **ビジュアルリグレッションテスト**: Percy/Chromatic統合
- **パフォーマンステスト**: Lighthouse CI
- **アクセシビリティテスト**: axe-core統合
- **契約テスト**: Pact.js導入検討
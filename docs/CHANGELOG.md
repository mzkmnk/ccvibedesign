# 変更履歴

## 2025-01-17 - ドキュメント更新

### 変更内容

#### Next.js記述の削除とAngular対応
- `docs/architecture/architecture-design.md`: Next.jsをAngularに変更
  - NextApp → AngularApp
  - ポート3000 → 4200（Angular標準）
  - Next.js App Router → Angular application構造
- `docs/PROJECT_OVERVIEW.md`: フレームワーク記述をAngular v20に統一

#### ライブラリバージョンの最新化
- **Angular**: v20 (2025年5月リリース最新版)
  - TypeScript 5.8必須
  - Node.js v20必須
  - Signals API安定版
  - Zoneless Change Detection (Developer Preview)
  - Incremental Hydration安定版

- **PrimeNG**: v18 → v20
  - Angular 20完全対応
  - @primeuix/themes, @primeuix/styles追加

- **NgRx**: v18 → v20
  - SignalStore成熟
  - @ngrx/signals追加
  - Zoneless対応

- **その他の更新**:
  - TypeScript: 5.6 → 5.8
  - Express: 4.18 → 4.21
  - Socket.io: 4.6 → 4.8
  - Commander: 11 → 12
  - Inquirer: 9 → 10
  - LowDB: 6 → 7
  - Zone.js: 0.14 → 0.15
  - Marked: 12 → 14
  - Highlight.js: 11.9 → 11.10
  - Mermaid: 10 → 11
  - Monaco Editor: 0.45 → 0.52
  - jsdom: 24 → 25
  - Playwright: 1.40 → 1.48
  - ESLint: 8 → 9
  - Prettier: 3.0 → 3.3

### 理由
- Angular v20の正式リリース（2025年5月）に対応
- 各ライブラリの最新安定版への更新
- パフォーマンス向上と新機能の活用
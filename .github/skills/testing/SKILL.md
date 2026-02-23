# テスト

## 1. テストコードの構造（AAAパターン）

*   **ルール**: テストコードは「Arrange（準備）」「Act（実行）」「Assert（検証）」の3フェーズに明確に分割し、コメントで区切る。
*   **目的**: 誰が見てもテストの意図を瞬時に理解できるようにするため。

### コード例 (TypeScript / Vitest)

```typescript
import { describe, it, expect } from 'vitest';
import { calculateTotal } from './cart';

describe('ShoppingCart', () => {
  it('should apply discount when total exceeds threshold', () => {
    // Arrange (準備)
    const items = [
      { price: 100, quantity: 2 },
      { price: 50, quantity: 1 }
    ];
    const discountThreshold = 200;

    // Act (実行)
    const total = calculateTotal(items, discountThreshold);

    // Assert (検証)
    expect(total).toBe(225); // 10% discount applied
  });
});
```

## 2. テストデータの生成（Builderパターン）

*   **ルール**: テストデータの生成には、モデルごとの専用Builderクラスを使用する。
*   **ルール**: `default()` メソッドで「バリデーションを通る最小限の有効データ」を生成し、テストケースごとに必要な差分のみをメソッドチェーンで上書きする。
*   **目的**: テストデータの準備を簡略化し、モデルの仕様変更に強いテストにするため。

### コード例 (TypeScript)

```typescript
// UserBuilder.ts
export class UserBuilder {
  private user: User;

  constructor() {
    this.user = this.default();
  }

  // バリデーションを通る最小限の有効データ
  private default(): User {
    return {
      id: 'user_123',
      name: 'Default User',
      email: 'test@example.com',
      role: 'GUEST',
      isActive: true,
    };
  }

  // 必要な差分のみを上書きするメソッド
  withName(name: string): this {
    this.user.name = name;
    return this;
  }

  withRole(role: UserRole): this {
    this.user.role = role;
    return this;
  }

  build(): User {
    return { ...this.user }; // 不変性を保つためにコピーを返す
  }
}

// Test usage
it('should allow ADMIN to delete posts', () => {
  // Arrange
  const adminUser = new UserBuilder()
    .withRole('ADMIN')
    .build();

  // ... Act & Assert
});
```

## 3. テストケースの分類

*   **ルール**: テストスイート（テストのグループ）は、以下の3つの観点で整理・網羅する。
    *   **正常系 (Normal)**: 正しい入力による基本機能の成功パターン。
    *   **準正常系 (Semi-normal)**: 境界値、バリデーションエラー、権限不足などの「仕様として想定されたエラー」。
    *   **異常系 (Abnormal)**: 接続エラー、タイムアウトなどの「システム的な予期せぬエラー」とリカバリ処理。

## 4. カバレッジ（網羅率）戦略

*   **ルール**: カバレッジの数値達成を目的とせず、「重要ロジックの網羅」を最優先とする。
*   **目標基準**:
    *   **全体目標**: C0（命令網羅）80%以上
    *   **重点目標（ビジネスロジック等）**: C1（分岐網羅）90%以上（必須）
    *   **重点目標（共通UI・コンポーネント等）**: C0 100%（目標）
*   **除外対象（テスト不要）**: 自動生成コード、外部ライブラリのラッパー、定数・型定義ファイル、設定ファイル。

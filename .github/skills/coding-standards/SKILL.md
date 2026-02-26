---
name: Coding Standards
description: 変数命名、プロジェクト構成、SOLID原則、DRY/KISS/YAGNIなど、コードの品質と一貫性を保つための規約とベストプラクティス。
---

# Coding Standards

## When to Use
- 全てのコードを書く際
- コードレビューを行う際
- 既存コードのリファクタリングを行う際

## Quick Start
1. コードを書く前に「Guidelines」の「1. プログラミングの基本原則」を確認する。
2. 実装中に変数名や関数名が「意図が伝わる命名」になっているか意識する。
3. コードをコミットする前に、Linter/Formatterを実行して形式を整える。

## Guidelines

### 1. プログラミングの基本原則

#### DRY原則 (Don't Repeat Yourself)
*   **同じコードを重複して書かない。**
*   **解説:** 繰り返し同じ処理を書くと、修正が必要になったときに全ての箇所を直す手間が発生し、バグの温床になります。共通処理は関数やモジュールにまとめましょう。
*   **TypeScript 例:**
    ```typescript
    // Bad
    function getCircleArea(radius: number) {
      return radius * radius * 3.14159;
    }

    function getCircleCircumference(radius: number) {
      return 2 * radius * 3.14159;
    }

    // Good: 定数として定義し、再利用する
    const PI = 3.14159;

    function getCircleArea(radius: number) {
      return radius * radius * PI;
    }

    function getCircleCircumference(radius: number) {
      return 2 * radius * PI;
    }
    ```

#### KISS原則 (Keep It Simple, Stupid)
*   **複雑さを避け、常にシンプルに保つ。**
*   **解説:** 複雑なコードは理解しにくく、メンテナンスも困難です。過剰な設計や最適化を避け、誰が見てもわかるシンプルなコードを目指しましょう。
*   **TypeScript 例:**
    ```typescript
    // Bad: 複雑な条件分岐
    function getDayName(day: number): string {
      if (day === 0) return 'Sunday';
      else if (day === 1) return 'Monday';
      // ... 他の曜日
      else return 'Unknown';
    }

    // Good: 配列を使ってシンプルに
    const DAYS = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

    function getDayName(day: number): string {
      return DAYS[day] || 'Unknown';
    }
    ```

#### YAGNI原則 (You Aren't Gonna Need It)
*   **将来の予測で作らず、今必要な機能だけを作る。**
*   **解説:** 「いつか使うかもしれない」と予測して実装した機能のほとんどは、実際には使われません。現在必要な機能の実装に集中し、無駄なコードを減らしましょう。
*   **TypeScript 例:**
    ```typescript
    // Bad: まだ必要ないメソッドを定義してしまう
    class User {
      constructor(public name: string) {}

      getName() { return this.name; }

      // 今は使わないが、将来使うかもしれない
      deleteAccount() { /* ... */ }
    }

    // Good: 今必要なものだけ実装する
    class User {
      constructor(public name: string) {}

      getName() { return this.name; }
    }
    ```

#### ボーイスカウトルール
*   **自分が触ったコードは、触る前よりも綺麗にして残す。**
*   **解説:** キャンプ場を来た時よりも綺麗にして去るボーイスカウトのように、コードを修正する際は周辺のコードもリファクタリングして少しでも良くしましょう。
*   **TypeScript 例:**
    ```typescript
    // Before: 変数名が不明瞭で、コメントも適切でない
    function c(a: number, b: number) {
      // 足し算
      return a + b;
    }

    // After: リファクタリングを行い、変数名を明確にし、不要なコメントを削除
    function add(firstNumber: number, secondNumber: number): number {
      return firstNumber + secondNumber;
    }
    ```

#### SOLID原則
*   **クラスやモジュールの役割を1つに絞り、変更に強く拡張しやすい設計にする。**
*   **解説:** ソフトウェア設計の5つの原則（単一責任の原則、オープン・クローズドの原則など）の総称です。ここでは特に「単一責任の原則（SRP）」に焦点を当てます。
*   **TypeScript 例 (単一責任の原則):**
    ```typescript
    // Bad: Userクラスがデータの保持と保存処理の両方を行っている
    class User {
      constructor(public name: string) {}

      saveToDatabase() {
        // データベースへの保存処理...
        console.log(`Saving ${this.name} to DB`);
      }
    }

    // Good: 役割を分離する (データの保持と保存処理を分ける)
    class User {
      constructor(public name: string) {}
    }

    class UserRepository {
      save(user: User) {
        // データベースへの保存処理...
        console.log(`Saving ${user.name} to DB`);
      }
    }
    ```

### 2. コーディング規約の基本

#### 意図が伝わる命名
*   **変数や関数は、中身や目的がひと目で分かる名前にする。**
*   **解説:** 名前だけで役割が理解できるようにすることで、コードの可読性が向上し、バグを減らせます。
*   **TypeScript 例:**
    ```typescript
    // Bad: 何を表しているかわからない
    const d = 10;
    function proc(u: any) { /* ... */ }

    // Good: 具体的で意味のある名前
    const daysUntilExpiration = 10;
    function processUserRegistration(user: User) { /* ... */ }
    ```

#### 記法の統一
*   **キャメルケースやスネークケースなど、言語ごとの命名ルールに従う。**
*   **解説:** プロジェクト全体で命名規則を統一することで、コードの一貫性を保ちます。TypeScriptでは一般的にキャメルケース（camelCase）が推奨されます。
*   **TypeScript 例:**
    ```typescript
    // Bad: 命名規則が混在している
    const user_name = 'Alice';
    function Get_User_Info() { /* ... */ }

    // Good: キャメルケースで統一
    const userName = 'Alice';
    function getUserInfo() { /* ... */ }

    // (クラス名やインターフェース名はパスカルケース PascalCase)
    class UserProfile {}
    ```

#### マジックナンバーの排除
*   **コード中の直接的な数値（意味不明な数字）は避け、名前を付けて定数化する。**
*   **解説:** 直接数値を書くと、その数値の意味がわからず、変更時に修正漏れが発生しやすくなります。
*   **TypeScript 例:**
    ```typescript
    // Bad: 1が何を意味するのか不明
    if (user.status === 1) {
      // ...
    }

    // Good: 定数化して意味を持たせる
    const STATUS_ACTIVE = 1;

    if (user.status === STATUS_ACTIVE) {
      // ...
    }
    ```

#### 早期リターン (Early Return)
*   **無駄な条件分岐やネスト（インデント）を深くしない。**
*   **解説:** 条件を満たさない場合にすぐに関数から抜けることで、ネストを浅くし、主要な処理の流れを追いやすくします。
*   **TypeScript 例:**
    ```typescript
    // Bad: ネストが深い
    function updateUser(user: User | null) {
      if (user !== null) {
        if (user.isActive) {
          // 更新処理...
        }
      }
    }

    // Good: 早期リターンでネストを解消
    function updateUser(user: User | null) {
      if (!user) return;
      if (!user.isActive) return;

      // 更新処理...
    }
    ```

#### 「Why」のコメント
*   **「何をしているか」ではなく、背景や理由となる「なぜそう書いたか」をコメントに残す。**
*   **解説:** コード自体の動作はコードを読めばわかりますが、その実装に至った経緯や理由はコードだけでは伝わりにくいものです。
*   **TypeScript 例:**
    ```typescript
    // Bad: コードを見ればわかることを書いている
    // iを1増やす
    i++;

    // Good: なぜそうする必要があるのかを書いている
    // 次の処理でインデックスが1始まりである必要があるため調整
    i++;
    ```

#### フォーマットの自動化
*   **インデントや改行などの見た目は、手動ではなくツール（LinterやFormatter）で自動整形する。**
*   **解説:** 人手によるフォーマット修正は時間の無駄であり、ミスも起こりやすいです。Prettierなどのツールを使って自動化しましょう。
*   **TypeScript 例:**
    *   **Prettier** などを導入し、保存時に自動整形されるように設定する。
    *   **ESLint** を導入し、コードの品質チェックを自動化する。
    *   (コード例というよりは設定や運用の話になります)

## Templates
- Path: `.github/skills/coding-standards/templates/`

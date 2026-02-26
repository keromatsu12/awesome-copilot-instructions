# コーディング規約

## 1. プログラミングの基本原則

### DRY原則 (Don't Repeat Yourself)
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

### KISS原則 (Keep It Simple, Stupid)
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

### YAGNI原則 (You Aren't Gonna Need It)
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

### ボーイスカウトルール
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

### SOLID原則
*   **クラスやモジュールの役割を1つに絞り、変更に強く拡張しやすい設計にする。**
*   **解説:** ソフトウェア設計をより理解しやすく、柔軟で、メンテナンスしやすいものにするための5つの原則の総称です。

#### 1. 単一責任の原則 (Single Responsibility Principle - SRP)
*   **クラスやモジュールを変更する理由は1つだけでなければならない。**
*   **解説:** 1つのクラスがあまりに多くのことを行っていると、ある機能の変更が他の無関係な機能に影響を与えてしまう可能性があります。役割ごとにクラスを分割することで、影響範囲を限定できます。
*   **TypeScript 例:**
    ```typescript
    // Bad: Userクラスが「データ保持」と「保存処理」という2つの責任を持っている
    class User {
      constructor(public name: string) {}

      saveToDatabase() {
        // データベースへの保存処理...
        console.log(`Saving ${this.name} to DB`);
      }
    }

    // Good: 責任を分割する
    // データ保持の責任
    class User {
      constructor(public name: string) {}
    }

    // 保存処理の責任
    class UserRepository {
      save(user: User) {
        console.log(`Saving ${user.name} to DB`);
      }
    }
    ```
*   **改善点:** `User`クラスはデータの定義だけに関心を持ち、保存方法の変更（DBからファイルへなど）の影響を受けなくなりました。

#### 2. オープン・クローズドの原則 (Open/Closed Principle - OCP)
*   **ソフトウェアの構成要素（クラス、モジュール、関数など）は、拡張に対しては開いていて、修正に対しては閉じていなければならない。**
*   **解説:** 新しい機能を追加するときに、既存のコードを修正せずに済むように設計すべきです。インターフェースや継承をうまく使うことで実現します。
*   **TypeScript 例:**
    ```typescript
    // Bad: 新しい図形が増えるたびに、AreaCalculatorクラスを修正する必要がある
    class Rectangle {
      constructor(public width: number, public height: number) {}
    }

    class Circle {
      constructor(public radius: number) {}
    }

    class AreaCalculator {
      calculate(shape: any) {
        if (shape instanceof Rectangle) {
          return shape.width * shape.height;
        } else if (shape instanceof Circle) {
          return shape.radius * shape.radius * Math.PI;
        }
      }
    }

    // Good: インターフェースを使って、既存コードを修正せずに拡張可能にする
    interface Shape {
      area(): number;
    }

    class Rectangle implements Shape {
      constructor(public width: number, public height: number) {}
      area() { return this.width * this.height; }
    }

    class Circle implements Shape {
      constructor(public radius: number) {}
      area() { return this.radius * this.radius * Math.PI; }
    }

    class AreaCalculator {
      calculate(shape: Shape) {
        return shape.area();
      }
    }
    ```
*   **改善点:** 新しい図形（例えばTriangle）を追加したい場合、`AreaCalculator`を一切変更せずに、新しいクラスを作るだけで済みます。

#### 3. リスコフの置換原則 (Liskov Substitution Principle - LSP)
*   **派生型（サブクラス）はその基本型（スーパークラス）と置換可能でなければならない。**
*   **解説:** サブクラスは、スーパークラスの振る舞いを壊してはいけません。親クラスとして扱っても正しく動作する必要があります。
*   **TypeScript 例:**
    ```typescript
    // Bad: サブクラスで親クラスの前提を壊している
    class Bird {
      fly() { console.log('Flying'); }
    }

    class Penguin extends Bird {
      fly() {
        throw new Error('Cannot fly'); // ペンギンは飛べない
      }
    }

    function makeBirdFly(bird: Bird) {
      bird.fly(); // ペンギンが渡されるとエラーになり、プログラムが壊れる
    }

    // Good: 共通の振る舞いを正しく抽出する
    class Bird {
      move() { console.log('Moving'); }
    }

    class FlyingBird extends Bird {
      fly() { console.log('Flying'); }
    }

    class Penguin extends Bird {
      swim() { console.log('Swimming'); }
    }

    function makeBirdFly(bird: FlyingBird) {
      bird.fly(); // 飛べる鳥だけを受け取る
    }
    ```
*   **改善点:** `Penguin`を無理やり`Bird`（飛べるものとしての前提）のサブクラスにするのではなく、適切な継承関係やインターフェースに見直すことで、予期せぬエラーを防ぎます。

#### 4. インターフェース分離の原則 (Interface Segregation Principle - ISP)
*   **クライアントが利用しないメソッドへの依存を強制してはならない。**
*   **解説:** 巨大なインターフェースを作るよりも、目的別の小さなインターフェースを複数作った方が良いです。不要な実装を強制されるのを防ぎます。
*   **TypeScript 例:**
    ```typescript
    // Bad: 必要のないメソッドまで実装させられる
    interface Worker {
      work(): void;
      eat(): void;
    }

    class Human implements Worker {
      work() { console.log('Working'); }
      eat() { console.log('Eating lunch'); }
    }

    class Robot implements Worker {
      work() { console.log('Working'); }
      eat() {
        throw new Error('Cannot eat'); // ロボットは食べないが実装を強制される
      }
    }

    // Good: インターフェースを分離する
    interface Workable {
      work(): void;
    }

    interface Eatable {
      eat(): void;
    }

    class Human implements Workable, Eatable {
      work() { console.log('Working'); }
      eat() { console.log('Eating lunch'); }
    }

    class Robot implements Workable {
      work() { console.log('Working'); }
    }
    ```
*   **改善点:** `Robot`クラスは`eat`メソッドを実装する必要がなくなり、自分に関係のある機能だけに集中できます。

#### 5. 依存性逆転の原則 (Dependency Inversion Principle - DIP)
*   **上位モジュールは下位モジュールに依存してはならない。両者は抽象に依存すべきである。**
*   **解説:** 具体的なクラスに依存するのではなく、インターフェースや抽象クラスに依存させることで、結合度を下げます。これにより、実装の差し替えが容易になります。
*   **TypeScript 例:**
    ```typescript
    // Bad: 上位モジュール(UserService)が下位モジュール(MySQLDatabase)に直接依存している
    class MySQLDatabase {
      save(data: string) { console.log('Saved to MySQL'); }
    }

    class UserService {
      private db = new MySQLDatabase(); // 直接依存

      saveUser(name: string) {
        this.db.save(name);
      }
    }

    // Good: 抽象（インターフェース）に依存させる
    interface Database {
      save(data: string): void;
    }

    class MySQLDatabase implements Database {
      save(data: string) { console.log('Saved to MySQL'); }
    }

    class PostgreSQLDatabase implements Database {
      save(data: string) { console.log('Saved to PostgreSQL'); }
    }

    class UserService {
      constructor(private db: Database) {} // 抽象に依存

      saveUser(name: string) {
        this.db.save(name);
      }
    }
    ```
*   **改善点:** `UserService`は具体的なデータベースの実装を知る必要がなくなり、`MySQL`から`PostgreSQL`への切り替えなども`UserService`のコードを変更せずに行えます。

## 2. コーディング規約の基本

### 意図が伝わる命名
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

### 記法の統一
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

### マジックナンバーの排除
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

### 早期リターン (Early Return)
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

### 「Why」のコメント
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

### フォーマットの自動化
*   **インデントや改行などの見た目は、手動ではなくツール（LinterやFormatter）で自動整形する。**
*   **解説:** 人手によるフォーマット修正は時間の無駄であり、ミスも起こりやすいです。Prettierなどのツールを使って自動化しましょう。
*   **TypeScript 例:**
    *   **Prettier** などを導入し、保存時に自動整形されるように設定する。
    *   **ESLint** を導入し、コードの品質チェックを自動化する。
    *   (コード例というよりは設定や運用の話になります)

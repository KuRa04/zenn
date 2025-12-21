---
title: "責務毎にコードを整理するための考え方"
emoji: "🔧"
type: "tech"
topics: ["設計"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準と設計について学びを深めていきます。
第21回は責務毎にコードを整理するための考え方がテーマです。

## 設計の基礎を読んだけど、実務で迷いそう
『良いコード/悪いコードで学ぶ設計入門』2章にある4つの基本的な考え方
1. 省略せずに意図が伝わる名前を設計する
2. 変数を使い回さない、目的ごとの変数を用意する
3. ベタ書きせず、意味のあるまとまりでメソッド化
4. 関係し合うデータとロジックをクラスにまとめる

1は第20回の[良い命名・悪い命名について](https://zenn.dev/kura_04/articles/advent-calendar-2025-12-20)で触れました。今回は2~4に着目します。
これらの考え方をわかった気がしましたが、実装で判断に迷うことがありました。
迷った例とその解決策について説明していきます。

## 迷いと解決策

### 迷い①: どこまで変数をまとめるべき？

**迷いの具体例**
- 処理の中間結果を毎回変数に入れるべきか
- get → filter → map の各ステップで変数を分けるべきか

**過剰に分けると追いづらくなる**

```typescript
// ❌ 各ステップで変数を作りすぎ
const allUsers = await fetchUsers();
const activeUsers = allUsers.filter(u => u.isActive);
const activeUserIds = activeUsers.map(u => u.id);
const result = await fetchDetailsByIds(activeUserIds);
```

- 変数が4つあるが、`allUsers` や `activeUsers` を後で使うわけではない
- 途中経過を全部追わないとコードの意図が見えない

**意味のある塊でまとめる**

```typescript
// ✅ 中間結果をまとめて、意図を明確に
const activeUserIds = (await fetchUsers())
  .filter(u => u.isActive)
  .map(u => u.id);

const result = await fetchDetailsByIds(activeUserIds);
```

- `activeUserIds` という名前で「何を取り出したか」が明確
- 後続の処理に必要な変数だけが残る

**判断基準: その変数、後で使う？名前に意味がある？**

| 問い | 分けるべきサイン |
|------|------------------|
| この変数、後で参照する？ | No なら変数にしなくていい |
| この変数名、意図を伝えてる？ | No ならチェーンでまとめた方が読みやすい |
| デバッグ時に中間状態を見たい？ | Yes なら分ける価値あり |

**実践の問いかけ**
- 「この変数、後で使う？それとも次の行だけ？」
- 「変数名を見て、何が入っているか伝わる？」
- 「チェーンでつなげた方が処理の流れが見える？」

---

### 迷い②: どこまでメソッドを分けるべき？

**迷いの具体例**
- データ取得 → フィルタ → 変換、全部分けるべき？
- 1箇所でしか使わない処理も分けるべき？

**判断基準: 変更理由は何種類あるか**

```typescript
// 変更理由が複数ある例
function generateUserReport(user: User) {
  // 1. データの取得ロジック ← インフラ担当が変更を求める
  const activities = fetchUserActivities(user.id);
  
  // 2. ビジネスルール ← 人事部門が変更を求める
  const totalHours = activities.reduce((sum, a) => sum + a.hours, 0);
  const isOverworked = totalHours > 160;
  
  // 3. 表示フォーマット ← デザイナーが変更を求める
  return `<h1>${user.name}さんのレポート</h1>...`;
}
```

| 変更を求める人 | 変更理由の例 |
|---------------|-------------|
| インフラ/DB担当 | 「データ取得先をAPIに変えたい」 |
| 人事部門 | 「180時間を超えたら警告にして」 |
| フロントエンド| 「HTMLじゃなくてJSON返して」 |

**分けるとこうなる**

```typescript
// それぞれの関心事で分離
function fetchUserActivities(userId: string): Activity[] { ... }
function calculateWorkSummary(activities: Activity[]): WorkSummary { ... }
function formatReportAsHtml(user: User, summary: WorkSummary): string { ... }
```

**実践の問いかけ**
- 「もしDBスキーマが変わったら、このコードも変わる？」
- 「もしビジネスルールが変わったら？UIデザインが変わったら？」
- 複数に「はい」なら、変更理由が複数ある

---

### 迷い③: この関数はどこに置くべき？

**迷いの具体例**
- 新しい関数を追加したいけど、このI/Fでいいのか？
- 既存のクラスに追加すべきか、新しく作るべきか？

**迷いの原因: そのクラス・I/Fの責務を理解していない**

**責務を理解するアプローチ**

1. 名前から推測する
```typescript
interface UserRepository { }     // → ユーザーの永続化
interface AuthService { }        // → 認証
interface NotificationSender { } // → 通知を送る
```

2. 既存メソッドを観察する
```typescript
interface UserRepository {
  findById(id: string): User | null;
  findByEmail(email: string): User | null;
  save(user: User): void;
}
// ここに sendWelcomeEmail() は明らかに浮く
```

3. 追加したい関数の「主語」を考える
```typescript
// UserRepository に追加すべき？
validatePassword(password: string)  // 主語は「パスワード」→ 違和感
save(user: User)                    // 主語は「ユーザー」→ OK
```

**実践の問いかけ**
- 「`InterfaceX.doSomething()` と書いて自然に読める？」
- 「既存メソッドと同じ種類の操作？」
- 「このクラスの責務を一文で言える？」

## 根本解決: 視座を上げる

3つの迷いに共通するのは**狭い視野でコードを見ている**こと。視座を上げると判断しやすくなる。

**視点①: プロダクトのアクターから考える**

B2B SaaS なら、こんな登場人物がいるはず：

| アクター | やりたいこと |
|---------|-------------|
| システム管理者 | ユーザー・組織・権限を管理したい |
| アプリ作成者 | 業務に合ったアプリを作りたい |
| 一般ユーザー | アプリでデータを登録・閲覧したい |

この視点があると、コードを見たとき「これは誰のための機能だ」と位置づけられる。

**視点②: コードのアクター（呼び出し側）から考える**

```typescript
// Controller（アクター）から見た Service
class AppReviewController {
  async review(appConfig: AppConfig) {
    // Controller は「設定を渡したら結果が欲しい」だけ
    const result = await this.reviewService.review(appConfig);
    return result;
  }
}
```

「このクラスを呼び出すのは誰？何を期待してる？」と考えると、I/Fの設計が自然に決まる。

**2つの視点を行き来する**

| 視点 | 問い | 設計への影響 |
|------|------|-------------|
| ビジネスのアクター | この機能は誰のため？ | モジュール分割、ユースケースの境界 |
| コードのアクター | この関数は誰が呼ぶ？ | I/F設計、依存の方向 |

---

## まとめ
今回は命名について紹介しました。
適切に責務を分けてコードを書くために下記の内容を意識しようと思います。

**3つの迷いに対する問いかけ**
- 変数: 「一緒に変わる？別々に変わる？」
- メソッド: 「変更理由は何種類？」
- 配置する場所: 「このクラスの責務は何？」

**根本的には視座を上げることで迷いが減る**
- プロダクトのアクターは誰か
- このコードを呼び出すのは誰か

**下から積み上げるだけでなく、上から見下ろす視点を持つ**






## 参考
[良いコード/悪いコードで学ぶ設計入門 ―保守しやすい 成長し続けるコードの書き方](https://amzn.asia/d/94DSwFg)
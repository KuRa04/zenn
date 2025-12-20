---
title: "【設計】良い命名・悪い命名について"
emoji: "🔧"
type: "tech"
topics: ["設計"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準と設計について学びを深めていきます。
第20回は良い・悪い命名がテーマです。
カンファレンスでDDDについて聞き、興味が湧いた設計について学習をし始めたのですが、
DDD以前に設計の基礎をしっかりと固める必要があると感じました。
まずは、多くの書籍の冒頭で語られていることが多い命名をまとめていこうと思います。

## 命名とは
良い・悪い命名とは何かを考える前に、命名とは何かを整理したいと思います。
プログラムの中では、計算結果を格納する変数名、意味のある処理をまとめたメソッド名、関連するデータとロジックをまとめたクラス名などを定義します。
処理の結果やまとまりを示すため、意味のある分かりやすい名前にする必要があります。
意味のある名前、わかりやすい名前とはどのようなものでしょうか。
これらを具体例を用いてこれから説明していきます。

## 悪い命名パターンと改善例
下記の変数はレストランの注文前のオーダーを格納する配列です。

### 1. 技術駆動命名
「Memory」「Int」「Array」などプログラムの用語にある言葉で命名すること。
ただ、ネットワークやメモリ管理であれば、「packet」「Memory」という言葉は適しているとも考えている。そのため、今はレイヤーに合わせた命名をするべきという理解をしている。
```typescript
// ❌️ 型名を表すArrayが記述されており、格納される情報が具体的に説明されていない。
const orderArray: Order[] = [];

// ✅️ pendingとあり、注文前のオーダーだということが変数名から推測出来る。
const pendingOrders: Order[] = [];
```

### 2. 連番命名
変数を連番で定義すること。
```typescript
// ❌️ 連番になっているため、同じ処理だと推測しそうになるが、格納されている処理が全く異なる。
const order1 = createOrder(tableId);
const order2 = addItems(order1, items);
const order3 = applyDiscount(order2, coupon);

// ✅️ 格納されている処理に合わせて変数名が定義されているため、変数名から処理を推測しやすい。
const emptyOrder = createOrder(tableId);
const orderWithItems = addItems(emptyOrder, items);
const discountedOrder = applyDiscount(orderWithItems, coupon);
```

### 3. 名前と処理が異なる命名
定義した名前と実際の処理の内容が異なること。
```typescript
// ❌️ updateなのに存在しなければ作成する
class TableRepository {
  updateTable(tableId: string, status: TableStatus): void {
    const table = this.database.find(tableId);
    if (table) {
      table.status = status;
      this.database.update(table);
    } else {
      this.database.insert({ id: tableId, status });  // 作成してる
    }
  }
}

// ✅️ upsertと明示する
class TableRepository {
  upsertTable(tableId: string, status: TableStatus): void {
    const table = this.database.find(tableId);
    if (table) {
      table.status = status;
      this.database.update(table);
    } else {
      this.database.insert({ id: tableId, status });
    }
  }
}
```

### 4. 複数の責務をまとめている命名
複数の責務を持った処理をまとめて定義すること。
処理の内容を推測しづらいことにつながる。実装の仕方に問題があること可能性もある。
```typescript
// ❌️ 命名から在庫確認・金額計算・注文確定・通知・印刷などを推測することが不可能
class Order {
  processOrder(): void {
    // 在庫確認
    for (const item of this.items) {
      const stock = this.inventoryService.check(item.menuId);
      if (stock < item.quantity) {
        this.items = this.items.filter(i => i.menuId !== item.menuId);
        this.notificationService.notifyOutOfStock(item.menuId);
      }
    }
    
    // 金額計算
    this.subtotal = this.items.reduce((sum, item) => sum + item.price * item.quantity, 0);
    this.tax = this.subtotal * 0.1;
    if (this.items.length >= 5) {
      this.discount = this.subtotal * 0.1;
    }
    this.total = this.subtotal + this.tax - this.discount;
    
    // 注文確定
    this.status = "confirmed";
    this.confirmedAt = new Date();
    
    // キッチンに通知
    this.kitchenService.notify(this);
    
    // 印刷
    this.printService.printReceipt(this);
  }
}


// ✅️  在庫確認や金額計算を書くメソッドに分けており、命名から処理を推測しやすい
class Order {
  removeUnavailableItems(inventoryService: InventoryService): RemovedItem[] {
    const removed: RemovedItem[] = [];
    this.items = this.items.filter(item => {
      const available = inventoryService.check(item.menuId) >= item.quantity;
      if (!available) removed.push(item);
      return available;
    });
    return removed;
  }

  calculateTotal(): void {
    this.subtotal = this.items.reduce((sum, item) => sum + item.price * item.quantity, 0);
    this.tax = this.subtotal * 0.1;
    this.discount = this.items.length >= 5 ? this.subtotal * 0.1 : 0;
    this.total = this.subtotal + this.tax - this.discount;
  }

  confirm(): void {
    this.status = "confirmed";
    this.confirmedAt = new Date();
  }
}

// 呼び出し側で組み合わせる
class OrderService {
  submitOrder(order: Order): void {
    const removed = order.removeUnavailableItems(this.inventoryService);
    if (removed.length > 0) {
      this.notificationService.notifyOutOfStock(removed);
    }
    
    order.calculateTotal();
    order.confirm();
    
    this.kitchenService.notify(order);
    this.printService.printReceipt(order);
  }
}
```

## 良い命名をするために考えること
「命名とは」の部分で挙げた「意味のあるわかりやすい名前」とは格納されている処理が命名に含まれているかつ、そこから処理を推測可能であることです。
紹介した悪い命名パターンは名前から処理を推測することが不可能だと思います。
あるメソッドに対して在庫処理と金額計算など複数の処理を含んでいた場合、4番にあったように命名ではなく処理を疑ったほうが良いでしょう。
どのような責務があるのかに注目して考えると良い命名が出来そうです。
これが設計ですね。（とても難しいのですが、、、）

## まとめ
今回は命名について紹介しました。
プログラムを書く上で非常に大事で、設計の基礎となる内容だと思います。


## 参考
[良いコード/悪いコードで学ぶ設計入門 ―保守しやすい 成長し続けるコードの書き方](https://amzn.asia/d/94DSwFg)
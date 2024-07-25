---
title: "[Jest]単体テストの実装例"
emoji: "🐙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Jest"]
published: true
---

# はじめに
Jestの単体テストの学びを整理した記事です。
いくつかの記事に分けて投稿予定します。



# この記事のゴール
単体テストの基本的なテストメソッドを理解できる。


# 単体テストの実装例
**index.ts**
```

export const authUser = (userName: string) => {
  if (userName === "admin") {
    return true;
  }
  return false;
} 
```

**index.test.ts**
```
import { authUser } from ".";

describe("ユーザー認証", () => {
  const store = {
    users: [""],
  };

  beforeEach(() => {
    console.log("beforeEach");
    store.users.push("admin");
    console.log(store.users);
  });

  afterEach(() => {
    console.log("afterEach");
    store.users.pop();
    console.log(store.users);
  });

  describe("ユーザー登録", () => {
    it("正しくユーザーが登録される", () => {
      expect(authUser(store.users[1])).toBeTruthy();
    });

    it("ユーザーが登録されていない", () => {
      expect(authUser(store.users[0])).toBeFalsy();
    });
  });
});
```

# 上記のテストに利用しているメソッド一覧
- describe
- it
- expect
- toBeTruthy
- toBeFalsy
- beforeEach
- afterEach

## describe(name, fn)
describe(name, fn) は、いくつかの関連するテストをまとめたブロックを作成する。
```
const myBeverage = {
  delicious: true,
  sour: false,
};

describe('my beverage', () => {
  test('is delicious', () => {
    expect(myBeverage.delicious).toBeTruthy();
  });

  test('is not sour', () => {
    expect(myBeverage.sour).toBeFalsy();
  });
});
```

## it
第1引数にテスト名を、第2引数にテストの確認項目を含む関数を設定する。
 3番目の引数 (任意) は タイムアウト値 (ミリ秒単位) で、中止するまでの待ち時間を指定する。

```
  test('has lemon in it', () => {
    return fetchBeverageList().then(list => {
      expect(list).toContain('lemon');
    });
  });
```

## expect
テストを作成する時に、利用する条件文。
expectによって様々な事柄を検証することができる。

## toBeTruthy
期待値が真であるかテストをする。

```
drinkSomeLaCroix();
if (thirstInfo()) {
  drinkMoreLaCroix();
}
```

```
it('drinking La Croix leads to having thirst info', () => {
  drinkSomeLaCroix();
  expect(thirstInfo()).toBeTruthy();
});
```

## toBeFalsy
期待値が偽であるかテストをする。

```
drinkSomeLaCroix();
if (!getErrors()) {
  drinkMoreLaCroix();
}
```

```
it('drinking La Croix does not lead to errors', () => {
  drinkSomeLaCroix();
  expect(getErrors()).toBeFalsy();
});
```


## beforeEach
このファイル内の各テストが完了する前に、関数を実行する。
また、timeout (ミリ秒) を指定して、中断前にどのくらい待機するかを指定することができる。

```
const globalDatabase = makeGlobalDatabase();

beforeEach(() => {
  // データベースをクリアし、テストデータを追加します。
  // Jest will wait for this promise to resolve before running tests.
  return globalDatabase.clear().then(() => {
    return globalDatabase.insert({testData: 'foo'});
  });
});

test('can find things', () => {
  return globalDatabase.find('thing', {}, results => {
    expect(results.length).toBeGreaterThan(0);
  });
});

test('can insert a thing', () => {
  return globalDatabase.insert('thing', makeThing(), response => {
    expect(response.success).toBeTruthy();
  });
});
```


## afterEach
このファイル内の各テストが完了するたびに、関数を実行する。
また、timeout (ミリ秒) を指定して、中断前にどのくらい待機するかを指定することができる。
```
const globalDatabase = makeGlobalDatabase();

function cleanUpDatabase(db) {
  db.cleanUp();
}

afterEach(() => {
  cleanUpDatabase(globalDatabase);
});

it('can find things', () => {
  return globalDatabase.find('thing', {}, results => {
    expect(results.length).toBeGreaterThan(0);
  });
});

it('can insert a thing', () => {
  return globalDatabase.insert('thing', makeThing(), response => {
    expect(response.success).toBeTruthy();
  });
});
```

# 参考記事
[Jestの公式APIドキュメント](https://jestjs.io/ja/docs/api)
[フロントエンド開発のためのテスト入門 今からでも知っておきたい自動テスト戦略の必須知識](https://amzn.asia/d/0a8Kn9N2)


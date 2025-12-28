---
title: "JavaのNestedClassについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "Java"]
published: true
---
## この記事の概要
- NestedClassの書き方や使うメリットを紹介するよ
- 関連するクラスを1箇所にまとめたり、カプセル化で実装を隠蔽したり出来るよ

## NestedClassの利点
### 一箇所でしか使用されないクラスを論理的にグループ化する
`OuterClass`の内部に更にデータ構造がある場合、`OuterClass`の中に作る方がデータをまとめることが出来るため合理的です。
また、内部の`InnerClass`を`static`で定義することで、`OuterClass`のインスタンスを作成せずに`InnerClass`だけをインスタンス化することが可能になります。
下記の例では`record型`を使うとより簡潔に記述可能です。
```java
public class OuterClass {
    private final String outerField;

    public OuterClass(String outerField) {
        this.outerField = outerField;
    }

    String getOuterField() {
        return outerField;
    }

    
    static class InnerClass {
        private final String innerField;

        public InnerClass(String innerField) {
            this.innerField = innerField;
        }

        String getInnerField() {
            return innerField;
        }
    }

    // recordクラスでも表現可能。 
    // record InnerClass(String innerField) {
    // }

    public static void main(String[] args) {
        OuterClass outer = new OuterClass("Outer Value");
        InnerClass inner = new InnerClass("Inner Value");

        System.out.println("Outer Field: " + outer.getOuterField());
        System.out.println("Inner Field: " + inner.getInnerField());

    }
}
```

```java
public class OuterClass {
    private final String outerField;

    public OuterClass(String outerField) {
        this.outerField = outerField;
    }

    String getOuterField() {
        return outerField;
    }

    record InnerClass(String innerField) {
    }

    public static void main(String[] args) {
        OuterClass outer = new OuterClass("Outer Value");
        InnerClass inner = new InnerClass("Inner Value");

        System.out.println("Outer Field: " + outer.getOuterField());
        System.out.println("Inner Field: " + inner.getInnerField());

    }
}
```

### カプセル化を強化する
下記の`クラスB`は`EncapsulatedA`クラスの`privateメンバ`にアクセス出来ます。
通常、別のクラスから`privateフィールド`にアクセスできませんが、ネストしたクラスは外部クラスの`privateメンバ`にアクセスできます。
また、`クラスB`を`private`で宣言しているため、外部からは`クラスB`の存在自体が見えません。外部には`getSecretViaB()`というメソッドだけを公開し、内部で`クラスB`を使っていることは隠蔽されています。

**観点のまとめ**
| 観点 | 説明 |
|------|------|
| 実装の隠蔽 | 内部でどのように処理しているか外部に知らせない |
| 変更への耐性 | クラスBの実装を変更しても外部に影響しない |
| 責務の分離 | 特定の処理を専用クラスに委譲しつつ、外部APIはシンプルに保つ |

```java
public class EncapsulatedA {
    private int secretValue;

    public EncapsulatedA(int value) {
        this.secretValue = value;
    }

    // BはAのprivateメンバにアクセス可能
    private static class B {
        int revealSecret(EncapsulatedA a) {
            return a.secretValue;
        }
    }

    // 外部にBを公開せず、Aのメソッド経由でBの機能を提供
    public int getSecretViaB() {
        B b = new B();
        return b.revealSecret(this);
    }

    public static void main(String[] args) {
        EncapsulatedA a = new EncapsulatedA(42);
        System.out.println("Secret Value via B: " + a.getSecretViaB());
    }
}
```

## まとめ
- NestedClassの書き方や使うメリットを紹介したよ
- 関連するクラスを1箇所にまとめたり、カプセル化で実装を隠蔽したり出来るよ
- privateで隠しているクラス、隠しているクラスにアクセスするメソッドを実装したクラス、そのクラスを作成してprivateフィールドにアクセスなど責務を分けて実装できるよ

## 参考
https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html
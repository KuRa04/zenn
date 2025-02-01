---
title: "AssertJを利用したExceptionの確認方法"
type: "tech"
topics: ["SpringBoot", "AssertJ"]
published: true
---

### やったこと
対象のメソッドで発生する例外を期待値としてテスト。
assertThatThrownBy を利用。

```Java
// Hogeクラス
public class Hoge{
    String name;
    public Hoge(String name){
        if (name.length() < 2){
            throw new RuntimeException("名前を3文字以上にしてください。");
        }
    }
}

//具体的なテストコード RuntimeExceptionのみを観測
assertThatThrownBy(
    () -> new Book("1")
).isInstanceOf(
    RuntimeException.class
);
```

### 注意
Exception が try-catch 等で処理されていると、そのメソッド外に例外が吐き出されることはない。
そのため、try-catch で処理された後に呼び出されているメソッドで assertThatThronBy をしても対象のエラーをキャッチすることはできない。

# 参考記事
[【Java】よく使う assertThat のメソッド集【AssertJ】"](https://nainaistar.hatenablog.com/entry/2020/10/19/%E3%80%90Java%E3%80%91%E3%82%88%E3%81%8F%E4%BD%BF%E3%81%86assertThat%E3%81%AE%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E9%9B%86%E3%80%90AssertJ%E3%80%91)

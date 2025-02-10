---
title: "テストにMockを使うべきタイミングはいつなのか"
type: "tech"
topics: ["SpringBoot", "Mock"]
published: true
---

### 結論
基本的には Mock を使用しない方が良い。テストの保守性や信頼性が低くなるため。
Mock 対象が static メソッドの場合、Mock の処理がより複雑になるため、出来れば本物のデータで実装するべき。

```Java
    try(MockedStatic<StaticSample> mocked = mockStatic(StaticSample.class)) {
        mocked.when(() -> StaticSample.isEqual(anyString(), anyString()))
                .thenReturn(true);

        String actual = target.function(argValue);
        assertThat(actual).isEqualTo(expected);
    }
```

Mock を利用する場面は通常操作では起こらない状況の時
- テスト用データの用意が複雑
- 例外の発生
- 超時間のブロック

# 参考記事
[モッククラスを使うべきか否か"](https://irof.hateblo.jp/entry/2019/07/17/233048)
[モック入門『考え方と使い分けについて』"](https://yoshitaro-yoyo.hatenablog.com/entry/introduction-to-mock)

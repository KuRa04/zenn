---
title: "History APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第5回は「`History API`」をテーマとして記事を執筆しました。
この回では`Navigation API`を取り上げようと思っていたのですが、
調査を始めたところ`History API`の後継として`Navigation API`があることを知りました。
先に`History API`をまとめるのが良いと思い、`History API`の記事を執筆しました。
この回では`History API`とは何か、実際どのように実装するのかを学んでいきます。

## History APIとは
`History API`はブラウザのセッション履歴へのアクセスを提供しています。
遷移の履歴から前後のページへ移動したり、履歴自体の操作をしたりする時に活用できます。

**前のページに戻る**
```js
history.back();
```

**次のページに進む**
```js
history.forward();
```

**履歴内の特定のページへ移動**
```js
//1つ前の前のページに戻る。back()と同様の動作。
history.go(-1);

//1つ次のページに進む。forward()と同様の動作。
history.go(1);
```

**ブラウザのセッション履歴に項目を追加**
```js
// state: pushState()によって作られる新しい履歴項目のオブジェクト。
// unused: この引数は歴史的な理由のために存在していて、省略不可。空文字を渡しておくと安全。
// url: stateを保存するURL。この引数を指定しない場合、現在のURLが設定される。
pushState(state, unused);
pushState(state, unused, url);
```

**現在の履歴を編集し、メソッドに引数で渡された状態オブジェクトやURLで置き換える**
```js
// state: 編集する履歴項目のオブジェクト。登録されていないオブジェクトのキーであれば、値は新規作成され、すでに存在するものであれば更新される。
// unused: この引数は歴史的な理由のために存在していて、省略不可。空文字を渡しておくと安全。
// url: 編集を行う対象のURL。
replaceState(state, unused);
replaceState(state, unused, url);
```

## History APIのpushState, replaceStateの実装例
`history.back()`, `history.forward()`, `history.go()`は想像がつきやすいと思ったので、
ここでは`pushState()`と`replaceState()`の実装例をまとめています。

**動作の流れ**
1. pushStateボタンを押下
2. DevToolsのConsoleでhistory.stateを確認
3. replaceStateボタンを押下
4. DevToolsのConsoleでhistory.stateを確認
```html
<html>
  <body>
    <button id="pushBtn">pushState</button>
    <button id="replaceBtn">replaceState</button>
  </body>
</html>
<script>
  document.getElementById('pushBtn')?.addEventListener('click', () => {
    history.pushState({ foo: "bar" }, '', '/');
  });
  document.getElementById('replaceBtn')?.addEventListener('click', () => {
    history.replaceState({ foo: "baz" }, '', '/');
  });
</script>
```

![](/images/advent-calendar-2025-12-5/state.png)


## クイズ
1.`history.go()`に`0`を代入、または引数なしで呼び出すとどのような挙動になるでしょうか。
:::details 回答
現在のページの再読み込みをする。
:::

## まとめ
history APIはブラウザの履歴からページへの移動であったり、ページに対してstateの保存・更新が出来たりすることが分かりました。
次回はNavigationAPIについてまとめていきたいと思います。

## 参考
https://developer.mozilla.org/ja/docs/Web/API/History_API
https://developer.mozilla.org/ja/docs/Web/API/History/pushState
https://developer.mozilla.org/ja/docs/Web/API/History/replaceState
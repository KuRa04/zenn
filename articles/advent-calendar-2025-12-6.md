---
title: "Navigation APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第6回は「`History API`」がテーマです。
`History API`とは何か、実際どのように実装するのかを学んでいきます。

## Navigation APIとは
`Navigation API`はブラウザのナビゲーションアクションやアプリケーションの履歴を管理する機能を提供しています。`Navigation API`は`History API`の後継として実装されたAPIです。
`History API`にはいくつか指摘された問題がありました。
- [pushStateの第二引数を削除すべき](https://html5doctor.com/interview-with-ian-hickson-html-editor/#:%7E:text=My%20biggest%20mistake%E2%80%A6there%20are%20so%20many%20to%20choose%20from!%20pushState()%20is%20my%20favourite%20mistake)
- [The case for the new Web History API](https://github.com/dvoytenko/web-history-api/blob/master/problem.md)

`pushState`の第二引数を削除すべきという話はWebAPI共通の悩みのような気もしています。
設計したものが実際に良いものかどうかをテストするために、実世界でテストをしなければいけませんが、そのテストが終わる頃には変更出来る頃合いを過ぎていました。
WebAPIはリリースしたら全世界で利用されるので改善するのは中々難しいことが感じられます。

## Navigation APIの実装例
```html
<html>
  <body>
    <button id="navigationBtn">navigation()</button>
  </body>
</html>
<style>
  h1 {
    color: orange;
  }
</style>
<script>
  navigation.addEventListener('navigate', (event) => {
    event.intercept({
      handler: async () => {
        // state を設定
        navigation.updateCurrentEntry({ state: { foo: "bar" } });
      }
    });
  });

  document.getElementById('navigationBtn')?.addEventListener('click', async () => {
    // URLを/navigation/page1に設定
    await navigation.navigate('/navigation/page1/', { 
      info: "transition", 
      history: "push" 
    }).finished;

    // { foo: "bar" }
    console.log('State:', navigation.currentEntry.getState()); 
  });
</script>
```

## クイズ
上記の実装の状態で、`navigation/page1`の表示対象となる`page1.html`がある場合、
画面は`page1.html`に自動で遷移するでしょうか。

:::details 回答
URLは`navigation/page1`になりますが、自動で`page1.html`には遷移しません。
理由としては`event.intercept()`がブラウザの遷移を防いでいるからです。
手っ取り早く`page1.html`を表示させたいのであれば、`event.intercept()`に関わる処理を削除することで`page1.html`に遷移します。
```js
<script>
  document.getElementById('navigationBtn')?.addEventListener('click', async () => {
    await navigation.navigate('/navigation/page1/', { 
      info: "transition", 
      history: "push" 
    }).finished;
    
    console.log('State:', navigation.currentEntry.getState());
  });
</script>
```
:::

## まとめ
今回は`History API`の後継として実装された`Navigation API`を紹介しました。
`event.intercept()`などの挙動だけでもSPA向きなAPIだなと感じています。

## 参考

https://developer.mozilla.org/en-US/docs/Web/API/Navigation_API
https://html.spec.whatwg.org/multipage/nav-history-apis.html#navigation-api
https://developer.mozilla.org/en-US/docs/Web/API/NavigateEvent/intercept
https://html5doctor.com/interview-with-ian-hickson-html-editor/#:%7E:text=My%20biggest%20mistake%E2%80%A6there%20are%20so%20many%20to%20choose%20from!%20pushState()%20is%20my%20favourite%20mistake
https://qiita.com/Kyo18/items/bc33272725a6f4828da4
https://github.com/dvoytenko/web-history-api/blob/master/problem.md
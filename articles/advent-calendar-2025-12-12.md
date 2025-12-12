---
title: "Polyfillについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第12回は「`Polyfill`」がテーマです。
`Polyfill`とは何か学んでいきます。

## Polyfillとは
`Polyfill`とは、新しく登場したAPIをサポートしていない古いブラウザでその機能を利用可能にするためのコードです。具体例としては、JavaScriptのPromiseがIEではサポートされていませんでした。IEでPromise相当の機能を利用するためには、IEで利用可能なAPIを用いて実現する必要があります。
```js
// Polyfillの例
if (!window.Promise) {
  window.Promise = window.Promiseと同様の処理を記述
}
```
また、ブラウザ間で同じ機能を異なる方法で実装している場合があり、その問題に対処するためにも`Polyfill`を使われることもあります。IEとNetscapeのブラウザ戦争の時代ではブラウザ毎の実装方法が異なることが多く見られました。
現在のブラウザは標準的な意味に従ってAPIを実装しているので、ブラウザの異なる実装のために`Polyfill`を使うことは一般的ではありません。

## まとめ
今回は`Polyfill`について紹介しました。
Web標準を学ぶ中で、新しいAPIの登場に注目していましたが、古いブラウザとの互換性を保つための`Polyfill`という仕組みがあることを知りました。現代のWeb開発では、こうした下位互換性への配慮も重要な視点だと感じました。


## 参考
https://developer.mozilla.org/ja/docs/Glossary/Polyfill
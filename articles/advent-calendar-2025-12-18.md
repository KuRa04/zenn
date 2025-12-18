---
title: "URL APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第18回は「`URL API`」がテーマです。
`URL API`とは何か、どのように実装するのかを紹介します。

## URL APIとは
`URL API`はURL標準のコンポーネントであり、有効なURLやその構成要素にアクセスして操作するAPIを定義します。
URLの正式名称は`Uniform Resource Locator`といい、インターネット上における（ウェブページや画像やといった）リソースの位置を特定するための文字列です。
[HTTP](https://developer.mozilla.org/ja/docs/Glossary/HTTP)において、URLはウェブアドレスやリンクと呼ばれています。
URLはファイル転送や電子メールやその他のアプリケーションでも利用されています。

## URL APIの実装例
**URLコンポーネントにアクセス**
```js
const addr = new URL("https://url.spec.whatwg.org/"); 

const host = addr.host;
console.log(host); // 'url.spec.whatwg.org'

const path = addr.pathname;
console.log(path); // '/'
```

**URLの変更**

```js
const ourUsername = "kuracchi";
const addr = new URL("https://example.com/login");
addr.username = ourUsername;

console.log(addr.username); // 'kuracchi'
```

## まとめ
今回は`URL API`について紹介しました。
`URL`はWebアプリケーションを開発する上では切り離せないものです。
`HTTP`だと呼ばれ方が変わったり、URL自体も色々な要素で作成されていたりするので、URLだけの学習回を作っても良さそうだなと思います。



## 参考
https://developer.mozilla.org/ja/docs/Web/API/URL_API
https://developer.mozilla.org/ja/docs/Glossary/URL
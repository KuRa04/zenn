---
title: "Content Index APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第16回は「`Content Index API`」がテーマです。
`Content Index API`とは何か、どのように実装するのかを紹介します。

## Content Index APIとは
`Content Index API`は特定のオフラインコンテンツをブラウザに通知可能です。
これによりユーザーは利用可能なコンテンツを発見・閲覧可能になり、開発者はコンテンツの追加・管理が可能になります。
具体例としては、ニュースサイトで最新記事をバッググラウンドでprefetchしたり、ストリーミングアプリでダウンロード済みコンテンツを登録したり出来ます。
chrome for developersでは、[Content Indexing API を使ったオフライン対応ページのインデックス登録](https://developer.chrome.com/docs/capabilities/web-apis/content-indexing-api?hl=ja)という形で紹介されています。
この例では、インターネットに接続した状態でアクセス→ページを表示→chromeのメニューからダウンロードを選択→おすすめの記事に切り替えると保存したコンテンツを閲覧可能です。

**提供されているインターフェース**
| 名前 | 詳細 |
| ---- | ---- |
| `ContentIndex` | オフラインで利用可能なコンテンツを登録する機能を提供 |
| `COntentIndexEvent` | `contentdelete`イベントを表すために使用されるオブジェクトを定義 |


**他のインターフェースへの拡張**
`ServiceWorker`へ以下の追加機能が仕様で規定されています。

| 名前 | 詳細 |
| ---- | ---- |
| `ServiceWorkerRegistration.index` | キャッシュされたページをインデックス化するための`ContentIndex`インターフェースを返す |
| `contentdelete` | ユーザーエージェントによってコンテンツが削除された時に発火する |


## Content Index APIの実装例
```js
// our content
const item = {
  id: "post",
  url: "/blogs/2025-12-17.html",
  title: "Content Index API",
  description:
    "Content indexing allows developers to tell the browser about their specific offline conten",
  icons: [
    {
      src: "/media/web.png",
      sizes: "128x128",
      type: "image/png",
    },
  ],
  category: "article",
};

async function registerContent(data) {
  const registration = await navigator.serviceWorker.ready;

  if (!registration.index) {
    return;
  }

  // コンテンツインデックスの登録
  try {
    await registration.index.add(data);
  } catch (e) {
    console.log("Failed to register content: ", e.message);
  }
}
```

## まとめ
今回は`Content Index API`について紹介しました。
オフラインでもデータを閲覧出来るのはとても便利ですね。


## 参考
https://developer.mozilla.org/en-US/docs/Web/API/Content_Index_API
https://developer.chrome.com/docs/capabilities/web-apis/content-indexing-api?hl=ja
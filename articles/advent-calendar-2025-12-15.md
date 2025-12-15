---
title: "Device Memory APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第15回は「`Device Memory API`」がテーマです。
`Device Memory API`とは何か、どのように実装するのか、実装例を交えて紹介します。

## Device Memory APIとは
`Device Memory API`とは自分が利用している端末のRAM容量を概算で出してくれるAPIです。
RAM容量にアクセスする方法は2つあり、JavaScript APIを使用する方法とクライアントヒントHTTPヘッダーを使用する方法になります。
クライアントヒントとは、端末、ネットワーク、ユーザー、ユーザーエージェント固有の環境設定に関する情報を取得することが出来るHTTPリクエストヘッダーのフィールド群です。
`Accept-CH: Width, Downlink, Sec-CH-UA`のように`Accept-CH`を利用して受信したいヒントを指定できます。

**JavaScript API**
`navigator.deviceMemory`を使用
```js
const RAM = navigator.deviceMemory;
```

**クライアントヒント**
クライアントヒントヘッダーである`Device-Memory`ディレクティブを使用
```http
Accept-CH: Device-Memory
```

このAPIはChrome、Edge、Operaで利用可能ですが、FirefoxとSafariでは実装されていません。
[Battery Status API](https://zenn.dev/kura_04/articles/advent-calendar-2025-12-13)で、FireFoxががプライバシーの懸念から`Battery Status API`を削除したとありました。
`Device Memory API`も同じような文脈で実装していないのか気になるところですが、そのような情報は見当たりませんでした。
引き続き、色々なWeb APIを調べてブラウザーベンダー毎の違いを理解していきたいと思います。

## Device Memory APIの実装例
メモリの容量によって、色と文言を変化させる実装を行いました。
@[codepen](https://codepen.io/KuRa04-the-sans/pen/zxqQJKr)


## まとめ
今回は`Device Memory API`について紹介しました。
利用しているデバイスのRAM容量を取得可能なことから、メモリに応じて軽い・重い処理を切り替えることが出来ます。
また、アドカレ13日目の`Battery Status API`に続きデバイスの情報を取れるということでOSとWeb APIの関係も気になってきたところです。
アドカレで知識が広がっている感覚があるので、無理せず続けていきたいと思います。

## 参考
https://developer.mozilla.org/en-US/docs/Web/API/Device_Memory_API
https://developer.mozilla.org/ja/docs/Web/HTTP/Guides/Client_hints
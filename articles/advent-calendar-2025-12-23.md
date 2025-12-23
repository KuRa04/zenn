---
title: "Popover APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第23回は「`Popover API`」がテーマです。
`Popover API`とは何か、どのように実装するのかを紹介します。

## Popover APIとは
`Popover API`は他のコンテンツの上に表示する「ポップオーバーコンテンツ」を表示するための仕組みを提供します。ポップオーバーコンテンツは、HTML属性またはJavaScriptを用いて制御することが出来ます。
本当にHTML属性だけで実現できるのか？と思いましたが、本当でした。笑
下記の実装例では「ポップオーバーの切り替え」ボタンを押すと、画面中央にHTMLが表示されます。もう一度押すと非表示になります。

@[codepen](https://codepen.io/KuRa04-the-sans/pen/dPXyMbO)

モーダルを作成したい場合は、このポップオーバーではなく`<dialog>`を利用するのが適切です。
ポップオーバーは非モーダルと定義されているからです。モーダルと非モーダルの説明は下記にまとめています。

| 名称 | 概要 |
|------|------------------|
| モーダル | ポップオーバーが示されている間、他のコンテンツはポップオーバーがアクションを<br>起こすまで、反応しない |
| 非モーダル |ポップオーバーが表示されている間も、ページの残りの部分が反応する |


## Popover APIの実装例
先程HTMLで行ったものをJavaScriptを使って実現した例
@[codepen](https://codepen.io/KuRa04-the-sans/pen/emzYZpq)

## まとめ
今回は`Popover API`について紹介しました。
HTMLでポップオーバーの表示・非表示の切り替えが出来ることに驚きました。

## 参考
https://developer.mozilla.org/ja/docs/Web/API/Popover_API

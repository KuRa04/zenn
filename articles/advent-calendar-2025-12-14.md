---
title: "HTML Drag and Drop APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第14回は「`HTML Drag and Drop API`」がテーマです。
`HTML Drag and Drop API`とは何か、どのように実装するのか、実装例を交えて紹介します。

## HTML Drag and Drop APIとは
ブラウザでドラッグ&ドロップ機能を利用することが可能です。
ドラッグ&ドロップには3つの異なる用途があります。
「ページ内での要素のドラッグ」「ページからのドラッグ」「ページへのデータのドラッグ」です。`HTML Drag and Drop API`はこれらの動作が出来ます。

**ドラッグイベント一覧**
ドラッグイベントは7つあり、数としては多く感じますが内容としてはシンプルなものです。

| イベント | 発生する条件 |
| ---- | ---- |
| `dragstart` | `draggable=true`のアイテムがドラッグ開始された時 |
| `drag` | `draggable=true`のアイテムがドラッグされている時 |
| `dragenter` | その要素に入ってきた`draggable=true`のアイテムがある時 |
| `dragleave` | その要素から出ていく`draggable=true`のアイテムがある時 |
| `dragover` | その要素の上を`draggable=true`のアイテムがドラッグされている時 |
| `drop` | その要素がドロップ対象であり、`draggable=true`のアイテムがその上にドロップされた時 |
| `dragend` | `draggable=true`のアイテムのドラッグが終了されたとき。 |

**ドラッグ可能/不可能にする**
任意の要素をドラッグ可能にするには、`draggable`属性を`true`に設定します。
@[codepen](https://codepen.io/KuRa04-the-sans/pen/ZYWZLEV)

**ドラッグしたアイテムをドロップする**
@[codepen](https://codepen.io/KuRa04-the-sans/pen/emZogxm)

## HTML Drag and Drop APIの実装例
HTML Drag and Drop APIを使ったシンプルなパズルゲームを実装しました。
様々なドラッグイベントを活用して実装したので、ぜひ覗いてみてください。
@[codepen](https://codepen.io/KuRa04-the-sans/pen/GgZLmpK)

## まとめ
今回は`HTML Drag and Drop API`について紹介しました。
ドラッグアンドドロップの実装はライブラリを使うイメージを持っていたのですが、WebAPIで可能だったのは驚きです。
Web API単体で出来ることはたくさんあるので、しっかりと覚えていきたいと思います。

## 参考
https://developer.mozilla.org/ja/docs/Web/API/HTML_Drag_and_Drop_API
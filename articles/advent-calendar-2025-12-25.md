---
title: "Canvas APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第25回は「`Canvas API`」がテーマです。
`Canvas API`とは何か、どのように実装するのかを紹介します。

## Canvas APIとは
`Canvas API`はJavaScriptと`<canvas>`よってグラフィックを描く方法を提供しています。
アニメーションや、ゲームのグラフィック、データの可視化などに使用することが出来ます。
`Canvas API`は2Dグラフィックを対象としているため、3Dグラフィックを扱いたい場合はWebGL APIを使いましょう。

下記のように、`document.getElementById`でcanvas要素を取得し、`HTMLCanvasElement.getContext()`で要素のコンテキスト要素を取得します。
`fillStyle`プロパティで色を選択できたり、`fillRect()`メソッドで図形の位置や大きさを指定できたりします。
@[codepen](https://codepen.io/KuRa04-the-sans/pen/jErOjJV)

## Canvas APIの実装例
クリスマス🎄なのでサンタを描いてみました。

@[codepen](https://codepen.io/KuRa04-the-sans/pen/JoKjQwB)

## まとめ
今回は`Canvas API`について紹介しました。
描画の自由度が高いので、色々な表現が出来ると思います。
そして、本日で25日間の1人アドカレ終了しました！
このアドカレを通して得た学びを活かし、来年はさらに成長していきたいと思います！

## 参考
https://developer.mozilla.org/ja/docs/Web/API/Canvas_API
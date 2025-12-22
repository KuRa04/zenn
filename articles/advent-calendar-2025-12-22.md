---
title: "Page Visibility APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第22回は「`Page Visibility API`」がテーマです。
`Page Visibility API`とは何か、どのように実装するのかを紹介します。

## Page Visibility APIとは
`Page Visibility API`は現在ページが見えているかどうかを調べる機能とともに、表示・非表示になった時を監視するイベントを提供しています。
画面が表示されていない（別タブや最小化）時に不必要なタスクを止めることでリソースを節約するなどが出来ます。
例えば、ユーザが動画を視聴している時に画面を最小化したら動画を一時停止させる、のようなことが可能です。UXの観点でも活用できそうですね。
MDNでは、他にも下記のような使用例が紹介されていました。
- 画像のスライドショーがあるサイトで、ユーザが見ていない間に次のスライドに進むべきではないもの
- 情報をダッシュボードに表示するアプリで、ページが見えていない時は更新情報をサーバーへリクエストしてほしくないもの
- 端末がスタンバイモードである時に音声を止めたいサイト

## Page Visibility APIの実装例
別タブや最小化をした時に動画を一時停止する
@[codepen](https://codepen.io/KuRa04-the-sans/pen/GgqKwja)

## まとめ
今回は`Page Visibility API`について紹介しました。
`Page Visibility API`では別タブや最小化など、ページが表示されていない時の実装が可能です。

## 参考
https://developer.mozilla.org/ja/docs/Web/API/File_System_API

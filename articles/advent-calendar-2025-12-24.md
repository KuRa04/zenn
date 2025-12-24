---
title: "Fullscreen APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第24回は「`Fullscreen API`」がテーマです。
`Fullscreen API`とは何か、どのように実装するのかを紹介します。

## Fullscreen APIとは
`Fullscreen API`は特定の要素を全画面モードで表示したり、全画面モードを抜けたりする方法を提供します。
`Fullscreen API`はDocument、Elementのインターフェースにメソッドを追加しており、全画面モードを追加したり終了したりすることが可能です。

| 名称 | 概要 |
|------|------------------|
| `Document.exitFullscreen()` | 全画面モードからウインドウモードに切り替えることをリクエストする |
| `Element.requestFullscreen()` |　全画面モードに切り替えることをリクエストする |

また、全画面モードを解除するのを実装で行うのではなく、`ESC`や`F11`キーを押すことで抜けることも可能です。
そして、ユーザに対してこれらの操作が出来ることを伝える設計にした方が良いとされています。
Chromeで全画面表示すると「全画面表示を終了するにはescを長押しします」と表示されます。

![](/images/advent-calendar-2025-12-24/exit_fullscreen_chrome.png)


## Fullscreen APIの実装例
「全画面表示」ボタンを押下すると全画面表示に切り替わる例です。
※zennのウインドウだとvideoの全画面が出来ないようなので、私のcodepenに遷移するかローカルでお試しください。
@[codepen](https://codepen.io/KuRa04-the-sans/pen/wBWvmqv)

## まとめ
今回は`Fullscreen API`について紹介しました。
全画面モードの切り替えはOSのショートカットで行っていましたが、それがAPIでも可能なのですね。

## 参考
https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API

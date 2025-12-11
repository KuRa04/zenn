---
title: "CSS anchor positioningについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第11回は「`CSS anchor positioning`」がテーマです。
`CSS anchor positioning`とは何か、実際どのように実装するのかを学んでいきます。

## CSS anchor positioningとは
`CSS anchor positioning`はアンカー要素に紐づいた要素(以下、`anchor-positioned elements`)のサイズや位置をアンカー位置に対して相対的に設定できます。`anchor-positioned elements`の具体例としてはツールチップやダイアログが挙げられます。
`anchor-positioned elements`は複数の代替位置を指定する仕組みを提供しています。例えば、PCのホーム画面でコンテキストメニューを表示させると、どの場所で開いても画面内に収まるように自動調整されます。これと同様に、画面端でツールチップを表示する際も、規定の設定では画面外にレンダリングされてしまうものが、画面内に収まるように配置できます。
従来、アンカー要素に基づいて`anchor-positioned elements`の位置やサイズを動的に変更するには`JavaScript`が必要でしたが、実装には複雑さやパフォーマンスの問題が伴っていました。`CSS anchor positioning`の登場により、これをCSSのみで実装できるようになりました。

## CSS anchor positioningの実装例
**動作の流れ**
「Hover me」にhoverするとtooltipが表示

![](/images/advent-calendar-2025-12-11/css_anchor_positioning.png)


```html
<html>
  <head>
    <title>CSS Anchor Positioning</title>
    <style>
      body {
        font-family: system-ui, sans-serif;
        padding: 2rem;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
      }

      .container {
        text-align: center;
      }

      h1 {
        margin-bottom: 2rem;
      }

      /* anchor elements */
      .anchor-button {
        anchor-name: --my-anchor;
        padding: 12px 24px;
        font-size: 16px;
        background: #007bff;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
      }

      .anchor-button:hover {
        background: #0056b3;
      }

      /* anchor-positioned elements */
      .tooltip {
        position: absolute;
        position-anchor: --my-anchor;
        bottom: anchor(top);
        left: anchor(center);
        translate: -50% -8px;
        
        padding: 8px 12px;
        background: #333;
        color: white;
        border-radius: 4px;
        font-size: 14px;
        white-space: nowrap;
        opacity: 0;
        pointer-events: none;
        transition: opacity 0.2s;
      }

      .tooltip::after {
        content: '';
        position: absolute;
        top: 100%;
        left: 50%;
        translate: -50% 0;
        border: 6px solid transparent;
        border-top-color: #333;
      }

      .anchor-button:hover + .tooltip,
      .anchor-button:focus + .tooltip {
        opacity: 1;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <h1>CSS Anchor Positioning Demo</h1>
      <button class="anchor-button">Hover me</button>
      <div class="tooltip">This is a tooltip</div>
    </div>
  </body>
</html>
```

## まとめ
今回は`CSS anchor positioning`について紹介しました。
JavaScriptで行われていたツールチップやダイアログの場所の計算がCSSで可能になるのはすごいですね。

## 参考
https://developer.mozilla.org/ja/docs/Web/CSS/Guides/Anchor_positioning/Using
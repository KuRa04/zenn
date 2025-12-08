---
title: "View Transition APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第8回は「`View Transition API`」がテーマです。
`View Transition API`とは何か、実際どのように実装するのかを学んでいきます。

## View Transition APIとは
異なるページの移動をアニメーション遷移を簡単に作成する仕組みです。
SPAにおけるDOMの状態変化のアニメーションなどの移動時のアニメーションも含みます。
`View Transition API`はアプリケーションの状態またはレビュー間を移動する際の認知的負荷を小さくして読み込み待ちの知覚時間を短縮するための有力な設計の選択肢です。
SPAで状態変化の遷移を行うためにはCSSとJavaScriptを頑張る必要があったのですが、このAPIを使うことで簡単に実現できます。
[アドカレ6日目](https://zenn.dev/kura_04/articles/advent-calendar-2025-12-6)で`Navigation API`を紹介したのですが、このAPIと相性が良さそうですね！

## View Transition APIの実装例
**動作の内容**
![](/images/advent-calendar-2025-12-8/viewtransition.gif)

**動作の流れ**
1. index.htmlのNavigate with View Transitionをクリック
2. フェードイン・アウトのアニメーションでpage1.htmlに遷移

`index.html`
```html
<html>
  <head>
    <title>View Transition with Navigation API</title>
  </head>
  <body>
    <h1 id="demoTitle">View Transition Demo</h1>
    <button id="navigationBtn">Navigate with View Transition</button>
  </body>
</html>
<style>
  h1 {
    color: orange;
  }
  
  /* View Transition アニメーションの設定 */
  ::view-transition-old(root) {
    animation: fade-out 0.3s ease-out;
  }
  
  ::view-transition-new(root) {
    animation: fade-in 0.3s ease-in;
  }
  
  @keyframes fade-out {
    from {
      opacity: 1;
    }
    to {
      opacity: 0;
    }
  }
  
  @keyframes fade-in {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
</style>
<script>
  // Navigation API で navigate イベントをインターセプトし、View Transitionを実行
  navigation.addEventListener('navigate', (event) => {
    event.intercept({
      handler: async () => {
        console.log('Navigating to:', event.destination.url);
        
        // View Transition API でページをアニメーションを用いて遷移
        if (document.startViewTransition) {
          await document.startViewTransition(async () => {
            // 新しいページへの遷移処理
            const response = await fetch(event.destination.url);
            const html = await response.text();
            const parser = new DOMParser();
            const doc = parser.parseFromString(html, 'text/html');

            document.body.replaceChildren(...doc.body.childNodes);
          }).finished;
        }
      }
    });
  });

  document.getElementById('navigationBtn')?.addEventListener('click', async () => {
    await navigation.navigate('/viewtransition/page1/', { 
      info: "transition", 
      history: "push" 
    }).finished;
    
    console.log('State:', navigation.currentEntry.getState());
  });
</script>
```

`page1.html`
```html
<html>
  <head>
    <title>Page 1 - View Transition</title>
  </head>
  <body>
    <h1 style="color: blue;">Page 1</h1>
    <button id="backBtn">Back to Index</button>
  </body>
</html>
<style>
  h1 {
    color: blue;
  }
  
  body {
    padding: 20px;
    font-family: sans-serif;
  }
  
  button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }
</style>
```

## まとめ
今回はView Transition APIを紹介しました。
直近で学習したAPIとの関連付けが出来たのでより深く学べた気がします。

## 参考
https://developer.mozilla.org/ja/docs/Web/API/View_Transition_API
https://developer.chrome.com/docs/web-platform/view-transitions?hl=ja
https://zenn.dev/sonicmoov/articles/f819f4904e793f
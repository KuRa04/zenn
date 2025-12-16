---
title: "Document Picture-in-Picture APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第16回は「`Document Picture-in-Picture API`」がテーマです。
`Document Picture-in-Picture API`とは何か、どのように実装するのかを紹介します。

## Document Picture-in-Picture APIとは
`Document Picture-in-Picture API`を利用することによって、常に最前面に表示されるウインドウの実装が可能で、任意のHTMLコンテンツを表示出来ます。
これは`<video>`用の`Picture-in-Picture`を拡張したもので、`<video>`要素を常時最前面のウインドウに配置することが可能です。
Webアプリを実行しているメインウインドウとは別に、ウインドウを用意できると便利な場合があります。
特定のアプリコンテンツを表示したまま他のウインドウを閲覧したい場合や、そのコンテンツに専用のスペースを与えつつ、メインアプリのウインドウを他のコンテンツ表示用に開けておきたい場合です。
通常のブラウザで新しくウインドウを開くことでも対応可能ではありますが、これには2つの大きな問題があります。
1つ目は2つのウインドウ間で状態情報の共有を処理する必要があること、2つ目は追加のアプリウインドウは常に最前面に表示されるわけではないことです。
これらの問題を解決するため、Webブラウザはアプリが同じセッションの一部となる常時最前面表示ウインドウを生成できるAPIを導入しました。

## Document Picture-in-Picture APIの実装例
「PiPで開く」を押下すると`Picture-in-Picture`の表示になる実装です。

![](/images/advent-calendar-2025-12-16/home.png)
![](/images/advent-calendar-2025-12-16/pip.png)
```html
<html lang="ja">
  <head>
    <meta charset="UTF-8">
    <title>Document PiP</title>
  </head>
  <body>
    <h1>Document Picture-in-Picture API</h1>
    <video id="video" width="400" controls muted>
      <source src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm" type="video/webm">
    </video>
    <br>
    <button id="pipBtn">PiPで開く</button>

    <script>
      const video = document.getElementById('video');
      const pipBtn = document.getElementById('pipBtn');

      pipBtn.addEventListener('click', async () => {
        // PiPウィンドウを開く
        const pipWindow = await window.documentPictureInPicture.requestWindow({
          width: 320,
          height: 240,
        });

        // videoをPiPウィンドウに移動
        pipWindow.document.body.appendChild(video);

        // PiPウィンドウが閉じられたらvideoを元に戻す
        pipWindow.addEventListener('pagehide', () => {
          pipBtn.after(video);
        });
      });
    </script>
  </body>
</html>
```


## まとめ
今回は`Document Picture-in-Picture API`について紹介しました。
PiPは普段の生活にも馴染みのあると思います。動画を扱うアプリには特に取り入れられやすいものだと思いますが、それがWeb APIで実装できるのは良いですね。


## 参考
https://developer.mozilla.org/en-US/docs/Web/API/Document_Picture-in-Picture_API
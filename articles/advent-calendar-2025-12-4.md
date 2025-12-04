---
title: "BroadcastChannelAPIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第3回は「`BroadcastChannelAPI`」がテーマです。
`BroadcastChannelAPI`とは何か、実際どのように実装するのかを学んでいきます。

## BroadcastChannelAPIとは
`BroadcastChannelAPI`は同じオリジンの閲覧コンテキスト（ウインドウ、タブ、フレーム、iframeなど）やワーカー間で、メッセージの送受信を行うことが出来ます。
`BroadcastChannel`オブジェクトを作成することで、メッセージの送受信が可能になります。
![](/images/advent-calendar-2025-12-4/abstract.png)

**ブロードキャストチャンネルへの接続**
`BroadcastChannel`のオブジェクトはチャンネル名を引数として作成出来ます。
```js
const broadcastChannel = new BroadcastChannel('my_channel');
```

**メッセージの送信**
`postMessage()`の引数にメッセージを代入すれば良いです。
これだけで同じオリジンのタブなどにメッセージを送ることが出来ます。
```js
broadcastChannel.postMessage("This is a test message.");
```

**メッセージの受信**
`onMessage()`の`event`に送信されたデータが格納されています。
メッセージの内容は`event.data`で取得可能です。
```js
broadcastChannel.onmessage = (event) => {
  console.log(event.data);
};
```

**チャンネルの切断**
チャンネルの切断には`close()`を呼び出す必要があります。
これによりオブジェクトとチャンネルを切断し、GCをすることが出来ます。
```js
broadcastChannel.close();
```


## BroadcastChannelAPIの実装例
**動作の流れ**
1. 同じオリジンのページを別タブで複数開いている状態で片方の画面をリロード
  a. 下記の画像では左側の画面をリロード
2. 左側の画面描画時にメッセージを送信
3. 右側の画面では、左側の画面から受信したメッセージをログに出力

**ログが出力されていることを確認**
![](/images/advent-calendar-2025-12-4/broadcastChannel.png)


**画面描画時にメッセージを送受信するメソッド**
```js
...
const broadcastChannel = new BroadcastChannel('my_channel');
broadcastChannel.postMessage('Hello from index page');
broadcastChannel.onmessage = (event) => {
  console.log('Received message: ', event.data);
};

window.addEventListener('beforeunload', () => {
  broadcastChannel.close();
});
...
```


## 参考
https://developer.mozilla.org/ja/docs/Web/API/Broadcast_Channel_API
https://developer.mozilla.org/ja/docs/Glossary/Origin
https://developer.mozilla.org/ja/docs/Glossary/Browsing_context
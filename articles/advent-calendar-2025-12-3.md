---
title: "Streams APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第3回は「Streams API」がテーマです。
Streams APIとは何か、実際どのように実装するのかを学んでいきます。

## Streams APIとは
Streams APIは、JavaScriptでデータの流れを扱うためのAPIです。大きなファイルやネットワークから受信するデータを、すべてメモリに読み込まず、少しずつ処理できる仕組みを提供します。

従来のJavaScriptでは、データを扱う際に配列やBlobとして一度にすべてをメモリに載せる必要がありました。Streams APIを使えば、データを小さな塊（chunk）に分割し、順次処理できます。
また、ストリームの開始や連鎖、エラー処理と必要に応じたストリームのキャンセル、ストリームの読み取り速度へ対応が可能です。

Streams APIは、WHATWGによって策定され、現在は主要ブラウザで広くサポートされており、[Streams Living Standard](https://streams.spec.whatwg.org/)に仕様書が記載されています。

## 3種類の主要なストリーム
**1. ReadableStream（読み取り可能ストリーム）**
データソースからデータを読み取るためのストリームです。
`fetch()`APIのレスポンスボディや、ファイル読み込みなどで使用されます。

**2. WritableStream（書き込み可能ストリーム）**
データを書き込む先を表すストリームです。
受け取ったデータを保存したり、別の処理に渡したりします。

**3. TransformStream（変換ストリーム）**
`WritableStream`と`ReadableStream`のペアから構成されるストリームです。データの変換処理として機能し、`ReadableStreamのpipeThrough()`でパイプラインに挿入できます。
データの圧縮・解凍、テキストのエンコーディング変換などに使用されます。


## ReadableStreamの実装例

**動作の流れ**
1. 最初のreader.read(): キューが空なので、データが来るまで待機
2. 1秒後: controller.enqueue()が実行され、チャンク1がキューに追加
3. reader.read()が解決: キューにデータが入ったので、Promiseが解決されて値を返す
4. 画面に表示: valueを使って画面表示
5. 次のreader.read(): 再びキューが空なので待機...
6. 1秒後: チャンク2が追加され、またPromiseが解決...
@[codepen](https://codepen.io/KuRa04-the-sans/pen/azNagdG)

#### 1. stream機能の設定
- `start(controller)`:  `ReadableStream`が作成された後に1回だけ呼び出されるメソッドでストリーム機能を設定するコードを含める必要がある
- `controller.enqueue()` : 指定されたチャンクを関連する読み取り可能なバイトストリームのキューに入れる


```js
// `start()`の中で1秒毎にチャンクをキューに詰める
const stream = new ReadableStream({
  start(controller) {
    let count = 1;
    const interval = setInterval(() => {
      if (count <= 5) {
        controller.enqueue(`チャンク ${count}`);
        count++;
      } else {
        clearInterval(interval);
        controller.close();
      }
    }, 1000);
  }
});
```

#### 2. streamの読み取り
- `getReader() `: リーダーを作成し、ストリームをロック
  -  ストリームがロックされている間は、このリーダーが解放されるまで他のリーダーを取得出来ない
- `read()`: ストリームの内部キュー内の次のチャンクへのアクセスを提供するプロミスを返す
- `releaseLock()`: ストリームのリーダーのロックを解除
```js
// 追加されたキューを解決して出力
const reader = stream.getReader();
try {
  while (true) {
    const { done, value } = await reader.read();

    if (done) {
      output.innerHTML += '<p style="color: green; font-weight: bold;">✓ ストリーム完了</p>';
      break;
    }
    
    const chunkDiv = document.createElement('div');
    chunkDiv.className = 'chunk';
    chunkDiv.textContent = `受信: ${value}`;
    output.appendChild(chunkDiv);
  }
} finally {
  reader.releaseLock();
}
```


## 参考
https://developer.mozilla.org/ja/docs/Web/API/Streams_API
https://developer.mozilla.org/ja/docs/Web/API/ReadableStreamDefaultController
https://developer.mozilla.org/ja/docs/Web/API/ReadableByteStreamController/enqueue
https://developer.mozilla.org/ja/docs/Web/API/ReadableStream/getReader
https://developer.mozilla.org/ja/docs/Web/API/ReadableStreamDefaultReader/read
https://developer.mozilla.org/ja/docs/Web/API/ReadableStreamDefaultReader/releaseLock
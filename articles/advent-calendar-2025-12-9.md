---
title: "Clipboard APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第8回は「`Clipboard API`」がテーマです。
`Clipboard API`とは何か、実際どのように実装するのかを学んでいきます。

## Clipboard APIとは
clipboardapi はクリップボードの切り取り、コピー、貼り付けに応答する機能や、システムクリップボードの非同期の読み取りや書き込み機能を提供しています。
保護されたコンテキストで、[セキュリティに関する考慮](https://developer.mozilla.org/ja/docs/Web/API/Clipboard_API#%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%81%AE%E8%80%83%E6%85%AE)の条件が成立する時に利用可能です。
ブラウザーの実装は仕様から剥離しており、その概要は先程のセキュリティに関する考慮に記載されています。
例えば、Chromium系では読み取りが許可されておらず、clipboard-read権限を要求します。
ユーザーが許可するなど、権限が許可された場合には読み取りに成功します。
下記はChromeで`navigator.clipboard.readText()`を実行した時に権限を要求されたものです。
![](/images/advent-calendar-2025-12-9/paste_permission.png)


## Clipboard APIの実装例
**動作の流れ**
1. copyテキストの下部にあるinputに入力
2. copyボタンを押下するとinputに入力した内容をclipboardにcopy
3. paestボタンを押下するとclipboardにcopyした内容をpeastテキストの下部にあるinputに反映
   clipboardにはコピーされているので`cmd + V`などの貼付け操作でも可能

![](/images/advent-calendar-2025-12-9/clipboard_copy_paste.gif)

**コード**
```html
<html>
  <body>
    <p>copy</p>
    <input id="copyInput" type="text">
    <button id="copyBtn">copy</button>
    <br/>
    <p>paste</p>
    <input id="pasteInput" type="text">
    <button id="pasteBtn">paste</button>

  </body>
</html>
<script>
  document.getElementById('copyBtn').addEventListener('click', async () => {
    try {
      await navigator.clipboard.writeText(document.getElementById('copyInput').value);
    } catch (err) {
      console.error('Failed to copy text: ', err);
    }
  });

  document.getElementById('pasteBtn').addEventListener('click', async () => {
    try {
      const text = await navigator.clipboard.readText();
      document.getElementById('pasteInput').value = text;
    } catch (err) {
      console.error('Failed to read clipboard contents: ', err);
    }
  });
</script>
```

## まとめ
今回は`Clipboard API`を紹介しました。
仕様を読み、clipboardにセキュリティの観点が必要なことを初めて知りました。
コピーした内容に不正にアクセスされる場合があることを考えると危険ですね。

## 参考
https://developer.mozilla.org/ja/docs/Web/API/Clipboard_API
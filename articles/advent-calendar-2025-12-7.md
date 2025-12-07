---
title: "HTML Sanitizer APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第7回は「`HTML Sanitizer API`」がテーマです。
`HTML Sanitizer API`とは何か、実際どのように実装するのかを学んでいきます。

## HTML Sanitizer APIとは
`HTML Sanitizer API`はHTML文字列を受け取り、`DOM`または [ShadowDOM](https://developer.mozilla.org/ja/docs/Web/API/Web_components/Using_shadow_DOM)に挿入される際に不要な要素や属性、その他の`HTML`をフィルタリングします。Webアプリケーションはクライアント側で作成したデータをもとにHTMLを構築することがありますよね。
この時に`<script>`を埋め込んだり、`onError`を埋め込んだりすると、アプリケーションとして意図しない挙動をする可能性があります。（[XSS攻撃](https://developer.mozilla.org/ja/docs/Web/Security/Attacks/XSS)）
このような攻撃から守るために、不要な要素や属性などをフィルタリングする処理が必要です。
`HTML`を代入するプロパティとして、`documentElement.innerHTML`が提供されていますが、ユーザーの入力データを利用したい場合は推奨されていません。
理由としては、意図しない要素や属性が埋め込まれる可能性があり、`innerHTML`はフィルタリングを行っていないからです。
ユーザー入力データを利用して`HTML`を構築する場合は`HTML Sanitizer API`が提供している`setHTML`を使うのが良いとされています。
ただ、`2025/12/7`時点では`HTML Sanitizer API`は主要なブラウザで実装されておらずベースラインになっていないので、WebAPIとして利用することが出来ません。
`HTML`のサニタイズで`OSS`として提供されているものだと[DOMPurify](https://github.com/cure53/DOMPurify/tree/main/src)がありますね。
`HTML Sanitizer API`と同じく不要な要素や属性、`HTML`をフィルタリング出来ます。
しかし、新しいHTML要素が追加されたり、利用者側のアップデートが遅れたりすると脆弱性につながる可能性があると考えています。
`HTML Sanitizer API`であれば、このあたりは保証されるはずです。
そういった意味でも、`HTML Sanitizer API`がベースラインになってほしいという気持ちがあります。

## HTML Sanitizer APIの実装例
```html
<html>
  <body>
    <a id="target" href="/history/page2">Go to Page 2</a>
  </body>
</html>

<script>
  const unsanitizedHTML = "untrastedHTML <script>alert(1)<" + "/script>";
  const target = document.getElementById("target");

  // setHTMLでサニタイズ
  target.setHTML(unsanitizedString);

  // Sanitizerのコンストラクタでサニタイズの詳細設定
  // setHTMLの第二引数は任意の引数のoptionsを代入可能
  // optionsにサニタイズの設定を代入すると反映される
  const sanitizer1 = new Sanitizer({
    elements: ["div", "p", "button", "script"],
  });
  target.setHTML(unsanitizedHTML, { sanitizer: sanitizer1 });

  // removeElementsはサニタイズによって削除される要素を指定
  // 反対に、サニタイズをしない設定としてallowElementがある
  target.setHTML(unsanitizedHTML, {
    sanitizer: { removeElements: ["div", "p", "button", "script"] },
  });
</script>
```

## クイズ
2025/12/7時点で、ユーザーの入力を`HTML`に組み込む場合、次のどれを使うべきでしょうか。

1. `DOMPurify.sanitize()`
2. `documentElement.setHTML`
3. `documentElement.innerHTML`

:::details 回答
1. `DOMPurify.sanitize()`が正解。
`documentElement.setHTML`はAPIとして提供されていない、`documentElement.innerHTML`はサニタイズがされていないため不正解。
:::

## まとめ
今回は`HTML Sanitizer API`を紹介しました。
Web APIとしてサニタイズの機能を持っているのは便利だと思うので、ベースラインになると嬉しいなと思います。

## 参考
https://developer.mozilla.org/en-US/docs/Web/API/HTML_Sanitizer_API
https://wicg.github.io/sanitizer-api/
https://developer.mozilla.org/en-US/docs/Web/API/Element/setHTML
https://developer.mozilla.org/en-US/docs/Web/API/Sanitizer
https://developer.mozilla.org/en-US/docs/Web/API/Sanitizer/Sanitizer
https://developer.mozilla.org/en-US/docs/Glossary/Baseline/Compatibility
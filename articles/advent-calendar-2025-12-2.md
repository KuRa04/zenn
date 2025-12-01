---
title: "W3C/WHATWGについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第2回は「Web標準を策定する標準化団体」がテーマです。
どのように標準化団体が出来たのか、標準化団体は何をしているのかなどを学んでいきます。
それでは、Web標準を策定する標準化団体について見ていきましょう。


## W3CとWHATWGの誕生
Web標準を策定する組織として、W3C（World Wide Web Consortium）とWHATWG（Web Hypertext Application Technology Working Group）という2つの団体が存在します。なぜ2つの組織が必要なのでしょうか。
1994年、Webの創始者ティム・バーナーズ=リーによって設立されました。HTMLやCSS、DOMなど、Web技術全般の標準化を担当し、長年にわたってWebの発展を支えてきました。W3Cは段階的な勧告プロセスを採用し、慎重に標準を策定していきます。
2004年頃、W3CがXHTMLという新しい方向性を推進する中で、Apple、Mozilla、Operaなどが実用的なHTML進化の必要性を感じていました。XHTMLは既存のHTMLと互換性がなく、「Webを壊さない」という原則に反していたためです。XHTMLでは、「タグを省略してはならない」、「属性値の空白は`&nbsp;`と記述する」などのルールがありました。こうした背景から、WHATWGが設立されました。

## それぞれが担当する技術領域
現在、2つの組織は異なる技術領域を担当しています。
WHATWGは、ブラウザのコア機能に関わる仕様を管理しています。代表的なものがHTMLです。例えば、`<div>`や`<button>`といった要素の定義、`fetch()`APIの仕様などは全てWHATWGで標準化されています。その他にも、DOM（Document Object Model）、URL、Streams、Encodingなど、ブラウザの基盤となる技術を担当しています。
一方、W3Cは幅広い技術領域をカバーしています。CSSはW3Cが管理しており、レイアウトやデザインに関する仕様が継続的に開発されています。WebAssemblyもW3Cの管轄で、ブラウザ上で高速に動作するバイナリフォーマットの標準化を進めています。また、WAI-ARIAなどのアクセシビリティ関連の仕様や、プライバシー・セキュリティに関する技術も担当しています。

## 仕様をキャッチアップする方法
WHATWGの仕様は https://html.spec.whatwg.org/ で確認出来ます。
W3Cの仕様は https://www.w3.org/TR/ に一覧があり、各技術の勧告や草案を閲覧出来ます。
また、最新動向のキャッチアップには、[MDN Web Docs](https://developer.mozilla.org/)が役立ちます。機能やAPIをどのように使用するかをサンプルコードを用いて説明しています。[MDN Web Docs の掲載基準](https://developer.mozilla.org/ja/docs/MDN/Writing_guidelines/Criteria_for_inclusion)に「信頼できる標準化団体によって公開された仕様書にあり、少なくとも一つの安定したブラウザーで対応しているウェブ標準技術を文書化すること」と記載があり、MDNのドキュメントからWeb標準に沿った仕様を追うことが分かります。
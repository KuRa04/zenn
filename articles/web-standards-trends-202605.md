---
title: "Web 標準動向 2026年5月版"
emoji: "🎏"
type: "idea"
topics: ["frontend", "cybozuwebstandards"]
published: false
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [くらっち (@kuracchi04)](https://x.com/kuracchi04) です。

## はじめに

サイボウズは 2025 年 4 月より、W3C のメンバーに加入しました。

https://blog.cybozu.io/entry/joining-w3c

標準化プロセスに関わることができるようになるための最初の一歩として、フロントエンドエンジニアの一部のメンバーは積極的に Web 標準のキャッチアップを行っています。

そこで、毎月メンバーが興味を持った Web 標準に関する話題や、実際に標準化プロセスに関わることができた場合にはその報告などを 1 つの記事としてまとめ、紹介していきます。
また、ここでは W3C に限らず、TC39 や WHATWG などの標準化団体のトピックについても扱います。

:::message
今月の執筆者は以下の 5 名です。

- [Saji](https://x.com/sajikix)
  - ECMA402 (Intl) 周りのトピックを執筆
- [saku](https://x.com/sakupi01)
  - HTML, CSS に関連するトピックを執筆
- [くらっち](https://x.com/kuracchi04)
  - ECMA262 周りのトピックを執筆
- [mehm8128](https://x.com/mehm8128)
  - 主にアクセシビリティに関連するトピックを執筆
- [コサキン](https://x.com/karintou74073)
  - 主に editing に関連するトピックを執筆
:::

## HTML

### \[Editing\] Fix caret jumping to document end on ArrowDown near contenteditable=false <ruby>
https://chromium-review.googlesource.com/c/chromium/src/+/7776687

Blink では `<ruby contenteditable=false>` を含む行で下矢印を押すと、ドキュメントの末尾に飛ぶバグがありました。詳細は以下の記事を参照してください。
https://zenn.dev/cybozu_frontend/articles/ruby-wysiwyg

コサキンが IssueTracker でバグ報告をし、PR という形でコントリビュートしました。  
6/2 公開の Chrome 149 Stable に修正が適用される予定です。

### Blink: Intent to Ship: Expose the 'autocorrect' global html attribute
https://groups.google.com/a/chromium.org/g/blink-dev/c/UDOKZD6o8X0/m/dV3Hz8QkBQAJ

編集可能な要素において、綴りなどのミスの自動修正を有効化する`autocorrect`グローバル属性が、Blinkで利用可能になりました。

他の主要ブラウザでは以前から利用可能であったので、これでNewly Availableになります。

### Ship: Out of order streaming
https://groups.google.com/a/chromium.org/g/blink-dev/c/eAUPFR-sloc

JavaScript を使わずに、宣言的な構文で既存ドキュメントの一部を後から更新できるようにする機能です。`<template>` 要素の `for` 属性と、`<?start>` / `<?end>` といった XML 風のマーカーを組み合わせることで、ストリーミング中に任意の位置のコンテンツを差し替えられます。  
詳細は Chrome for Developers をご参照ください。
https://developer.chrome.com/blog/declarative-partial-updates?hl=en

この機能は Declarative Partial Updates とも呼ばれます。仕様は5月の WHATNOT で stage 3 になっており、Firefox・WebKit の Standard Position も positive です。

## CSS

### Intent to Ship: CSS Gap Decorations
https://groups.google.com/a/chromium.org/g/blink-dev/c/x6pObJjIUdg

Grid・Flexbox・multicol のアイテム間の gap に、区切り線などの装飾を付けられるようにする機能です。これまで multicol の `column-rule` でしか行えなかった gap の装飾を、Grid・Flexbox にも拡張し、さらに水平方向の gap 用に `row-rule` プロパティが追加されます。

`column-rule` / `row-rule` はそれぞれ `-width` / `-style` / `-color` のショートハンドで、`column-rule: 2px solid gray` のように `border` と似た構文で線を引くことが可能です。

Microsoft Edge チームが仕様の策定や Chromium への実装に大きく貢献してきたもので、機能の詳細は Chrome for Developers などで確認できます。

- [Gap decorations: Now available in Chromium | Chrome for Developers](https://developer.chrome.com/blog/gap-decorations-stable)
- [A new way to style gaps in CSS | Chrome for Developers](https://developer.chrome.com/blog/gap-decorations)
  

### Intent to Ship: CSS `text-fit` property
https://groups.google.com/a/chromium.org/g/blink-dev/c/55BYmN6RZtE

フォントサイズを、テキストの含まれるコンテナの幅にぴったり収まるよう自動的にスケールさせる `text-fit` プロパティの提案です。`text-fit: grow`（拡大）/ `text-fit: shrink`（縮小）のように指定して利用します。

当初の [explainer](https://github.com/explainers-by-googlers/css-fit-text/blob/main/README.md) では `text-grow` / `text-shrink` の2プロパティ案でしたが、議論を経て単一の `text-fit` プロパティにまとまりました。関連する CSSWG の issue には 110 を超えるいいねが付いており、多くの人にとっての待望の機能ではないでしょうか。仕様は [CSS Text Module Level 4](https://drafts.csswg.org/css-text-4/#text-fit-property) に定義されており、Chrome 150 で Ship される見込みです。

### Intent to Prototype: Incremental font transfer
https://groups.google.com/a/chromium.org/g/blink-dev/c/3emvPjUOtaM

`unicode-range` で subsetting せず段階的にフォントをロードできる新しい機能の提案です。

特に文字の種類が多い CJK では、 WebFont のサイズが大きくなる傾向があるため、非常に嬉しい提案になりそうです。

### Ship: Responsively-sized <iframe>
https://groups.google.com/a/chromium.org/g/blink-dev/c/zBx_uoW7jRQ

`<iframe>` を、埋め込んだドキュメントのコンテンツの高さに合わせて自動的にサイズ調整できるようにする機能です。

これまで iframe には固定サイズを指定する必要があり、コンテンツがはみ出すとスクロールバーが出たりコンテンツが切れたりして、デザインに影響が出ていました。しかし、「子のサイズに合わせて親をリサイズする」というのも、クロスサイトの情報漏洩につながり、これまで実現が難しいものでした。

今回、埋め込まれる側と埋め込む側の双方が opt-in したときに、親ドキュメント側の `<iframe>` 要素が中身のコンテンツ量に合わせてサイズ調整されます。

埋め込まれる側（子）のドキュメントは、対応を宣言する meta 要素を含めます。

```
<meta name="responsive-embedded-sizing">
```

埋め込む側（親）は、`<iframe>` 要素に新設の `frame-sizing` プロパティを指定します。

```
iframe { frame-sizing: content-height; }
```
  
レイアウトループやパフォーマンス上の問題を避けるため、サイズの確定は基本的に `load` イベント時に行われます。  
また、クロスオリジンで埋め込みを許可する場合は `Content-Security-Policy: frame-ancestors` を設定します。

Chrome 150 で Ship される予定で、`about://flags` の `responsive-iframes` で試せるとのことです。

### Ship: CSSPseudoElement support for ::backdrop, ::scroll-marker and ::view-transition
https://groups.google.com/a/chromium.org/g/blink-dev/c/CB44320ip2E

`CSSPseudoElement` の対応範囲を、これまでの `::before` / `::after` / `::marker` に加えて `::backdrop`・`::scroll-marker`・`::view-transition` に拡張するものです。

これにより、`<dialog>` の backdrop のクリック判定を疑似要素から直接扱ったり、scroll-marker のクリック判定を行ったり、View Transition で利用される `::view-transition-old` などのスナップショット結果の擬似要素を扱ったりすることが可能になります。

### CSS Spatial Layout | explainers
https://webkit.github.io/explainers/css-spatial/explainer.html

CSS に Z 軸方向（奥行き）のレイアウト概念を導入し、宣言的な 3D 配置を可能にする提案です。WebKit と Immersive Web WG/CG が中心となって議論と仕様の策定を進めています。

Immersive Web WG で Meta なども交えて議論されてきた経緯があり、5/20 の Immersive Web WG/CG の face-to-face でも取り上げられました。
https://www.w3.org/2026/05/20-immersive-web-minutes.html

## ARIA・WCAG

### Add initial draft for input method article / Add new Technique: Guarding keyboard event handlers against IME composition
https://github.com/w3c/i18n-drafts/pull/884
https://github.com/w3c/wcag/pull/5081

Enterキーで送信が発火するフォームにて、IME確定時に送信されてしまう問題（参考：[IME vs Input Field Shortcuts: Enhancing Text Input Accessibility - Slidev](https://ef81sp.github.io/ime-vs-input-keyboard-shortcut/1 "https://ef81sp.github.io/ime-vs-input-keyboard-shortcut/1")）について、[W3C i18n WGのサイト](https://www.w3.org/International/articlelist "https://www.w3.org/International/articlelist")上に掲載される記事のdraftが作成されました。

また、WCAGのSufficient Techniquesとして同様の内容を追加する提案を、mehm8128がissueとして提出し、draftのPRも作成しました。

### AX: Interactive elements containing the <svg> element which is named by <title> element doesn't have accessible name
https://github.com/WebKit/WebKit/pull/64593

[アイコンボタンのアクセシブルな名前はボタンが持つべきかアイコンが持つべきか](https://zenn.dev/moneyforward/articles/20231120-icon-button-accessible-name#3.-%3Csvg%3E%E8%A6%81%E7%B4%A0%E3%81%ABrole%3D%22img%22%E3%82%92%E4%BB%98%E4%B8%8E%E3%81%97%E3%80%81%3Csvg%3E%E8%A6%81%E7%B4%A0%E5%86%85%E3%81%AB%3Ctitle%3E%E8%A6%81%E7%B4%A0%E3%82%92%E5%85%A5%E3%82%8C%E3%81%A6%E4%BB%A3%E6%9B%BF%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B) の記事にて挙げられている、インタラクティブ要素内の`<svg>`要素が`<title>`要素を持つ場合に、WebKitではインタラクティブ要素のaccessible nameとして`<title>`要素の中身が考慮されないという問題に対する修正が入りました。

これは[yuheiyさん](https://x.com/_yuheiy)がwptの[issue](https://github.com/web-platform-tests/wpt/issues/52459)を作成したもので、mehm8128がそれに対する[PR](https://github.com/web-platform-tests/wpt/pull/56902)を作成して少し前にwptのテストケースが追加されました。

その後、mehm8128がWebKitに対してそのwptのテストケースを参照しながら[Bug](https://bugs.webkit.org/show_bug.cgi?id=309958)を報告したところ、Appleのエンジニアによって上記のPRが作成・マージされました。

これによってaccessible nameが付与されない問題は解決されますが、`<title>`要素にホバーした際にツールチップが表示されたり、`<title>`要素の中身が機械翻訳されない問題などが残っているので、必要に応じて`aria-label`や`aria-labelledby`を使うことも検討できます。

[Release Notes for Safari Technology Preview 244 | WebKit](https://webkit.org/blog/17962/release-notes-for-safari-technology-preview-244/)

参考

- [<svg>ではaltが使えないからaria-labelはロジックをすっ飛ばしている - 水底の血](https://momdo.hatenablog.jp/entry/20250510/1746858580)
- [インラインSVGの代替テキストはどうするべきか – TAKLOG](https://www.tak-dcxi.com/article/how-to-handle-alt-text-for-inline-svg/)

### Create new test type aamtest for accessibility API testing
https://github.com/web-platform-tests/wpt/pull/57696

[2月版の記事](https://zenn.dev/cybozu_frontend/articles/web_standards_trends_202602#web-platform-tests%2Finterop-accessibility)で紹介していたPRがマージされました。

これにより、AAMのテストをwptで実装できるようになります。

### ARIA-ATとACD
https://github.com/w3c/aria-at/issues/1356
https://www.w3.org/2026/05/06-aria-at-minutes.html

ACDではTPACでの議論を踏まえて、既存のwptやARIA-ATのテストケース及びテスト基盤を流用して各ブラウザ・支援技術のサポート状況をまとめようとしています。

それに伴い、ARIA-ATのミーティングで再度議論が行われました。ARIA-ATでは既にAPG以外にもARIAやHTMLの、よりアトミックなテストを追加する準備ができていることから、[既存のaria-required-text-inputのテストケース](https://github.com/w3c/aria-at/tree/master/tests/aria/aria-required-text-input)に倣って追加できるという話になっています。

mehm8128はACDにスポンサーしています。
https://opencollective.com/lolas-lab/projects/acd

### Publishing a second ARIA 1.3 Working Draft
https://github.com/w3c/aria/issues/2795

ARIA 1.3のSecond Public Working Draftの公開が検討されています。

First Public Working Draftからの主な差分として、以下の3つが挙げられています。

- [add sectionheader and sectionfooter roles](https://github.com/w3c/aria/pull/1931)
  - 以下の記事で紹介しています
  - [\`nameFrom: heading\`とsectionheader/sectionfooterについて - mehm8128のWeblog](https://portfolio.hm8128.me/blog/namefrom-heading/#sectionheader-role-%E3%81%A8-sectionfooter-role)
- [Add ariaNotify](https://github.com/w3c/aria/pull/2577)
  - 以下の記事やスライドで紹介されています
  - [命令的な ARIA ライブリージョン：ARIA Notifyの紹介 - mehm8128のWeblog](https://portfolio.hm8128.me/blog/aria-notify-introduction/)
  - [ARIA Notifyについて](https://speakerdeck.com/ryokatsuse/aria-notifynituite)
- [prevent use of aria-hidden=true on document root elements by scottaohara · Pull Request #1880 · w3c/aria](https://github.com/w3c/aria/pull/1880)
  - [Web 標準動向 2026年3月版](https://zenn.dev/cybozu_frontend/articles/web_standards_trends_202603#add-browser-accessibility-heuristics-wiki-page) で紹介したbrowser accessibility heuristicsのwikiページで紹介されている`aria-hidden`の問題が、仕様書にも記載されるようです
    

新しいARIA属性の`aria-actions`は、APGには既に[experimentalとしてページが作られてい](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/examples/tabs-actions/)ますが、仕様書にはまだ入っていないようです。

### W3C recognized on the 2026 Forbes Accessibility 200 list
https://www.w3.org/blog/2026/w3c-recognized-on-the-2026-forbes-accessibility-200-list/

W3Cが2026年のForbes Accessibility 200の1つに選出されました。

このリストは、インタビューや専門家の意見に基づき、アクセシビリティの分野で世界に大きな影響を与えた企業や個人、組織をピックアップしたものです。

[Global Accessibility Awareness Day (GAAD)](https://accessibility.day/) である5/21に、W3C CEO and PresidentであるSethがこの件に言及した記事を出しています。

日本の企業は、Sonyやトヨタを始めとした5団体が選出されています。

## JavaScript

### ECMA402
https://github.com/tc39/ecma402/blob/main/meetings/notes-2026-05-19.md

#### Stable Formatting for Stage 2?
https://github.com/orgs/tc39/projects/1

以前から特定のロケールに属さない安定的Formatのためのロケールとしてnullのようなロケールをサポートするべきだという話があり、Stable Formattingとして提案されています。

https://github.com/tc39/proposal-stable-formatting

以前の議論でロケールを`null`にすることは `undefined` との挙動の差（`undefined` はホストロケールを使用）により混乱を招くというフィードバックがあり、今回 `null` の代わりに `zxx` という文字列を定義することで合意が得られました。またこれを Intl.STABLE という定数として保持することも合意されました。

#### Intl Sequence Units for Stage 2
https://github.com/tc39/proposal-intl-sequence-units

「5フィート11インチ」のような複合単位を扱う Intl Sequence Units プロポーザルの Stage 2 進展に向けた議論が行われました。
主に以下の３点について話し合われ、それぞれ合意が得られました。
- データ構造 → 単一の数値ではなく `{ feet: 5, inch: 11 } `のようなオブジェクトバッグ形式
- 負の数が混在する場合の扱い →  `RangeError` とする

  - 「最小単位以外は整数でなければならない」という `Duration` と同様の制約を設ける

- 任意の単位組み合わせを許容するか → 許可されたカテゴリかつ「降順の大きさ」での組み合わせのみを厳格に定義する

これらの挙動が決まったこともあり最終的ににStage2への進展が承認がなされました。

####  Intl Keep Trailing Zeros for Stage 3?
https://github.com/tc39/proposal-intl-keep-trailing-zeros

フォーマットの際に数値の末尾のゼロを保持したいという提案ですが、現状Stage 3 進出を阻んでいたエッジケース（Issue #15：非常に小さい値をゼロとして描画する際の精度）の解決を待っている状態です。

https://github.com/tc39/proposal-intl-keep-trailing-zeros/issues/15

今回の議論ではIssue #15 を修正する PR のレビュー待ちであることが確認され、これが解決されれば晴れてStage 3 の準備が整うことが再確認されました。

#### ECMA-402 should stop accepting language subtags with more than 3 letters 
https://github.com/tc39/ecma402/issues/951

3文字を超える言語サブタグ（例：`posix`）の受け入れを停止すべきかという長期的な議論です。
全ページロードの 0.0021% で依然としてこれらのタグが使用されており、ウェブ互換性のリスクを完全に排除できるか慎重な判断が必要なため、今回の議論では具体的な決定は見送られました。

### ECMA262

#### Decorators to stage 2.7
https://github.com/tc39/proposals/commit/cd2396f40e8bcfc76162bcc2d3340024f236bfe0

classに対して追加機能やメタデータを付与できるようにするproposalであるDecoratorsが、Stage3からStage2.7に下がりました。

Decoratorsについての詳細はTSKaigiでの発表資料をご覧ください。
[Stage 3 Decorators でできること / できないこと / TSKaigi 2026 - Speaker Deck](https://speakerdeck.com/susisu/tskaigi-2026)

#### Amount to stage 2
https://github.com/tc39/proposal-amount

値・単位・精度をまとめた不変なオブジェクトを導入する提案です。
現状の `Intl.NumberFormat` では「データ（値・単位・精度）」「ロケール」「表示設定」が引数に混在しています。`Intl.NumberFormat` と `Intl.PluralRules` 間で精度を指定し忘れることで複数形判定がずれる（例: `"The rating is 1.0 star"` になってしまう）といったバグが起きやすい状況にあります。
`Amount` を導入し関心を分離することでこれらの問題を解決することが可能です。

```js
const appleWeight = new Amount(42.7, { unit: "kilogram" });
appleWeight.value; // 42.7 
typeof appleWeight.value; // "number"
appleWeight.toString(); // "[4.27e+1 kilogram]"
appleWeight.toLocaleString("fr"); // "42,7 kg"
```

TC39 第 114 回の MTG（2026 年 5 月）で Stage 1 から Stage 2 への昇格しました。

#### Editorial: consistently abbreviate terms used in aliases
https://github.com/tc39/ecma262/pull/3837

ECMA-262 仕様内で混在しているエイリアスの命名を統一するPRです。
変数名のレビュー観点は普段の開発に活かすことが出来ると思いました。

**短くする変数名（略記形が一般的・可読的な語）**

- `argument` → `arg`
- `constructor` → `ctor`
- `descriptor` → `desc`
- `function` → `func`
- `prototype` → `proto`


**長くする変数名（略すと曖昧になる語）**

- `cp` → `codePoint`
- `e` → `element`
- `i` → `index`
- `len` → `length`
- `m` → `ancestorModule`

## Baseline

### 📃 May 2026 release notes
https://web-platform-dx.github.io/web-features-explorer/release-notes/may-2026

## Misc

### Google I/O

Google I/O 2026 が開催されました。全体的に AI 中心の内容でしたが、ウェブ関連でも以下のような発表がありました。

- **WebMCP**: AI エージェントがウェブページ上のツールを構造化された形で呼び出し、タスクを確実に実行できるようにすものです。
  - [WebMCP  |  AI on Chrome  |  Chrome for Developers](https://developer.chrome.com/docs/ai/webmcp)
- **HTML-in-Canvas API**: WebGL / WebGPU を使う canvas 内に実 DOM を埋め込み、検索可能かつ、アクセシブルでインタラクティブな 3D 表現を可能にする機能です。
  - [Introducing the HTML-in-Canvas API origin trial  |  Blog  |  Chrome for Developers](https://developer.chrome.com/blog/html-in-canvas-origin-trial)
- **Chrome DevTools for Agents**: Chrome DevTools の機能を AI エージェント向けに拡張し、DevTools MCP 以外にもパフォーマンス・アクセシビリティ・セキュリティなどの Agent Skills、自動化操作向けの CLI が新たに搭載されています。
  - [Chrome DevTools for agents  |  Chrome for Developers](https://developer.chrome.com/docs/devtools/agents)
- **Modern Web Guidance**: 業界のエキスパートのウェブ構築のナレッジを集めた Agent Skills です。これまで AI Agent が得意としてこなかった HTML や CSS を用いた Web UI 構築に吉報となるものです。
  - [Modern Web Guidance  |  Chrome for Developers](https://developer.chrome.com/docs/modern-web-guidance)
    

CEO Sunder Pichai によるキーノートや Developer Keynote は以下でご確認ください。

- Google I/O 2026: Sundar Pichai’s opening keynote
  - [I/O 2026: Welcome to the agentic Gemini era](https://blog.google/innovation-and-ai/sundar-pichai-io-2026/)
- All the news from the Google I/O 2026 Developer keynote
  - [Google for Developers Blog - News about Web, Mobile, AI and Cloud](https://developers.googleblog.com/all-the-news-from-the-google-io-2026-developer-keynote/)
    

### State of CSS 2026
https://survey.devographics.com/en-US/survey/state-of-css/2026
年次調査 State of CSS の2026年版です。CSS の新機能の利用状況や認知度、開発者が抱える課題などを集計しており、結果はブラウザベンダーや CSSWG が機能の優先度を判断する際の参考にもなっています。

### GAAD 2026
https://accessibility.day/
5月21日（木）に、第15回 Global Accessibility Awareness Day（GAAD）が開催されました。  
今年のテーマは「Design, Develop, Deliver」です。

### W3C Japan Member Meeting and W3C in Japan 30th Anniversary Ceremony
https://www.w3.org/blog/2026/w3c-japan-member-meeting-and-w3c-in-japan-30th-anniversary-ceremony/

5/14に、慶應義塾大学三田キャンパスにて2026年 第1回 W3C日本会員会議が開催されました。

第3部では村井純 氏と弊社の青野社長による、W3C Japan 30周年特別対談が行われました。当日の様子は、後日Cybozu Inside Outにて記事が公開される予定です。
---
title: "W3C/WHATWGについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: false
---

## Web標準化の2つの潮流 - W3CとWHATWGの誕生

Web標準を策定する組織として、W3C（World Wide Web Consortium）とWHATWG（Web Hypertext Application Technology Working Group）という2つの団体が存在します。なぜ2つの組織が必要なのでしょうか。

W3Cは1994年、Webの創始者ティム・バーナーズ=リーによって設立されました。HTMLやCSS、DOMなど、Web技術全般の標準化を担当し、長年にわたってWebの発展を支えてきました。W3Cは段階的な勧告プロセス（Working Draft→Candidate Recommendation→Recommendation）を採用し、慎重に標準を策定していきます。

しかし2004年頃、W3CがXHTML 2.0という新しい方向性を推進する中で、Apple、Mozilla、Operaといったブラウザベンダーは実用的なHTML進化の必要性を感じていました。XHTML 2.0は既存のHTMLと互換性がなく、「Webを壊さない」という原則に反していたためです。こうした背景から、ブラウザベンダー主導でWHATWGが設立されました。

WHATWGの特徴は「Living Standard」という考え方です。これは、仕様を完成させて固定するのではなく、継続的に更新し続けるアプローチです。実装と仕様が常に連動し、実際のブラウザで動作する技術を重視します。2019年、HTMLとDOMの標準化権限はWHATWGに一本化され、現在のHTML仕様はWHATWGが管理しています。

## それぞれが担当する技術領域

現在、2つの組織は異なる技術領域を担当しています。

WHATWGは、ブラウザのコア機能に関わる仕様を管理しています。代表的なものがHTMLです。例えば、`<div>`や`<button>`といった要素の定義、フォームの動作、`fetch()`APIの仕様などはすべてWHATWGで標準化されています。その他にも、DOM（Document Object Model）、URL、Streams、Encodingなど、ブラウザの基盤となる技術を担当しています。

一方、W3Cは幅広い技術領域をカバーしています。CSSはW3Cが管理しており、レイアウトやデザインに関する仕様が継続的に開発されています。WebAssemblyもW3Cの管轄で、ブラウザ上で高速に動作するバイナリフォーマットの標準化を進めています。また、WAI-ARIAなどのアクセシビリティ関連の仕様や、プライバシー・セキュリティに関する技術も担当しています。

興味深いのは、両組織が協力して標準化を進めるケースもあることです。例えば、Service WorkerはWHATWGとW3Cの両方で議論されながら発展してきました。

## 開発者にとっての実践的な理解

では、実際に仕様を確認したり、最新動向をキャッチアップするにはどうすればよいでしょうか。

WHATWGの仕様は、各技術ごとに専用のURLで公開されています。
HTML仕様なら https://html.spec.whatwg.org/ で常に最新版を確認できます。
W3Cの仕様は https://www.w3.org/TR/ に一覧があり、各技術の勧告や草案を閲覧できます。

重要なのは、これらの仕様がGitHubで管理されている点です。WHATWGの仕様リポジトリ（例: https://github.com/whatwg/html ）では、issueやPRを通じて実際の議論を追うことができます。新機能の提案や、実装上の問題について、ブラウザベンダーの開発者たちがどのように議論しているかを見られるのは非常に学びになります。

最新動向のキャッチアップには、[MDN Web Docs](https://developer.mozilla.org/)が役立ちます。各ブラウザの実装状況や、標準化の進捗も確認できます。また、Chrome Platform StatusやWebKit Blogなど、各ブラウザベンダーの公式ブログも有用な情報源です。

IndexedDBのような具体的なAPIを学ぶ際も、まずMDNで概要を掴み、詳細が知りたければWHATWG仕様を参照し、実装の最新状況はGitHubで確認するという流れが効果的です。

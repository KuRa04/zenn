---
title: "webの設計哲学について"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---
## この記事の概要
- webのAPIは新しいものが出てくるけど、古いブラウザでも動作を保証する必要があるよ
- Webの設計哲学にProgressive EnhancementとGraceful degradationというものがあるよ
- 古い、新しいブラウザでも動くようにするという設計哲学だけど、アプローチが違うよ
- これらの違いを簡単に説明するよ

## 僕がWebの設計哲学に出会った背景
今月、WebAPIを毎日調べていたのですが、その中で`Polyfill`というものを知りました。
`Polyfill`は新しく登場したAPIをサポートしていない古いブラウザでその機能を利用可能にするためのコードのことを言います。こちらでも簡単にまとめているので読んでみてください。
https://zenn.dev/kura_04/articles/advent-calendar-2025-12-12
Polyfillの学習を通して新しいAPIを利用したときに古いブラウザでも動作を保証するという設計の考え方を学びました。これが私がwebの設計哲学に出会った瞬間です。設計哲学には大きく2種類あるので、次の章で説明していきます。

## 2つの設計哲学
2つの設計哲学をMDNから引用してみました。
どちらも古い・新しいブラウザで動くことを保証します。

**Progressive Enhancement**
> プログレッシブエンハンスメント (Progressive enhancement) とは、可能な限り多くのユーザーに不可欠なコンテンツと機能のベースラインを提供することを中心とした設計哲学であり、必要なすべてのコードを実行できる最新のブラウザーのユーザーに限り、最高の体験を提供します。
https://developer.mozilla.org/ja/docs/Glossary/Progressive_Enhancement

**Graceful degradation**
> グレースフルデグラデーション (上品な劣化) とは設計哲学の一つで、最新のブラウザーで動作するように新しいウェブサイトやアプリケーションを構築するものの、古いブラウザーでも、良いものでなくても基本的なコンテンツや機能を引き続き提供する使用方法で代替できるようにしようとすることを目指したものです。

最初に読んだ時、どちらも古い・新しいブラウザで動くことを説明しているのは理解出来たのですが、具体的に何が違うのががわかりませんでした。
そこで「Progressive Enhancement Graceful degradation」と検索としたところ、W3Cの記事がヒットしました。ここに分かりやすく違いが書いてあったので、次の章で説明していきます。

https://www.w3.org/wiki/Graceful_degradation_versus_progressive_enhancement

## これらのアプローチの違い
W3Cの記事によるとProgressive Enhancementは全てのブラウザで基本的なユーザー体験を出来ることを提供、同時に新しいAPIも組み込み、新しいAPIに対応しているブラウザで機能する状態にします。一方、Graceful degradationは新しいブラウザで基本的なユーザー体験を提供し、古いブラウザでは機能も劣化させて動作するようにします。
新しさを重視しているのが、「Progressive Enhancement」で、安定を重視しているのが「Graceful degradation」という理解をしました。

## まとめ
- webのAPIは新しいものが出てくるけど、古いブラウザでも動作を保証する必要があるよ
- Webの設計哲学にProgressive EnhancementとGraceful degradationというものがあるよ
- 古い、新しいブラウザでも動くようにするという設計哲学だけど、アプローチが違うよ
- 新しい機能も利用できるようにするのが、Progressive Enhancementで古いブラウザでも安定して動くようにするのがGraceful degradationだよ

## 参考
https://developer.mozilla.org/ja/docs/Glossary/Progressive_Enhancement
https://developer.mozilla.org/ja/docs/Glossary/Graceful_degradation
https://www.w3.org/wiki/Graceful_degradation_versus_progressive_enhancement
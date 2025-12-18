---
title: "React2Shell関連で新しい脆弱性など: Cybozu Frontend Weekly (2025-12-16号)"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
publication_name: "cybozu_frontend"
published: false
---

こんにちは！サイボウズ株式会社 プロダクトエンジニアの[くらっち(@Kuracchi04)](https://x.com/Kuracchi04)です。

# はじめに
サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/12/16 の Frontend Weekly で取り上げた記事や話題を紹介します。

# 取り上げた記事・話題

## Node.jsの脆弱性とセキュリティ対応したバージョンリリース
https://nodejs.org/ja/blog/vulnerability/december-2025-security-releases

Node.jsの脆弱性対応に関するリリース情報になります。
25.x、24.x、22.x、20.xのセキュリティリリースが進められており、2026/1/7にリリースされる予定です。

## Cursor Browser 向けビジュアルエディタ
https://cursor.com/ja/blog/browser-visual-editor

Cursor Browser向けのビジュアルエディタがリリースされました。
アプリのコンポーネントをドラッグ&ドロップで配置出来たり、色の設定をスライダーを使って出来たりします。

## Base UI v1
https://x.com/base_ui/status/1999154611123257522

Base UI v1の紹介です。
APGとWCAG2.2に準拠したことや、shadcn/uiが内部で使うコンポーネントライブラリにBase UIを選べるようになりました。

## ブラウザからソースコードにジャンプ出来るLocatorJS
https://www.locatorjs.com/

ブラウザで選択したソースコードをエディタで開くことが出来るLocatorJSの紹介です。
chromeの拡張機能で利用することができ、React、Vue、Svelteなど様々なフレームワークがサポートされています。

## RSCで新しい脆弱性が開示された
https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components

RSCの脆弱性が新たに2点開示されました。
悪用されるとユーザが操作できない状態（DoS）になったり、ソースコードが漏洩したりする可能性があり、深刻度の高い脆弱性となっています。

## HTML Sanitizer APIの進捗
https://groups.google.com/a/chromium.org/g/blink-dev/c/iu3VwMotMBc/m/2-LB7pDXAQAJ

HTML Sanitizer APIの標準化についての話題です。
Web開発者や一部のブラウザから支持を受けており、検討は進展しています。

## Deno 2.6がリリース
https://deno.com/blog/v2.6

Deno 2.6のリリース情報です。
npxと同等のdxコマンド追加やminimumDependencyAgeが導入されました。

# あとがき
今週はセキュリティ関連の記事が多く、特にNode.jsとRSCの脆弱性対応に注目しました。
また、効率的に開発するためのツールも紹介されていたため、セキュリティと生産性の両面を意識した回となりました。
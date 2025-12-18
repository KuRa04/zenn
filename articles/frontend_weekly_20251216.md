---
title: "Node.jsの脆弱性対応など: Cybozu Frontend Weekly (2025-12-16号)"
emoji: "🔐"
type: "idea"
topics: ["cybozufrontendweek", "frontend"]
publication_name: "cybozu_frontend"
published: true
---

こんにちは！サイボウズ株式会社 プロダクトエンジニアの[くらっち(@Kuracchi04)](https://x.com/Kuracchi04)です。

## はじめに
サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/12/16 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### Node.js: Wednesday, January 7, 2026 Security Releases
https://nodejs.org/ja/blog/vulnerability/december-2025-security-releases

Node.jsの脆弱性対応に関するリリース情報です。
25.x、24.x、22.x、20.xのセキュリティリリースが進められており、2026/1/7にリリースされる予定です。

### Cursor Browser 向けビジュアルエディタ
https://cursor.com/ja/blog/browser-visual-editor

Cursor Browser向けのビジュアルエディタがリリースされました。
アプリのコンポーネントをドラッグ&ドロップで配置出来たり、色の設定をスライダーを使って出来たりします。

### Introducing Base UI v1
https://x.com/base_ui/status/1999154611123257522

ヘッドレスなUIコンポーネントライブラリであるBase UIのv1がリリースされました。
Base UIはAPGとWCAG2.2に準拠しています。
また、shadcn/uiが内部で使うコンポーネントライブラリにBase UIを選べるようになりました。
https://x.com/shadcn/status/1999530415653113871?s=20

### LocatorJS
https://www.locatorjs.com/

ブラウザで選択したソースコードをエディタで開くことが出来るLocatorJSの紹介です。
chromeの拡張機能で利用することができ、React、Vue、Svelteなど様々なフレームワークがサポートされています。

### React: Denial of Service and Source Code Exposure in React Server Components
https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components

RSCの脆弱性が新たに2点開示されました。
悪用されるとユーザが操作できない状態（DoS）になったり、ソースコードが漏洩したりする可能性があり、深刻度の高い脆弱性となっています。

### Blink: Intent to Ship: Sanitizer API
https://groups.google.com/a/chromium.org/g/blink-dev/c/iu3VwMotMBc/m/2-LB7pDXAQAJ

HTML Sanitizer APIの標準化についての話題です。
Web開発者や一部のブラウザから支持を受けており、検討は進展しています。

### Deno 2.6: dx is the new npx
https://deno.com/blog/v2.6

Deno 2.6のリリース情報です。
npxと同等のdxコマンド追加やminimumDependencyAgeが導入されました。

## あとがき
今週はセキュリティ関連の記事が多く、特にNode.jsとRSCの脆弱性対応に注目しました。
また、効率的に開発するためのツールも紹介されていたため、セキュリティと生産性の両面を意識した回となりました。
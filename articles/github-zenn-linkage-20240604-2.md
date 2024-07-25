---
title: "githubとzennの連携方法"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["zenn", "github"]
published: false
---

# はじめに
「読み手につたわる文章 - テクニカルライティング」を読んで、アウトプットの重要性を理解しました。まずは自分の備忘録として、ローカルでzennを執筆する際に必要なことをまとめようと思います。


# 執筆に必要なコマンド

## zennのプレビュー環境を立ち上げる
- デフォルトでlocalhost:8000が立ち上がる。

```
npx zenn preview
```

## 執筆するファイルの作成

- slug は a-z0-9、-, \_の 12~50 文字の組み合わせにする必要がある。

```
npx zenn new:article --slug hogehoge
```

# 参考記事
[Zenn CLI で記事・本を管理する方法](https://zenn.dev/zenn/articles/zenn-cli-guide)
[Markdownでリンクを作る方法：外部リンクや内部リンク、ページ内リンクはどうやって作る？](https://growi.cloud/blog/3044#)
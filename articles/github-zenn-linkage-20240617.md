---
title: "PHPの暗号化、復号化メソッド"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: true
---

# はじめに
PHPで学習したことのメモ

# 利用したメソッド
- bin2hex
  - バイナリのデータを16進表現に変換
- random_bytes
  - 暗号学的にセキュアな、ランダムなバイト列を生成
- hex2bin
  - 16進エンコードされたバイナリ文字列をデコード

# 上記のメソッドを利用したコード
```
$manage_password = bin2hex(random_bytes(16));（ランダムな16進桁のバイト列を16進数に変換）
$bin_password = hex2bin($manage_password);（16進数エンコードされた文字列をデコード）
$encryption_password = $password . $bin_password;（クライアントの画面から入力されたパスワードと結合）
```
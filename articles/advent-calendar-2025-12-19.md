---
title: "File System APIについて"
emoji: "🌍️"
type: "tech"
topics: ["Web", "ブラウザ"]
published: true
---

## はじめに
12月1日から25日まで、毎日1記事ずつ公開していくアドベントカレンダー企画です。
この連載では、Web標準とDDDについて学びを深めていきます。
第19回は「`File System API`」がテーマです。
`File System API`とは何か、どのように実装するのかを紹介します。

## File System APIとは
`File System API`はデバイスのファイルシステム上のファイルにアクセスして読み取り、書き込み、ファイル管理機能を使用することが出来ます。
ファイルやディレクトリの操作はハンドルを介して行います。`FileSystemFileHandle`、`FileSystemDirectoryHandle`の子クラスが定義されており、これらを用いてファイルやディレクトリの操作を実施します。`window.showOpenFilePicker()`を用いるとファイルピッカーを表示し、ファイルへのアクセス権を得ることが出来ます。
また、`HTML Drag and Drop API`の`DataTransferItem.getAsFileSystemHandle()`や`File Handling API `でファイルハンドルへのアクセス権を得ることも可能です。
`getAsFileSystemHandle()`を用いてファイルのドラッグ&ドロップを実現している例です。

@[codepen](https://codepen.io/KuRa04-the-sans/pen/raeXGZw)


## File System APIの実装例
### 実装した画面
![](/images/advent-calendar-2025-12-19/file_upload.png)


### File System APIを利用したコード
```html
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>File Picker Example</title>
</head>
<body>
  <h1>File System Access API - ファイル選択</h1>  
  <button id="pickImage">画像を選択</button>
  
  <div id="result"></div>
</body>
<script>
  const pickImageBtn = document.getElementById('pickImage');
  const resultDiv = document.getElementById('result');

  pickImageBtn.addEventListener('click', async () => {
    try {
      const [fileHandle] = await window.showOpenFilePicker({
        types: [{
          description: '画像ファイル',
          accept: {
            'image/*': ['.png', '.jpg', '.jpeg', '.gif']
          }
        }]
      });
      const file = await fileHandle.getFile();
      resultDiv.innerText = `選択された画像ファイル: ${file.name}`;
    } catch (error) {
      resultDiv.innerText = `エラー: ${error.message}`;
    }
  });
</script>
</html>
```



## まとめ
今回は`File System API`について紹介しました。
`File System API`ではファイル書き込みやドラッグ&ドロップへの活用など様々なことが可能です。
HTMLの要素として`<input type="file">`でもファイルアップロードは出来ますが、ファイルの操作に対して機能を加える場合はこのAPIの利用を検討したいと思います。


## 参考
https://developer.mozilla.org/ja/docs/Web/API/File_System_API

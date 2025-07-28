---
title: Assembly 入門メモ
author: "新里 伊武輝"
description: "macOS で x86_64 アセンブリを書くための基本手順と注意点"
updated: 2025-07-28
---

### アセンブラのメモ

[アセンブラ変換サイト（Godbolt）](https://godbolt.org)

- スタックは下に伸びる
- `DWORD`（Double Word） = 32bit = 4バイト

## 実行方法（macOSの場合）

```bash
nasm -f macho64 <file.asm> -o <file.o>
clang <file.o> -o <executive_file> -arch x86_64
```

- $?は終了コード（プログラムがOSに返す数値、基本は0）
- global命令で外部公開
- global命令の際に_(アンダーバー)をつけないとリンカが見つけられないからmainも_mainとシンボル名にする
- *Linuxの場合は`-f elf64`を使う


## 動作確認
```bash
./<executive_file>
echo $?
```

`$?` は直前のコマンドの終了コード（= 戻り値、通常は `eax` の値）

## global命令とは？

- `global` 命令で関数やラベルを外部公開できる（リンカから参照可能にする）
- macOS では C の `main` は `_main` という名前で呼ばれるため、アセンブリでは `global _main` のように書く必要がある。

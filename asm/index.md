---
title: Assembly 入門メモ
description: "macOS で x86_64 アセンブリを書くための基本手順と注意点"
updated: 2025-07-28
---

## アセンブラのメモ

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

## clangでアセンブリ出力
Cのコードをclangでアセンブリ出力してみる

### main.c
```c:main.c
int add(int a, int b) {
	return a + b;
}

int main() {
	return add(5, 7);
}
```

### アセンブリ出力
```bash
clang -S -O0 -arch x86_64 main.c -o main.s
```

### main.s
```s:main.s
	.section	__TEXT,__text,regular,pure_instructions
	.build_version macos, 15, 0	sdk_version 15, 5
	.globl	_add                            ## -- Begin function add
	.p2align	4, 0x90
_add:                                   ## @add
	.cfi_startproc
## %bb.0:
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register %rbp
	movl	%edi, -4(%rbp)
	movl	%esi, -8(%rbp)
	movl	-4(%rbp), %eax
	addl	-8(%rbp), %eax
	popq	%rbp
	retq
	.cfi_endproc
                                        ## -- End function
	.globl	_main                           ## -- Begin function main
	.p2align	4, 0x90
_main:                                  ## @main
	.cfi_startproc
## %bb.0:
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register %rbp
	subq	$16, %rsp
	movl	$0, -4(%rbp)
	movl	$5, %edi
	movl	$7, %esi
	callq	_add
	addq	$16, %rsp
	popq	%rbp
	retq
	.cfi_endproc
                                        ## -- End function
.subsections_via_symbols
```

- -S アセンブリ出力
- -O0 最適化オプション -O0は最適化なし
- -arch アーキテクチャ指定


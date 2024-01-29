---
title: "Cデータ型モデル"
date: 2024-01-29T11:44:36+09:00
draft: false
---

C言語での開発では、標準ライブラリを用いて`uint32_t`や`int8_t`のような固定幅整数型を用いることで異なるシステム間での互換性を高めることが一般的です。しかし、時には`long int`や`unsigned int`のような実装定義型が使用されることもあります。
実装定義型は、そのサイズや表現がコンパイラや実行環境に依存するため、あるシステムでは`long int`が32ビットであり、別のシステムでは64ビットかもしれません。このような違いは、データの整合性に問題を生じさせ、移植した際にプログラムの挙動に悪影響を及ぼすことがあります。

### データ型モデル

異なる環境におけるデータ型のサイズの違いは、それぞれの環境が採用しているデータ型モデルに基づいています。
データ型モデルとは、処理系が使用するデータ型のサイズを定義するもので32ビットと64ビット環境のためのILP32、LP32、LLP64、LP64、ILP64があります。これらのモデルはプラットフォームやOSによって異なります。

#### 32ビット環境

- **ILP32**
32bit環境ではもっとも主流であり、Windows32やBSD/Linux用のGCCではもっとも多いデータ型モデル
I (int)とL(long)とP(pointer)が32bitという意味

| char | 8bit |
| --- | --- |
| short | 16bit |
| int | 32bit |
| long | 32bit |
| long long | 64bit |
| pointer | 32bit |

- **LP32**
古い32bit環境にあるが今ではほとんど見られないデータ型モデル
L(long)とP(pointer)が32bitという意味

| char | 8bit |
| --- | --- |
| short | 16bit |
| int | 16bit |
| long | 32bit |
| pointer | 32bit |

#### 64ビット環境

- **LLP64(I32LP64)**
Windowsの64ビット版で採用されているデータ型モデル
LL(long long)とP(pointer)が64bitという意味

| char | 8bit |
| --- | --- |
| short | 16bit |
| int | 32bit |
| long | 32bit |
| long long | 64bit |
| pointer | 64bit |
- **LP64(IL32P64)**
64ビット版のLinux、FreeBSD/OpenBSD等BSDなど、一般にUNIXと解釈されているOS全般で採用されているデータ型モデル
L(long)とP(pointer)が64bitという意味

| char | 8bit |
| --- | --- |
| short | 16bit |
| int | 32bit |
| long | 64bit |
| long long | 64bit |
| pointer | 64bit |

- **ILP64**
あまり使われていないデータ型モデル
SGIなどではサポートはされているそう
I(int)と L(long)とP(pointer)が64bitという意味(long long も64bitですが...)

| char | 8bit |
| --- | --- |
| short | 16bit |
| int | 64bit |
| long | 64bit |
| long long | 64bit |
| pointer | 64bit |

### データ型サイズの確認

基本的に一般的な実行環境であれば調べればすぐにサイズを知ることができると思いますが、特殊な環境ではそうもいかない場合もあります。そんな時は以下のようなコードを使用してデータ型のサイズを取得するのが手っ取り早いですね。

```c
#include <stdio.h>

int main() {
    printf("Size of char: %lu byte(s)\n", sizeof(char));
    printf("Size of short int: %lu byte(s)\n", sizeof(short int));
    printf("Size of int: %lu byte(s)\n", sizeof(int));
    printf("Size of long int: %lu byte(s)\n", sizeof(long int));
    printf("Size of long long int: %lu byte(s)\n", sizeof(long long int));
    printf("Size of float: %lu byte(s)\n", sizeof(float));
    printf("Size of double: %lu byte(s)\n", sizeof(double));
    printf("Size of long double: %lu byte(s)\n", sizeof(long double));
    printf("Size of pointer: %lu byte(s)\n", sizeof(void*));

    return 0;
}
```

### <参考文献>

[https://www.wdic.org/w/TECH/データ型モデル](https://www.wdic.org/w/TECH/%E3%83%87%E3%83%BC%E3%82%BF%E5%9E%8B%E3%83%A2%E3%83%87%E3%83%AB)

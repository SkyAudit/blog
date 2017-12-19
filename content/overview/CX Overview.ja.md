+++
title = "CXの概要"
tags = [
    "CX",
]
bounty = 40
date = "2017-09-06"
categories = [
    "Overview",
]
+++

<!-- MarkdownTOC autolink="true" bracket="round" depth="2" -->

- [CXの紹介](#cx-introduction)
- [プロジェクトのリポジトリ](#projects-repository)
- [構文](#syntax)
- [アフォーダンス](#affordances)
    - [アニティ制約](#arity-restrictions)
    - [型制約](#type-restrictions)
    - [Existential Restrictions](#existential-restrictions)
    - [Identifier Restrictions](#identifier-restrictions)
    - [Boundaries Restrictions](#boundaries-restrictions)
    - [User-defined Restrictions](#user-defined-restrictions)
- [強い型付けシステム](#strict-typing-system)
- [Compiled and Interpreted](#compiled-and-interpreted)
    - [Read-Eval-Print Loop](#read-eval-print-loop)
    - [Meta-programming Commands](#meta-programming-commands)
    - [Stepping](#stepping)
    - [Interactive Debugging](#interactive-debugging)
- [Integrated Evolutionary Algorithm](#integrated-evolutionary-algorithm)
- [直列化](#serialization)


存在する制限
識別子の制限
境界の制限
ユーザー定義の制限
コンパイルと解釈
読み取り - 評価 - 印刷ループ
メタプログラミングコマンド
ステッピング
インタラクティブなデバッグ
統合された進化的アルゴリズム


<!-- /MarkdownTOC -->

# CXの紹介

CXとは、アフォーダンスの概念に基づいた新しいプログラミングパラダイムを採用して設計された仕様とプログラミング言語の両方を指します。
アフォーダンスは、何ができて、何ができないのかをプログラムが知ることを可能にします。
たとえば、関数にどのような引数を送ることができるかをプログラムに問い合わせることができ、プログラムは可能なアクションのリストを返します。
リストからどのようなアクションが適切であるかを決定した後、選択肢の1つを選び、そのアクションををプログラムが実行します。
CXのアフォーダンスシステムの重要な要素として、遺伝的プログラミングアルゴリズムが構築され、実行時にプログラムの構造を最適化するために使用できるネイティブ関数として提供されます。

CXの仕様では、プログラマはコンパイラとインタプリタの両方にアクセス可能でなければならないと述べています。
インタプリタは、プログラマが対話的に要素をプログラムに追加したり削除したりできるREPL(Read-eval-print loop)を通じてアクセスできます。
プログラムが完成すると、そのパフォーマンスを向上させるためにコンパイルすることができます。

CXの型システムは非常に厳格です。
唯一の "暗黙のキャスト"は、パーサーが整数、浮動小数点数、ブール値、文字列、または配列を判別するときに発生します。
たとえば、関数が64ビット整数を必要とする場合、明示的に必要な型に変換するためには、キャスト関数を使用する必要があります。

最後に、CXプログラムをバイト配列に完全にシリアル化して、実行状態と構造を維持することができます。
このシリアル化されたバージョンのプログラムは、後でデシリアライズして、CXインタープリタ/コンパイラを持つ任意のデバイスでその実行を再開することができます。

以下のセクションでは、上で説明したCXの機能について詳しく説明します。

# プロジェクトのリポジトリ
プロジェクトのソースコードは、Githubリポジトリ[https://github.com/skycoin/cx](https://github.com/skycoin/cx) からダウンロードできます。
リポジトリには、仕様ファイル、ドキュメント、サンプル、およびソースコードが含まれています。

# 構文
はじめに述べたように、CXは仕様とプログラミング言語の両方を指します。
CX仕様は構文を規定するのではなく、むしろCXとみなすためにCX言語が実装しなければならない構造とプロセスを規定しています。
結果として、2つのCX言語を実装することができ、1つはLispのような構文で、もう1つはCのような構文です。
この基礎となる言語は、CX Base、すなわち「基本言語」と呼ばれています。
このドキュメントでは、実装は仕様の機能を示すために使用されていますが、その目的は学術ツールとしての機能だけでなく、一般的な目的に使用できる完全で堅牢な言語となることです。

このドキュメントで使用されているCXは、Goの構文にできるだけ似た構文を持つことを目標としています。

# アフォーダンス

プログラマは、関数が受け取るパラメータの数、戻すパラメータの数、目的の機能を得るために必要な記述、文関数にパラメータとして送る必要がある引数など、プログラムを構築する際に多くの決定をする必要があります。
CXのアフォーダンスシステムでは、要素に適用できる可能なアクションのリストを取得するための問い合わせを受けることができます。
この文脈では、要素の例は、関数、構造体、モジュール、および式です。

プログラムの背後にある論理と目的が何であるべきかを指示する一連のルールと事実を持たずに、少なくともプログラムが意味論的に正しいことを保証する基本的な制約をいくつか決めることができます。
アフォーダンスシステムは、第1のフィルタリング層として、以下で説明する制約を提供します。

### アリティの制約

CXの式は複数の値を返すことができます。
これは、式の出力引数を受け取る変数の数が、式の演算子で定義された出力の数と一致する必要があるため、アフォーダンスシステムの仕事となります。

```
out1, out2, ..., outN := op(inp1, inp2, ..., inpM)
```

上記の例が正しい場合、*op*は*N*個の引数を出力する必要があります。
*op*の定義がアフォーダンスシステム自体またはユーザーによって将来変更され得ることを考慮すると、この問題はさらに複雑になり得ます。
*op*の定義が変更されるとすぐに、*op*の出力引数を受け取る変数の数が不一致になるため、新しいアフォーダンスが*op*を演算子として使っているどんな式にも適用されます。

前のロジックはまた、受信変数の数が式の演算子の出力パラメータの数と一致する場合、新しい受信変数を追加する動作がもはや実行できないことを意味します。

アリティ制約は式の入力引数にも適用されます。
つまり、関数呼び出しのすべての入力引数がすでに定義されている場合、アフォーダンスシステムは、別の引数を可能なアクションとして追加する必要がありません。
同様に、式が必要な数よりも少ない引数を持つ演算子を呼び出そうとしている場合、アフォーダンスシステムは問い合わせを受けた時に、新しい引数を関数呼び出しに追加できることをプログラマに伝える必要があります。

**例：**

*注：文字列の連結はまだ実装されていません。
また、print関数は、表示される文字列の最後に常に改行を追加します。
このドキュメントで紹介するCX実装の将来のバージョンでは、これらの問題が解決されます。*

```
var age i32 = 18
var steps i32 = 23

func advance (direction str, numberSteps i32) () {
    printStr("Advancing:")
    printStr(direction)
    printStr("Number of steps:")
    printI32(numberSteps)
}

func main () () {
    advance("North")
}
```
上記の例では、*main*関数内の*advance*の呼び出しで引数が１つ不足しています。
アフォーダンスシステムに問い合わせると、システムは次のような行動を取ります。

```
...
(k)       AddArgument advance age
(k+1)     AddArgument advance steps
...
```

ここでkは任意のインデックスを表します。
アフォーダンスシステムは、2つのアクションが実行可能で、それらは*advance*関数に別の引数を追加することをプログラマに告げており、グローバル変数の*age*と*steps*は引数となり得る選択肢です。

アフォーダンスは常に列挙されるべきであり、アフォーダンスシステムへの問い合わせにおいてそれらの順序が一定であるべきことは重要である。
その理由は、プログラマが問い合わせの結果を調べた後に、どのアフォーダンスを適用するのかをシステムに示すことができなければならないためです。

### 型の制約

プログラミング言語における共通の挙動は、プログラマーが予期しない型の引数を関数呼び出しに送ることを制限する型システムを持つことです。
たとえ弱く型付けされたプログラミング言語であっても、次の演算`true / "hello world"`はエラーを発生させるはずです(もちろん、[難解プログラミング言語](https://ja.wikipedia.org/wiki/%E9%9B%A3%E8%A7%A3%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E8%A8%80%E8%AA%9E)の場合を除いて).
CXは非常に[強い型付けシステム](#strict-typing-system)に従っており 、予想される型ではない引数は、アフォーダンスのアクションの候補と見なすべきではありません（回避策は、アフォーダンスとして表示される前にこれらの引数をキャスト関数で囲みます）。

すでに存在する変数に新しい値を割り当てる場合は、型の制約も考慮する必要があります。
CXでは、特定の型の宣言された変数は、全てのライフタイム中で（その型がメタプログラミングのコマンド/関数を使用して削除され、新たに作成されない限り）、その型のままでなければなりません。
したがって、32ビット整数を保持すると宣言された変数は、例えば64ビット浮動小数点出力引数を受け取る候補とはみなされません。

### 存在の制約

このタイプの制約は一見すると些細なことに思えるかもしれません。"要素が存在しない場合、その要素を含むアフォーダンスは存在してはならない"
にもかかわらず、関数の名前が変更され、プログラム中の式の演算子としてすでに使用されている状況を考えれば、この制約は困難になります。
プログラムがソースコード形式であれば、この問題は単純な「検索と置換」プロセスになりますが、実行時にはアフォーダンスシステムが非常に便利になります。
"演算子にバインドされた識別子を変更するアフォーダンス"

要素の名前を変更しない場合でも、要素が存在するかどうかを判定することは簡単ではありません。
アフォーダンスで使用される要素は、コールスタックの現在のスコープ、グローバルスコープ、および他のモジュールのグローバルスコープで検索する必要があります。

### 識別子の制約

新しい名前付き要素を追加することは、一般的に、アフォーダンスの候補アクションです。
そのようなタイプのアフォーダンスを適用しようとするときに生じる制約は、再定義を避けるため新しい要素に一意の識別子を割り当てることです。
アフォーダンスシステムは、要素のスコープ内に固有の識別子を生成するか、またはプログラマに適切な識別子を提供するように要求することができます。

### 境界の制約

CX provides native functions for accessing and modifying elements from
arrays. Examples of an array reader and an array writer are:
CXは、配列内の要素にアクセスし変更するためのネイティブ関数を提供します。
配列読み出しと配列書き込みの例は次のとおりです。

```
readI32([]i32{0, 10, 20, 30}, 3)
writeF32([]f32{0.0, 10.10, 20.20}, 1, 5.5)
```

最初の式では、4つの32ビット整数の配列がインデックス3でアクセスされ、配列の最後の要素が返されます。
2番目の式では、3つの32ビット浮動小数点数の配列の2番目の要素が5.5に変更されています。
これらの配列のいずれかが、負のインデックスまたは配列の長さを超えるインデックスを使用してアクセスされた場合、「境界外」エラーが発生します。

型の制約のみに従うことによって、アフォーダンスシステムは、任意の32ビット整数引数を任意の配列にアクセスするためのインデックスとして使用できることをプログラマに通知します。
これらのプログラムはコンパイルされますが、プログラマが特別な注意を払わなければ、境界外のエラーが発生する可能性は非常に高いです。

アフォーダンスシステムは、以下の基準に従ってアフォーダンスをフィルタリングする必要があります。
負の32ビット整数を破棄し、配列読み出し関数または書き込み関数に送信される配列の長さを超える32ビット整数を破棄します。

### Uユーザー定義の制約
*注：ユーザー定義の制約システムはまだ実験段階です。*

上記の基本的な制約は、プログラムがランタイムエラーに遭遇しないことを少なくとも保証するべきです。
これらの制約は、CXの固有の[進化アルゴリズム](#integrated-evolutionary-algorithm)などの興味深いシステムを構築するのに十分なものでなければなりません。
それにもかかわらず、場合によってはより堅牢なシステムが必要とされます。
この目的のために、節、問い合わせ、およびオブジェクトは、モジュールの環境を記述するために使用されます。
これらの要素は、統合されたPrologインタプリタと、CXネイティブ関数*setClauses*、*setQuery*、および*addObject*を使用して定義されます。

この制約システムの最も一般的な説明は、追加された各オブジェクトに対して、定義されたPrologクエリを使用して問い合わせされる一連のProlog句（ファクトとルール）をプログラマが定義することです。
この説明は、初めてこれを読んだ人にとっては、ほとんど意味不明でしょう。
次の例は、概念とプロセスをもう少し明確にしてくれるはずです。

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.")

setQuery("move(robot, %s, %s, R).")
```

この例では、1つのルールしか定義されていません。
このルールは、「ロボットが北に移動したい場合はXが何であるかを尋ねる。もしXがnorthWallだった場合は移動できない」と概ね解釈できます。
クエリはアクション*move*のクエリとして機能する書式文字列です。
それは方向とオブジェクトという2つの引数を受け取る要素*robot*のためのものです。

オブジェクトは、*addObject*関数を使用して定義できます。

```
addObject("southWall")
addObject("northWall")
```

制約システムは、モジュールに存在するオブジェクトのそれぞれについてシステムに問い合わせます。
この例では、システムはまず「move（robot、north、southWall）」というクエリを実行し、システムは「nil」と応答します。
つまり、このような状況を処理するためのルールは定義されていません。
そしてデフォルトのアクションはアフォーダンスを破棄しません。
2番目のクエリは "move（robot、north、northWall）"になり、システムは "false"と応答します。
この場合、アフォーダンスはテストに合格しておらず、破棄されます。

上記の例は、これらのルールが条件を使用してアフォーダンスを否定する方法を示しています。
しかし、前のルールによって否定された後でも、ルールはアフォーダンスを受け入れるために使用できます。

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.
    move(robot, north, X, R) :- X = northWormhole, R = true.")

setQuery("move(robot, %s, %s, R).")
```

上記のコードで追加されたルールは、ワームホールが存在する場合、北に向かうロボットの動きを受け入れるようにシステムに指示します。
オブジェクト配列が前に定義した通りに残っていれば、移動アフォーダンスは破棄されますが、 `addObject("northWormhole")` が評価された場合は、「northWormhole」が追加され、ロボットはワームホールを使用して壁を通過することができます。

# 強い型付けシステム

はじめに述べたように、CXには暗黙のキャストはありません。
このため、各プリミティブ型の複数のバージョンがコアモジュールで定義されています。
たとえば、addI32、addI64、addF32、およびaddF64の4つのネイティブ関数が存在します。

パーサーは、ソースコード内で見つかったデータにデフォルトの型を付与します。
整数が読み取られた場合、デフォルトの型は*i32*または32ビット整数です。
浮動小数点が読み取られた場合、デフォルトの型は*f32*または32ビットfloatです。
パーサーが読み取る他のデータにはあいまいさはありません。
*true*と*false* は常にブーリアンです。
ダブルクォーテーションで囲まれた一連の文字は常に文字列です。
配列は `[]i64{1, 2, 3}`のように要素のリストの前にその型を示す必要があります。

プログラマがある型の値を別の型に明示的にキャストする必要がある場合、コアモジュールはプリミティブ型を扱うためにいくつかのキャスト関数を提供します。
たとえば`byteAToStr`はバイト配列を文字列にキャストし、`i32ToF32`は32ビットの整数を32ビットのfloatにキャストします。

# コンパイルと逐次実行

CX仕様では、開発者にインタプリタとコンパイラの両方を提供するためにCX言語が適用されています。
逐次実行されるプログラムは、コンパイルされたプログラムよりもはるかに遅いですが、より柔軟なプログラムが可能になります。
この柔軟性は、メタプログラミング機能、および実行時にプログラムの構造を変更するアフォーダンスによってもたらされます。

コンパイルされたプログラムは、多くの最適化処理がその堅牢性を利用するため、逐次実行プログラムよりも堅牢な構造を必要とします。
結果として、アフォーダンスシステムおよびプログラム構造上で動作する機能は、コンパイルされたプログラムではその機能が制限されます。

パフォーマンスが最大の関心事である場合はコンパイラを使用するべきですが、プログラマがCX機能によって提供されるすべての柔軟性を必要とする場合は、プログラムを逐次実行プログラムのままにしておく必要があります。
以下のサブセクションでは、これらの機能の一部をチュートリアルとして提供するのではなく、単なる紹介として提示します。

### Read-Eval-Print Loop

The read-eval-print loop (REPL) is an interactive tool where a
programmer can input new program elements and evaluate them. Starting
a new REPL session will print the following messages to the console:

```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

*
```

The "*" tells the programmer that the REPL is ready to receive a new
line of code. The REPL will keep reading input from the user until a
semicolon and a new line character are encountered.

If no program was initially loaded into the REPL, CX will start with
an empty program. This can be seen if the `:dProgram true;`
meta-programming command is given as input:


```
* :dProgram true;
Program

*
```

The REPL is only printing the word "Program" followed by an empty
line. As a first step, a new module and function can be declared:

As the first steps, a new *main* module and a new *main* function
should be declared:

```
* package main;
Program
0.- Module: main

* func main () () {};
Program
0.- Module: main
	Functions
		0.- Function: main () ()

*
```

As can be seen, the program structure is being printed every time a
new element is added to the program.

### Meta-programming Commands

`:dProgram` was used in the subsection above. Any statement that
starts with a colon (:) is part of a category of instructions known as
"meta-programming commands."

Declaring elements in the REPL instructs CX to add them to the
program's structure. But, as in many other programming languages,
these declarations are limited to only be added, and at most be
redefined.

But, as in many other programming languages that provide a REPL, the
programmer is limited to adding new elements to a program and, at
most, redefining elements. Meta-programming commands allow the
programmer to be in more control on how the program's structure is
being modified.

`:dProgram`, `:dState`, and `:dStack` are used for
debugging purposes only, by printing the program's structure, the
current call's state, and the full call stack to the user,
respectively. `:step` instructs the interpreter to go forward or
backward in its execution. `:package`, `:func`, and `:struct`, known
as *selectors*, are used to change the program's scope. `:rem` gives
the programmer access to *removers*, which can be used to selectively
remove elements from a program's structure. `:aff` is used to access
CX's affordance system; this meta-programming command is used to both
query and apply affordances for the different elements of a
program. Lastly, `:clauses` is used to set a module's clauses to be used by the
[user-defined restrictions system](#user-defined-restrictions);
`:object` and `:objects` are used for adding and printing objects,
respectively; and the last two meta-programming commands: `:query`,
which is used for setting the module's query, and `:dQuery` which is a
helper for debugging the user-defined restrictions.

### Stepping

A program started in REPL mode can be initialized with a program
structure defined in a source file. For example:
current directory

```
$ ./cx --load examples/looping.cx

```

loads `looping.cx` from the examples directory (the full list of
examples can be found in the
[project's repository](https://github.com/skycoin/cx)). Even though a
program has been loaded, it has not yet been executed. In the REPL, in
order to execute a program one has to use the meta-programming command
`:step`. To run a program until the end, `:step 0;` must be used. But
`:step` is interesting because it can take other integers as its
argument (even negative integers). For example:

```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

* :dStack false;

* :step 5;
0

* :step 5;
1

* :step 5;
2

*
```

The *examples/looping.cx* program is being run 5 steps at a time. We
can see that 5 steps are required in order for the program to
re-evaluate the *while* condition, print the counter, and add 1 to the
counter.

Likewise, we should "go back in time" if the REPL is instructed to
`:step -5`.

```
...

* :step 5;
2

* :step -5;

* :step 5;
2

*
```

After instructing CX to advance 5 steps again, the 2 is printed again
to the console. It must be noted that the counter is not just being
assigned with a different value. What is happening is that the call
stack is being reverted to a previous state.

### Interactive Debugging

A CX program will enter the REPL mode once an error has been
found. This behaviour gives the programmer the opportunity to debug
the program before attempting to resume its execution.

In the example below, a division by 0 error is raised, the REPL alerts
the programmer about the error, the last call in the call stack is
dumped, and the REPL continues its execution.


```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

* package main;

* func main () () {};

* :func main;
main
:func main {...
	* foo := divI32(5, 3);
main
:func main {...
	* bar := divI32(10, 0);
main
:func main {...
	* :step 0;
fn:main ln:0, 	locals:
>> 1
fn:main ln:1, 	locals: foo: 1

Call's State:
foo:		1

divI32() Arguments:
0: 10
1: 0

0: divI32: Division by 0
main
:func main {...
	*
```

Likewise, if a program is given as input to the CX interpreter,
without calling the REPL, but an error is raised, the REPL will be
called for the programmer or system administrator to debug the
program:

```
$ ./cx examples/program-halt.cx
1

Call's State:
nonAssign_0:		1
nonAssign_1:		1

divI32() Arguments:
0: 5
1: 0

5: divI32: Division by 0
CX REPL
More information about CX is available at https://github.com/skycoin/cx

*
```

# Integrated Evolutionary Algorithm

The affordance system and meta-programming functions in CX allow the
flexibility of changing the program's structure in a supervised
manner. However, affordances can still be automated by having a
function that selects the index of the affordance to be applied.

`evolve` is a native function that constructs user-defined functions
by using random affordances. An iterative process is used to test

`evolve` follows the principles of evolutionary computation. In
particular, evolve performs a technique called genetic
programming. Genetic programming tries to find a combination of
operators and arguments that will solve a problem. For example, you
could instruct `evolve` to find a combination of operators that, when
sent 10 as an argument, returns 20. This might sound trivial, but
genetic programming and other evolutionary algorithms can solve very
complicated problems.

In the *examples* directory from the repository, one can find an
example (*examples/evolving-a-function.cx*) that describes the process for
evolving a
[curve-fitting](https://en.wikipedia.org/wiki/Curve_fitting) function.

# Serialization

A program in CX can be partially or fully serialized to a byte
array. This serialization capability allows a program to create a
program image (similar to
[system images](#https://en.wikipedia.org/wiki/System_image)), where
the exact state at which the program was serialized is
maintained. This means that a serialized program can be deserialized,
and resume its execution later on. Serialiation can also be used to
create backups.

A CX program can leverage its integrated features to create some
interesting scenarios. For example, a program can be serialized to
create a backup of itself, and start an
[evolutionary algorithm](#integrated-evolutionary-algorithm) on one of
its functions. If the evolutionary algorithm finds a function that
performs better than the previous definition, one can keep this new
version of the program. However, if the evolutionary algorithm
performed badly, the program can be restored to the saved backup. All
of these tasks can be automated.


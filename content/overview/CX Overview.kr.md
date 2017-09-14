+++
title = "CX 개요"
tags = [
    "CX",
]
date = "2017-09-06"
categories = [
    "Overview",
]
+++

<!-- MarkdownTOC autolink="true" bracket="round" depth="1" -->

- [CX 소개](#cx-introduction)
- [프로젝트의 레파지토리](#projects-repository)
- [문법](#syntax)
- [어포던스](#affordances)
  - [Arity 제한](#arity-restrictions)
  - [ 제한](#type-restrictions)
  - [존재 제한](#existential-restrictions)
  - [식별자 제한](#identifier-restrictions)
  - [경계 제한](#boundaries-restrictions)
  - [식별된 사용자 제한](#user-defined-restrictions)
- [정해진 타이핑 시스템](#strict-typing-system)
- [컴파일 및 해석](#compiled-and-interpreted)
  - [읽기 - 평가 - 인쇄 루프](#read-eval-print-loop)
  - [메타 프로그래밍 명령](#meta-programming-commands)
  - [스테핑](#stepping)
  - [대화식 디버깅](#interactive-debugging)
- [통합 진화 알고리즘](#integrated-evolutionary-algorithm)
- [직렬화](#serialization)

<!-- /MarkdownTOC -->

# CX 소개

CX는 어포던스 이론에 기반하여 새로운 프로그래밍 패러다임을 수용하도록 
설계된 설명서 및 프로그래밍 언어입니다.
어포던스는 프로그램이 어떤 어떤 작업을 할 수 있는지, 없는지를 알려줍니다.
예를 들면, 우리는 프로그램에 어떤 인수를 함수에 보낼 수 있는지를 물어볼 수 있으며, 
프로그램은 가능한 실행 목록을 반환합니다. 
목록에서 어떤 작업이 적절할 지 결정 후, 우리는 옵션 중 하나를 선택할 수 있고, 
프로그램은 선택한 작업을 적용할 것입니다. 
CX의 어포던스 시스템은 유전 프로그래밍 알고리즘이 구축되었으며 기본 함수로 제공되고, 
이것은 런타임 도중 프로그램의 구조를 최적화 하는데 이용될 수 있습니다.

CX 사양은 컴파일러와 인터프리터 모두 프로그래머가 액세스 할 수 있어야 한다고 명시합니다.
인터프리터는 읽기-평가-인쇄(read-eval-print) 루프를 통해 접근할 수 있으며, 
어디서든지 프로그래머가 요소를 대화식으로 추가 또는 제거할 수 있습니다.
일단 프로그램이 끝나면, 성능을 높이기 위해 그것을 컴파일 할 수 있습니다.


CX의 타이핑 시스템은 매우 엄격합니다. 유일한 "암시적 형변환"은 파서가 
무엇이 integer, float, boolean, string, 또는 배열(array)형인지 결정할 때 
발생된다. 만약 함수가 64비트 integer형을 필요로 하는 경우, 예를 들면, 
명시적으로 요구 타입으로 변환하기 위해 형변환 함수를 사용해야 합니다.

마지막으로, CX 프로그램은 실행 상태와 구조를 유지하면서 바이트 배열로 
완전히 직렬화 될 수 있습니다. 이 직렬화 된 버전의 프로그램은 나중에 비 직렬화 될 수 있으며, 
CX 인터프리터/컴파일러가 있는 모든 장치에서 해당 프로그램의 실행을 계속할 수 있습니다.

다음 섹션에서는, 위 단락에서 설명한 CX 기능에 대해 자세히 설명합니다.

# 프로젝트 레파지토리

프로젝트 소스코드는 Github 레파지토리에서 다운로드할 수 있습니다.:
[https://github.com/skycoin/cx](https://github.com/skycoin/cx). 
이 레파지토리에는 명세 파일, 문서, 예제 및 소스코드 자체가 포함됩니다.

# 문법

소개에서 언급했듯이 CX는 명세 및 프로그래밍 언어입니다. 
CX 명세는 문법을 강요하지는 않지만, CX로 간주되기 위해서는 
CX 전용언어를 통해 반드시 구조와 프로세스가 실행되어야 한다.
결과적으로 이것은 두 가지의 CX 전용언어를 사용할 수 있습니다. 
하나는 Lisp과 유사한 문법이며, 다른 하나는 C와 유사한 문법입니다. 
이 기반 언어는 CX 기반 또는 "기본언어"라고 합니다. 이 문서에서, 
구현은 사양을 보여주기 위해 사용되지만, 
이것의 목적은 학술도구만으로 사용되는 것이 아니라, 
일반적인 목적으로 사용될 수 있는 완성된 언어가 되는 것입니다.

이 문서에서 사용된 CX는 가능한 한 Go 문법과 유사한 문법을 사용하는 목표를 가지고 있습니다.

# 어포던스

프로그래머는 프로그램을 구성하는 동안 많은 결정을 내려야 합니다.
예를 들면, 함수가 얼마나 많은 매개변수를 받아야 하는지, 
얼마나 많은 파라미터가 반드시 반환되야 하는지, 원하는 기능을 얻기 위해 
어떤 명령문을 하용해야 하며, 무엇을 통해야 하는지 등이 말입니다. 
CX 내 어포던스 시스템을 쿼리하여 요소에 적용할 수 있는 가능한 작업 목록을 얻을 수 있습니다.
이 구문에서, 요소의 예는 함수, 구조, 모듈 그리고 표현식이 있습니다.

프로그램의 논리와 목적이 무엇이 되어야 하는지를 
규정하는 일련의 규칙과 사실이 없어도 최소한 프로그램의 의미 상 
정확성을 보장하는 몇 가지 기본 제한을 결정할 수 있습니다.
어포던스 시스템은 첫 번째 필터링 레이어와 같은 제한사항을 제공하며 
아래에서 설명하겠습니다.

### 제한 사항

CX의 표현식은 여러 값을 반환할 수 있습니다. 
이는 표현식의 출력 인수를 받는 변수의 수는 
표현식의 연산자에 의해 정의된 출력의 수와 일치해야 하기 때문에
어포던스 시스템에 대한 문제점을 발생시킵니다.

```
out1, out2, ..., outN := op(inp1, inp2, ..., inpM)
```

만약 위의 예제가 올바르다면, *op*는 *N* 인수를 출력해야 합니다. 
이 문제는 더욱 큰 문제가 될 수 있는데, 만약 우리가 알고 있는 *op*의 정의가 
어포던스 시스템 자체나 사용자에 의해 나중에 바뀔 수 있고 : *op*의 정의가 
바뀌자마자, 새로운 어포던스는 오퍼레이터로써 *op*를 사용하여 어떤 구문이라도 
접근할 수 있습니다. 왜냐하면 *op*의 출력 인수를 받은 많은 수의 변수는 이제 일치하지 않기 때문입니다. 

이전 논리는 또한 수식 변수의 개수와 수식 연산자의 출력 매개 변수 개수가 일치하면, 
더 이상 새로운 수식 변수를 추가하는 작업을 수행할 수 없다는 것을 의미합니다.

Arity 제한은 표현식의 입력 인수에도 적용됩니다. 예를 들어, 
함수 호출에 이미 입력 인수가 모두 정의되어 있는 경우, 
어포던스 시스템은 가능한 행위에 대한 다른 인수를 리스트에 추가하지 않아야 합니다.
마찬가지로, 표현식이 필요한 것보다 적은 수의 인수로 연산자를 호출하려고하는 경우, 
어포던스 시스템은 조회할 때, 함수 호출에 새로운 인수를 추가할 수 있다는 것을 
프로그래머에게 알려야합니다.

**예제:**

*공지 : 문자열 연결은 아직 구현되지 않았습니다. 
또한 인쇄 함수는 항상 인쇄 중인 문자열의 끝에 새로운 행을 추가합니다. 
이 문서에 제시된 CX 구현의 향후 버전에서는 이러한 문제를 해결할 것입니다..*

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

위 예제에서, *main*기능의 *advance*를 호출하는 것은 하나의 인자가 누락되었습니다.
만약 어포던스 시스템에 질문을 던지면, 그 시스템은 다른 것들과 마찬가지로 다음과 유사한 행동을 취해야만 합니다.:

```
...
(k)       AddArgument advance age
(k+1)     AddArgument advance steps
...
```

여기서 k는 임의의 색인을 나타냅니다. 
위에서 볼 수 있듯이, 어포던스 시스템은 프로그래머에게 두 가지 동작이 가능하다는 것을 알려주는데, 
이전 기능에 다른 인수를 추가할 수 있고, 전역 정의 "age"와 "steps"는 인수로 작용하는 옵션 중 
하나로 동작할 수 있습니다.

어포던스는 항상 열거되어야 한다는 것은 주목할 만한 일이며, 
그들의 요청은 어포던스 시스템으로 여러차례 호출을 보냅니다.
그 이유는 프로그래머가 쿼리 결과를 검토한 후에 어떤 어포던스가 
적용될 것인지를 시스템에 알려주어야 하기 때문입니다.

### 타입 제한

프로그래밍 언어의 일반적인 동작은 프로그래머가 예기치 않은 유형의 
인수를 함수 호출로 보내는 것을 제한하는 입력 시스템을 가지는 것입니다.
취약한 타입의 프로그래밍 언어에서조차도 `true / "hello world"` 의 경우에 에러를 발생시킬 것입니다.
(물론 
[난해한 언어](https://en.wikipedia.org/wiki/Esoteric_programming_language),가 아닌경우). 
CX는 매우
[엄격한 타이핑 시스템](#strict-typing-system), 을 따르고 예상되는 유형과 
정확히 일치하지 않는 인수는 어포던스의 동작 후보군으로 간주될 수 없습니다. 
(해결책은 어포던스로 표시되기 전에 변환함수 내의 인수들을 묶는 것이지만)

이미 존재하는 변수에 새로운 값을 할당할 때는 타입 제한을 고려해야합니다. 
CX에서 특정 유형으로 선언된 변수는 모든 수명주기 기간동안 반드시 그 타입으로 남아있어야 합니다.
(메타프로그래밍 명령/함수를 통해 제거되거나 새로 작성된 경우를 제외하고)
따라서 32 비트 정수(integer)를 보유하도록 선언된 변수는, 
64 비트 부동 소수점 출력 인수(float)를 받을 수 없습니다.

### 존재 제한

이러한 유형의 제한은 처음에는 사소한 것처럼 보일 수 있습니다.: 
요소가 존재하지 않는다면 요소가 포함 된 어포던스도 마찬가지로 존재해서는 안됩니다.
그럼에도 불구하고 이 제한은 우리가 함수의 이름을 변경하는 것을 
고려하는 상황이 될 때 문제가 됩니다.
이 함수는 이미 프로그램 전체에서 표현식의 연산자로 사용되었습니다. 
프로그램이 소스 코드 형식인 경우, 이 문제는 간단한 "검색 및 바꾸기"프로세스로 
축소되지만, 런타임 중이라면, 어포던스 시스템이 매우 유용해집니다.:
어포던스는 이 연산자에 바인딩 된 식별자를 변경할 수 있습니다.

요소의 이름을 변경하지 않는다고 할지라도, 
요소가 존재하는지 여부를 알아내는 것은 간단하지 않습니다. 
어포던스에서 사용할 요소는 호출 스택의 현재 범위, 
전역 범위 및 다른 모듈의 전역 범위에서 검색해야합니다.

### 식별자 제한 사항

새롭게 명명된 요소를 추가하는 것은 일반적으로 어포던스에 대한 예비 작업입니다. 
이러한 유형의 어포던스를 적용하려고 할 때 발생하는 제한은 재 정의를 피하기 위해, 
새 요소에 대한 고유한 식별자를 보장하는 것입니다.
어포던스 시스템은 요소의 범위에서 고유 식별자를 생성하거나, 
프로그래머에게 적합한 식별자를 제공하도록 요청할 수 있습니다.

### 경계 제한

CX는 배열 요소에 접근 및 수정할 수 있는 기본 함수를 제공합니다. 
배열 판독기와 배열 작성기의 예는 다음과 같습니다.:

```
readI32([]i32{0, 10, 20, 30}, 3)
writeF32([]f32{0.0, 10.10, 20.20}, 1, 5.5)
```

In the first expression, an array of four 32 bit integers is accessed
at index 3, which returns the last element of the array. In the second
expression, the second element of an array of three 32 bit floats is
changed to 5.5. If any of these arrays was accessed using either a
negative index or an index which exceeds the length of the array, an
"out of boundaries" error is raised.

By obeying only the type restrictions, the affordance system will tell
the programmer that any 32 bit integer argument can be used as an
index to access any array. Although these programs would compile, out
of boundaries errors are very probable to occur if the programmer does not
pay extra attention to what is being chosen to be applied.

The affordance system needs to filter the affordances according to the
following criteria: discard any negative 32 bit integer, and discard
any 32 bit integer which exceeds the length of the array being sent to
the array reader or writer.

### User-defined Restrictions
*Note: The user-defined restrictions system is still in its
 experimental stages.*

The basic restrictions described above should at least guarantee that
the program does not encounter any runtime errors. These restrictions
should be enough to build some interesting systems, such as CX's
native
[evolutionary algorithm](#integrated-evolutionary-algorithm). Nevertheless,
in some situations a more robust system is required. For this purpose,
clauses, queries and objects are used to describe a module's
environment. These elements are defined by using an integrated Prolog
interpreter, and the CX native functions *setClauses*, *setQuery*, and
*addObject*.

The most general description of this restriction system is that the
programmer defines a series of Prolog clauses (facts and rules), that
will be queried using the defined Prolog query, for each of the
objects added. This will hardly make any sense to anyone reading this
for the first time. An example should clarify the concepts and the
process a bit more:

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.")

setQuery("move(robot, %s, %s, R).")
```

In this example, only one rule is defined. The rule can roughly be
interpreted as "if the robot wants to move north, ask what is X. If X
is northWall, then it can't move." The query is just a format string
that will serve as a query for the *move* action, and for the *robot*
element that will receive two more arguments: a direction, and an
object.

Objects can be defined by using the *addObject* function:

```
addObject("southWall")
addObject("northWall")
```

The restriction system will query the system for each of the objects
present in the module. In this example, the system will first perform
the query "move(robot, north, southWall)," and the system will respond
"nil," which means that it does not have any rule defined to handle
such situation, and the default action is to not discard the
affordance. The second query will be "move(robot, north, northWall),"
and the system will respond "false." In this case, the affordance did
not pass the test, and is discarded.

The example above illustrates how these rules can negate an
affordance using a condition. But rules can also be used to accept
affordances, even after being negated by previous rules.

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.
    move(robot, north, X, R) :- X = northWormhole, R = true.")

setQuery("move(robot, %s, %s, R).")
```

The added rule in the code above tells the system to accept the robot
movement towards north if a wormhole is present. If the object array
is left as it was defined before, the movement affordance will still
be discarded, but if `addObject("northWormhole")` is evaluated, the
"northWormhole" will be added and the robot will be able to pass
through the wall using the wormhole.

# Strict Typing System

As mentioned in the introduction, there is no implicit casting in
CX. Because of this, multiple versions for each of the primitive types
are defined in the core module. For example, four native functions for
addition exist: addI32, addI64, addF32, and addF64.

The parser attaches a default type to data it finds
in the source code: if an integer is read, its default type is *i32*
or 32 bit integer; and if a float is read, its default type is *f32* or 32
bit float. There is no ambiguity with other data read by the parser:
*true* and *false* are always booleans; a series of characters
enclosed between double quotes are always strings; and array needs to
indicate its type before the list of its elements, e.g., `[]i64{1, 2,
3}`.

For the cases where the programmer needs to explicitly cast a value of
one type to another, the core module provides a number of cast
functions to work with primitive types. For example, `byteAToStr`
casts a byte array to a string, and `i32ToF32` casts a 32 bit integer
to a 32 bit float.

# Compiled and Interpreted

The CX specification enforces a CX dialect to provide the developer
with both an interpreter and a compiler. An interpreted program is far
slower than its compiled counterpart, as is expected, but will allow a
more flexible program. This flexibility comes from meta-programming
functions, and affordances, which can modify a program's structure
during runtime.

A compiled program needs a more rigid structure than an interpreted
program, as many of the optimizations leverage this rigidity. As a
consequence, the affordance system and any function that operates over
the program's structure will be limited in functionality in a compiled
program.

The compiler should be used when performance is the biggest concern,
while a program should remain being interpreted when the programmer
requires all the flexibility provided by the CX features. In the
following subsections, some of these features are presented, without
the aim of serving as a tutorial, but rather as a mere introduction. 

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

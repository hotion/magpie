# Monkey程序语言

## 主页

[monkey](https://github.com/haifenghuang/monkey)

## 概述

Monkey是一个用go语言写的解析器. 语法借鉴了C, Ruby, Python和Perl.

## 总览

此项目是基于mayoms的项目 [monkey](https://github.com/mayoms/monkey)，修改了其中的一些bug，同时增加了许多语言特性：

* 更改了`string`模块(能够正确处理utf8字符编码)
* 修改了`file`模块(包含一些新方法).
* 增加了`math`模块
* 增加了`sql(db)`模块(能够正确的处理`null`值)
* 增加了`time`模块
* 增加了`sort`模块
* 增加了`os`模块
* 增加了`log`模块
* 增加了`net`模块
* 增加了`http`模块
* 增加了`filepath`模块
* 增加了`flag`模块(用来处理命令行参数)
* 增加了`json`模块(json序列化和反序列化)
* 增加了`fmt`模块
* 增加了`sync`模块
* 增加了`list`模块
* 增加了`linq`模块(代码来自[linq](https://github.com/ahmetb/go-linq)并进行了相应的更改)
* 增加了`csv`模块
* 增加了`template`模块
* 正则表达式支持(部分类似于perl)
* 管道(channel)(基于go语言的channel)
* 更多的操作符支持(&&, ||, &, |, ^, +=, -=, ?: 等等)
* utf8支持(例如，你可以使用utf8字符作为变量名)
* 更多的流程控制支持(例如: try/catch/finally, for-in, case-in, 类似c语言的for循环)
* defer支持
* spawn支持(goroutine)
* enum支持和
* pipe操作符支持
* 支持可变参数和缺省参数的函数

这个项目的目的主要有以下几点：
* 自学go语言
* 了解解析器的工作原理

但是，解析器的速度并不是这个项目考虑的因素

## 安装

下载本项目，运行`./run.sh`

## 基本用法

你可以如下方式使用REPL:

```sh
~ » monkey
Monkey programming language REPL

>>
```

或者运行一个monkey文件:

```sh
monkey path/to/file
```

## 语言之旅

### 注释

Monkey仅支持单行注释.

```swift
// an inline comment
# another inline comment
```

### 数据类型

Monkey支持7种基本类型: `String`, `Int`, `Float`, `Bool`, `Array`, `Hash` 和`Nil`

```swift
s1 = "hello, 黄"      # strings are UTF-8 encoded
s2 = `hello, "world"`  # raw string
i = 10                 # int
f = 10.0               # float
b = true               # bool
a = [1, "2"]           # array
h = { "a"=>1, "b"=>2}  # hash
n = nil
```

### 常量(字面值)

Monkey中，主要有9种类型的常量(字面量).

* Integer
* Float
* String
* 正则表达式
* Array
* Hash
* Nil
* Boolean
* Function

```swift
// Integer literals
i1 = 10
i2 = 20_000_000
i3 = 0x80           // hex
i4 = 0b10101        // binary
i5 = 0c127          // octal

// Float literals
f1 = 10.25
f2 = 1.02E3
f3 = 123_456.789_012

// String literals
s1 = "123"
s2 = "Hello world"

// Regular expression literals
r = /\d+/.match("12")
if (r) { prinln("regex matched!") }

// Array literals
a = [1+2, 3, 4, "5", 3]

// Hash literals
h = { "a"=>1, "b"=>2, "c"=>2}

// Nil literal
n = nil

// Boolean literals
t = true
f = false

// Function literals
let f = add(x, y) { return a + b }
println(f(1,2))
```

### 变量

你可以使用`let`来声明一个变量，或直接使用赋值的方式来声明并赋值一个变量:`variable=value`.

```swift
let a = 1, b = "hello world", c = [1,2,3]
d = 4
e = 5
姓="黄"
```

## 保留字

下面列出了monkey语言的保留字：

* fn
* let
* true false nil
* if elsif elseif else
* return
* include
* and or
* enum
* struct # 暂时没使用
* do while for break continue where
* grep map
* case is in
* try catch finally throw
* defer
* spawn
* yield #not used
* qw

### 类型转换

你可以使用内置的方法：`int()`, `float()`, `str()`, `array()`来进行不同类型之间的转换.

```swift
let i = 0xa
let s = str(i)     // result: "10"
let f = float(i)   // result: 10
let a = array(i)   // result: [10]
```

## `qw`(Quote word)关键字

`qw`关键字类似perl的`qw`关键字. 当你想使用很多的双引号字符串时,`qw`就是一个好帮手.

```swift
for str in qw<abc, def, ghi, jkl, mno> { //允许的成对操作符：'{}', '<>', '()'
  println('str={str}')
}

newArr = qw(1,2,3.5) //注：这里的newArr是一个字符串数组，不是一个整形数组.
fmt.printf("newArr=%v\n", newArr)
```

## `enum`关键字

在mokey中，你可以使用`enum`来定义常量.

```swift
LogOption = enum {
    Ldate         = 1 << 0,
    Ltime         = 1 << 1,
    Lmicroseconds = 1 << 2,
    Llongfile     = 1 << 3,
    Lshortfile    = 1 << 4,
    LUTC          = 1 << 5,
    LstdFlags     = 1 << 4 | 1 << 5
}

opt = LogOption.LstdFlags
println(opt)

//得到`enum`的所有名称
for s in LogOption.getNames() { //非排序(non-ordered)
    println(s)
}

//得到`enum`的所有值
for s in LogOption.getValues() { //非排序(non-ordered)
    println(s)
}

// 得到`enum`的一个特定的名字
println(LogOption.getName(LogOption.Lshortfile))
```

### 控制流程

* If-else
* for-in
* while
* do
* try-catch-finally
* case-in/case-is

```swift
// if-else
let a= 10, b = 5
if (a > b) { // '()'可选, 但是'{}'必须要有
    println("a > b")
}
elseif a == b { // 也可以使用'elsif'
    println("a = b")
}
else {
    println("a < b")
}

// for
i = 9
for { // 无限循环
    i = i + 2
    if (i > 20) { break }
    println('i = {i}')
}


i = 0
for (i = 0; i < 5; i++) {  // 类似c语言的for循环， '()'必须要有
    if (i > 4) { break }
    if (i == 2) { continue }
    println('i is {i}')
}


for i in range(10) {
    println('i = {i}')
}

a = [1,2,3,4]
for i in a where i % 2 != 0 {
    println(i)
}


hs = {"a"=>1, "b"=>2, "c"=>3, "d"=>4, "e"=>5, "f"=>6, "g"=>7}
for k, v in hs where v % 2 == 0 {
    println('{k} : {v}')
}


for i in 1..5 {
    println('i={i}')
}

for item in 10..20 where $_ % 2 == 0 { // $_ is the index
    printf("idx=%d, item=%d\n", $_, item)
}


for c in "m".."a" {
    println('c={c}')
}


for idx, v in "abcd" {
    printf("idx=%d, v=%s\n", idx, v)
}


for idx, v in ["a", "b", "c", "d"] {
    printf("idx=%d, v=%s\n", idx, v)
}

for item in ["a", "b", "c", "d"] where $_ % 2 == 0 { // $_ 是索引
    printf("idx=%d, item=%s\n", $_, v)
}


//for循环是个表达式(expression),而不是一个语句(statement), 因此它能够被赋值给一个变量
let plus_one = for i in [1,2,3,4] { i + 1 }
fmt.println(plus_one)

// while
i = 10
while (i>3) {
    i--
    println('i={i}')
}

// do
i = 10
do {
    i--
    if (i==3) { break }
}

// try-catch-finally(仅支持throw一个string类型的变量)
let exceptStr = "SUMERROR"
try {
    let th = 1 + 2
    if (th == 3) { throw exceptStr }
}
catch "OTHERERROR" {
    println("Catched OTHERERROR")
}
catch exceptStr {
    println("Catched is SUMERROR")
}
catch {
    println("Catched ALL")
}
finally {
    println("finally running")
}

// case-in/case-is
let testStr = "123"
case testStr in { // in(完全或部分匹配), is(完全匹配)
    "abc", "mno" { println("testStr is 'abc' or 'mno'") }
    "def"        { println("testStr is 'def'") }
    `\d+`        { println("testStr contains digit") }
    else         { println("testStr not matched") }
}

let i = [{"a"=>1, "b"=>2}, 10]
let x = [{"a"=>1, "b"=>2},10]
case i in {
	1, 2 { println("i matched 1, 2") }
	3    { println("i matched 3") }
	x    { println("i matched x") }
	else { println("i not matched anything")}
}

```

## 数组(Array)

在Monkey中, 你可以使用[]来初始化一个空的数组:

```swift
emptyArr = []
emptyArr[3] = 3 //将会自动扩容
println(emptyArr)
```

数组可以包含任意数据类型的元素。

```swift
mixedArr = [1, 2.5, "Hello", ["Another", "Array"], {"Name"=>"HHF", "SEX"=>"Male"}]
```

你可以使用索引来访问数组元素。

```swift
println('mixedArr[2]={mixedArr[2]}')
println(["a", "b", "c", "d"][2])
```

因为数组是一个对象, 因此你可以使用对象方法来操作它。

```swift
if ([].empty()) {
    println("array is empty")
}

emptyArr.push("Hello")
println(emptyArr)

//你可以使用'加算(+=)'的方式来向数组中添加一个元素:
emptyArr += 2
println(emptyArr)
```

可以使用`for`循环来遍历一个数组。

```swift
numArr = [1,3,5,2,4,6,7,8,9]
for item in numArr where item % 2 == 0 {
    println(item)
}

let strArr = ["1","a5","5", "5b","4","cc", "7", "dd", "9"]
for item in strArr where /^\d+/.match(item) {
    println(item)
}

for item in ["a", "b", "c", "d"] where $_ % 2 == 0 {  //$_是索引
    printf("idx=%d, v=%s\n", $_, item)
}
```

你可以使用内置函数`reverse`来反转数组元素:

```swift
let arr = [1,3,5,2,4,6,7,8,9]
println("Source Array =", arr)

revArr = reverse(arr)
println("Reverse Array =", revArr)
```

## 字符串(String)

在monkey中, 有三种类型的`string`:

* 原生字符串(可包含`\n`)
* 双引号字符串(不可包含`\n`)
* 单引号字符串(可解析字符串)

原生字符串是一系列字符序列。使用反引号(``)来表示. 在原生字符串中，除了不能使用反引号外，你可以使用其它的任意字符。

请看下面的例子:

```swift
normalStr = "Hello " + "world!"
println(normalStr)

println("123456"[2])

rawStr = `Welcome to
visit us!`
println(rawStr)

//当你希望一个变量在字符串中也能够被解析时，你可以使用单引号。
//需要被解析的字符串放在花括号('{}')中:
str = "Hello world"
println('str={str}') //输出: "Hello world"
str[6]="W"
println('str={str}') //输出: "Hello World"

```

在monkey中, 字符串是utf8编码的, 这说明你可以使用utf8编码的字符作为变量名：

```swift
三 = 3
五 = 5
println(三 + 五) //输出 : 8
```

字符串也是对象，你可以使用`strings`模块中的方法来操作字符串：

```swift
upperStr = "hello world".upper()
println(upperStr) //输出 : HELLO WORLD
```

字符串也可以被遍历:

```swift
for idx, v in "abcd" {
    printf("idx=%d, v=%s\n", idx, v)
}

for v in "Hello World" {
    printf("idx=%d, v=%s\n", $_, v) //$_是索引
}
```

你可以连接一个对象到字符串:

```swift
joinedStr = "Hello " + "World"
joinedStr += "!"
println(joinedStr)
```

你还可以使用内置函数`reverse`来反转字符串：

```swift
let str = "Hello world!"
println("Source Str =", str)
revStr = reverse(str)
println("Reverse str =", revStr)
```


## 哈希(Hash)

在monkey中, 使用{}来创建一个空的哈希:

```swift
emptyHash = {}
emptyHash["key1"] = "value1"
println(emptyHash)
```

哈希的键(key)可以是字符串(string)，整型(int)或布尔型(boolean):

```swift
hashObj = {
    12     => "twelve",
    true   => 1,
    "Name" => "HHF"
}
println(hashObj)
```

你还可以使用'+'或'-'来从一个哈希中增加或者删除一个元素:

```swift
hashObj += {"key1" => "value1"}
hashObj += {"key2" => "value2"}
hashObj += {5 => "five"}
hashObj -= "key2"
hashObj -= 5
println(hash)
```

哈希也是一个对象,你可以使用`hash`模块中的方法来操作哈希:

```swift

hashObj.push(15, "fifteen") //第一个参数是键，第二个参数是值
hashObj.pop(15)

keys = hashObj.keys()
println(keys)

values = hashObj.values()
println(values)
```

你还可以使用内置函数`reverse`来反转哈希的key和value:

```swift
let hs = {"key1"=>12, "key2"=>"HHF", "key3"=>false}
println("Source Hash =", hs)
revHash = reverse(hs)
println("Reverse Hash =", revHash)
```

## 标准输入/输出/错误

Monkey中预定义了下面三个对象: `stdin`, `stdout`, `stderr`。分别代表标准输入，标准输出，标准错误

```swift
fmt.fprintf(stdout, "Hello world\n")

print("Please type your name:")
name = stdin.read(1024)  //从标准输入读最多1024字节
println("Your name is " + name)
```

## 标准库中的错误处理

当标准库中的函数返回`nil`或者`false`的时候，你可以使用它们的`message()`方法类获取错误信息:

```swift
file = newFile(filename, "r")
if (file == nil) {
    println("opening ", filename, "for reading failed, error:", file.message())
}


let ret = http.listenAndServe("127.0.0.1:9090")
if (ret == false) {
    println("listenAndServe failed, error:", ret.message())
}

```

也许你会觉得奇怪，为什么`nil`或`false`有`message()`方法？ 因为在monkey中, `nil`和`false`两个都是对象，因此它们都有方法。

## 关于`defer`关键字

`defer`语句推迟(defer)某个函数的执行直到函数返回。

```swift
let add  =  fn(x,y){
    defer println("I'm defer1")
    println("I'm in add")
    defer println("I'm defer2")
    return x + y
}
println(add(2,2))
```

结果如下:

```sh
I'm in add
I'm defer2
I'm defer1
4
```

## 不同类型的联接

Monkey中，你可以联接不同的类型。请看下面的例子:

```swift
// Number plus assignment
num = 10
num += 10 + 15.6
num += 20
println(num)

// String plus assignment
str = "Hello "
str += "world! "
str += [1, 2, 3]
println(str)

// Array plus assignment
arr = []
arr += 1
arr += 10.5
arr += [1, 2, 3]
arr += {"key"=>"value"}
println(arr)

// Array compare
arr1 = [1, 10.5, [1, 2, 3], {"key" => "value"}]
println(arr1)
if arr == arr1 { //support ARRAY compare
    println("arr1 = arr")
} else {
    println("arr1 != arr")
}

// Hash assignment("+=", "-=")
hash = {}
hash += {"key1" => "value1"}
hash += {"key2" => "value2"}
hash += {5 => "five"}
println(hash)
hash -= "key2"
hash -= 5
println(hash)
```

## Grep和map

`grep`和`map`类似于perl的`grep`和`map`.

```swift
let sourceArr = [2,4,6,8,10,12]

//$_表示每次循环取得的值
let m = grep  $_ > 5, sourceArr //对每一个sourceArr中的元素，仅返回">5"的元素
println('m is {m}')

let cp = map $_ * 2 , sourceArr //将每个元素乘以2
println('cp is {cp}')

//一个复杂一点的例子
let fields = {
                "animal"   => "dog",
                "building" => "house",
                "colour"   => "red",
                "fruit"    => "apple"
             }
let pattern = `animal|fruit`
// =~(匹配), !~(不匹配)
let values = map { fields[$_] } grep { $_ =~ pattern } fields.keys()
println(values)
```

## 函数

在Monkey中，函数和别的基础类型一样，能够作为函数的参数，作为函数的返回值

函数还可以有缺省参数和可变参数。

```swift
//define a function
let add = fn() { [5,6] }
let n = [1, 2] + [3, 4] + add()
println(n)


let complex = {
   "add" => fn(x, y) { return fn(z) {x + y + z } }, //function with closure
   "sub" => fn(x, y) { x - y },
   "other" => [1,2,3,4]
}
println(complex["add"](1, 2)(3))
println(complex["sub"](10, 2))
println(complex["other"][2])


let warr = [1+1, 3, fn(x) { x + 1}(2),"abc","def"]
println(warr)


println("\nfor i in 5..1 where i > 2 :")
for i in fn(x){ x+1 }(4)..fn(x){ x+1 }(0) where i > 2 {
  if (i == 3) { continue }
  println('i={i}')
}


// 缺省参数和可变参数
add = fn (x, y=5, z=7, args...) {
    w = x + y + z
    for i in args {
        w += i
    }
    return w
}

w = add(2,3,4,5,6,7)
println(w)
```

## Pipe操作符

`pipe`操作符来自[Elixir](https://elixir-lang.org/).

```swift
# Test pipe operator(|>)
x = ["hello", "world"] |> strings.join(" ") |> strings.upper() |> strings.lower() |> strings.title()
printf("x=<%s>\n", x)

let add = fn(x,y) { return x + y }
let pow = fn(x) { return x ** 2}
let subtract = fn(x) { return x - 1}

let mm = add(1,2) |> pow() |> subtract()
printf("mm=%d\n", mm)
```

## Spawn 和 channel

你可以使用`spawn`来创建一个新的线程,  `chan`来和这个线程进行交互.

```swift
let aChan = chan()
spawn fn() {
    let message = aChan.recv()
    println('channel received message=<{message}>')
}()

//发送信息到线程
aChan.send("Hello Channel!")
```

## 标准模块介绍

Monkey中,预定义了一些标准模块，例如：json, sql, sort, fmt, os, logger, time, flag, net, http等等。

下面是对monkey的标准模块的一个简短的描述。

#### fmt 模块

```swift
let i = 108, f = 25.383, b=true, s = "Hello, world",
    aArr = [1, 2, 3, 4, "a", "b"],
    aHash = { "key1" => 1, "key2" => 2, "key3" => "abc"}

//使用 '%v (value)' 来打印变量值, '%_' 来打印变量类型
fmt.printf("i=[%05d, %X], b=[%t], f=[%.5f], s=[%-15s], aArr=%v, aHash=%v\n", i, i, b, f, s, aArr, aHash)
fmt.printf("i=[%_], b=[%t], f=[%f], aArr=%_, aHash=%_, s=[%s] \n", i, b, f, aArr, aHash, s)

sp = fmt.sprintf("i=[%05d, %X], b=[%t], f=[%.5f], s=[%-15s]\n", i, i, b, f, s)
fmt.printf("sp=%s", sp)

fmt.fprintf(stdout, "Hello %s\n", "world")
```

#### time 模块

```swift
t1 = newTime()
format = t1.strftime("%F %R")
println(t1.toStr(format))
Epoch = t1.toEpoch()
println(Epoch)

t2 = t1.fromEpoch(Epoch)
println(t2.toStr(format))
```

#### logger 模块

```swift
#输出到标准输出(stdout)
log = newLogger(stdout, "LOGGER-", logger.LSTDFLAGS | logger.LMICROSECONDS)

log.printf("Hello, %s\n", "logger")
fmt.printf("Logger: flags =<%d>, prefix=<%s>\n", log.flags(), log.prefix())

#输出到文件
file = newFile("./logger.log", "a+")
log.setOutput(file)
for i in 1..5 {
    log.printf("This is <%d>\n", i)
}
```

#### flag 模块(处理命令行选项)

```swift
let verV = flag.bool("version", false, "0.1")
let ageV = flag.int("age", 40, "an int")
let heightV = flag.float("height", 120.5, "a float")
let nameV = flag.string("name", "HuangHaiFeng", "a string")
let hobbiesV = flag.string("hobbies", "1,2,3", "a comma-delimited string")
flag.parse()

println("verV = ", verV)
println("ageV = ", ageV)
println("heightV = ", heightV)
println("nameV = ", nameV)
println("hobbies = ", hobbiesV.split(","))

if (flag.isSet("age")) {
    println("age is set")
} else {
    println("age is not set")
}
```

#### json 模块( json序列化(marshal)和反序列化(unmarshal) )

```swift
let hsJson = {"key1" => 10,
              "key2" => "Hello Json %s %s Module",
              "key3" => 15.8912,
              "key4" => [1,2,3.5, "Hello"],
              "key5" => true,
              "key6" => {"subkey1"=>12, "subkey2"=>"Json"},
              "key7" => fn(x,y){x+y}(1,2)
}
let hashStr = json.marshal(hsJson) //也可以使用 `json.toJson(hsJson)`
println(json.indent(hashStr, "  "))

let hsJson1 = json.unmarshal(hashStr)
println(hsJson1)


let arrJson = [1,2.3,"HHF",[],{ "key" =>10, "key1" =>11}]
let arrStr = json.marshal(arrJson)
println(json.indent(arrStr))
let arr1Json = json.unmarshal(arrStr)  //也可以使用 `json.fromJson(arrStr)`
println(arr1Json)
```

#### net 模块

```swift
//简单的TCP客户端
let conn = dialTCP("tcp", "127.0.0.1:9090")
if (conn == nil) {
    println("dailTCP failed, error:", conn.message())
    os.exit(1)
}

let n = conn.write("Hello server, I'm client")
if (n == nil) {
    println("conn write failed, error:", n.message())
    os.exit(1)
}

let ret = conn.close()
if (ret == false) {
    println("Server close failed, error:", ret.message())
}

//一个简单的TCP服务端
let ln = listenTCP("tcp", ":9090")
for {
    let conn = ln.acceptTCP()
    if (conn == nil) {
        println(conn.message())
    } else {
        printf("Accepted client, Address=%s\n", conn.addr())
    }
    spawn fn(conn) { //spawn a thread to handle the connection
        println(conn.read())
    }(conn)

} //end for

let ret = ln.close()
if (ret == false) {
    println("Server close failed, error:", ret.message())
}
```

#### linq 模块

```swift
let mm = [1,2,3,4,5,6,7,8,9,10]
println('before mm={mm}')

result = linq.from(mm).where(fn(x) {
    x % 2 == 0
}).select(fn(x) {
    x = x + 2
}).toSlice()
println('after result={result}')

result = linq.from(mm).where(fn(x) {
    x % 2 == 0
}).select(fn(x) {
    x = x + 2
}).last()
println('after result={result}')

let sortArr = [1,2,3,4,5,6,7,8,9,10]
result = linq.from(sortArr).sort(fn(x,y){
    return x > y
})
println('[1,2,3,4,5,6,7,8,9,10] sort(x>y)={result}')

result = linq.from(sortArr).sort(fn(x,y){
    return x < y
})
println('[1,2,3,4,5,6,7,8,9,10] sort(x<y)={result}')

thenByDescendingArr = [
    {"Owner" => "Google",    "Name" => "Chrome"},
    {"Owner" => "Microsoft", "Name" => "Windows"},
    {"Owner" => "Google",    "Name" => "GMail"},
    {"Owner" => "Microsoft", "Name" => "VisualStudio"},
    {"Owner" => "Google",    "Name" => "GMail"},
    {"Owner" => "Microsoft", "Name" => "XBox"},
    {"Owner" => "Google",    "Name" => "GMail"},
    {"Owner" => "Google",    "Name" => "AppEngine"},
    {"Owner" => "Intel",     "Name" => "ParallelStudio"},
    {"Owner" => "Intel",     "Name" => "VTune"},
    {"Owner" => "Microsoft", "Name" => "Office"},
    {"Owner" => "Intel",     "Name" => "Edison"},
    {"Owner" => "Google",    "Name" => "GMail"},
    {"Owner" => "Microsoft", "Name" => "PowerShell"},
    {"Owner" => "Google",    "Name" => "GMail"},
    {"Owner" => "Google",    "Name" => "GDrive"}
]

result = linq.from(thenByDescendingArr).orderBy(fn(x) {
    return x["Owner"]
}).thenByDescending(fn(x){
    return x["Name"]
}).toOrderedSlice()    //Note: You need to use toOrderedSlice

//use json.indent() for formatting the output
let thenByDescendingArrStr = json.marshal(result)
println(json.indent(thenByDescendingArrStr, "  "))

//测试 'selectManyByIndexed'
println()
let selectManyByIndexedArr1 = [[1, 2, 3], [4, 5, 6, 7]]
result = linq.from(selectManyByIndexedArr1).selectManyByIndexed(
fn(idx, x){
    if idx == 0 { return linq.from([10, 20, 30]) }
    return linq.from(x)
}, fn(x,y){
    return x + 1
})
println('[[1, 2, 3], [4, 5, 6, 7]] selectManyByIndexed() = {result}')

let selectManyByIndexedArr2 = ["st", "ng"]
result = linq.from(selectManyByIndexedArr2).selectManyByIndexed(
fn(idx,x){
    if idx == 0 { return linq.from(x + "r") }
    return linq.from("i" + x)
},fn(x,y){
    return x + "_"
})
println('["st", "ng"] selectManyByIndexed() = {result}')
```

#### csv 模块

```swift
//测试 csv reader
let r = newCsvReader("./examples/test.csv")
if r == nil {
    printf("newCsv returns err, message:%s\n", r.message())
}

r.setOptions({"Comma"=>";", "Comment"=>"#"})

ra = r.readAll()
if (ra == nil) {
    printf("readAll returns err, message:%s\n", ra.message())
}

for line in ra {
    println(line)
    for record in line {
        println("	", record)
    }
}

//测试 csv writer
let ofile = newFile("./examples/demo.csv", "a+")
let w = newCsvWriter(ofile)
w.setOptions({"Comma"=>"	"})
w.write(["1", "2", "3"])
w.writeAll([["4", "5", "6"],["7", "8", "9"],["10", "11", "12"]])
w.flush()
```

#### template 模块

`template` 模块包含'text'和'html'模版处理.

使用 `newText(...)` 或者 `parseTextFiles(...)` 来创建一个新的'text'模版。

使用 `newHtml(...)` 或者`parseHtmlFiles(...)` 来创建一个新的'html'模版。

```swift
arr = [
    { "key" => "key1", "value" => "value1" },
    { "key" => "key2", "value" => "value2" },
    { "key" => "key3", "value" => "value3" }
]

//使用parseTextFiles(), 来写入一个字符串
template.parseTextFiles("./examples/looping.tmpl").execute(resultValue, arr)
println('{resultValue}')

//使用parseTextFiles()来写入一个文件
file = newFile("./examples/outTemplate.log", "a+")
template.parseTextFiles("./examples/looping.tmpl").execute(file, arr)

//使用 parse()
//注: 我们需要使用"{{-" and "-}}"来移除输出中的回车换行(newline)
template.newText("array").parse(`Looping
{{- range . }}
        key={{ .key }}, value={{ .value -}}
{{- end }}
`).execute(resultValue, arr)
println('{resultValue}')
```

#### sql 模块

`sql` 模块提供了一个底层封装来操作数据库。

它可以正确的处理数据库中的null值,虽然没有经过完全的测试。

为了测试`sql`模块, 你需要做以下几个步骤:

1. 下载sql驱动器(sql driver)代码.

2. 将驱动器的包包含到'sql.go'文件中:

```go
    _ "github.com/mattn/go-sqlite3"
```

3. 重新编译monkey源码.

下面是一个完整的使用数据库的例子(`examples/db.my`):

```swift
let dbOp = fn() {
    os.remove("./foo.db") //delete `foo.db` file
    let db = dbOpen("sqlite3", "./foo.db")
    if (db == nil) {
        println("DB open failed, error:", db.message())
        return false
    }
    defer db.close()
    let sqlStmt = `create table foo (id integer not null primary key, name text);delete from foo;`
    let exec_ret = db.exec(sqlStmt)
    if (exec_ret == nil) {
        println("DB exec failed! error:", exec_ret.message())
        return false
    }
    
    let tx = db.begin()
    if (tx == nil) { 
        println("db.Begin failed!, error:", tx.message())
        return false
    }
    
    let stmt = tx.prepare(`insert into foo(id, name) values(?, ?)`)
    if (stmt == nil) {
        println("tx.Prepare failed!, error:", stmt.message())
        return false
    }
    
    defer stmt.close()
    let i = 0
    for (i = 0; i < 105; i++) {
        let name = "您好" + i
        if (i>100) {
            //插入`null`值. 有五个预定义的null常量：INT_NULL,FLOAT_NULL,STRING_NULL,BOOL_NULL,TIME_NULL.
            let rs = stmt.exec(i, sql.STRING_NULL)
        } else {
            let rs = stmt.exec(i, name)
        }
        
        if (rs == nil) {
            println("statement exec failed, error:", rs.message())
            return false
        }
    } //end for
    
    tx.commit()
    
    let id = 0, name = ""
    let rows = db.query("select id, name from foo")
    if (rows == nil) {
        println("db queue failed, error:", rows.message())
        return false
    }
    defer rows.close()
    while (rows.next()) {
        rows.scan(id, name)
        if (name.valid()) { //检查是否为`null`
            println(id, "|", name)
        } else {
            println(id, "|", "null")
        }
    }
    return true
}

let ret = dbOp()
if (ret == nil) {
    os.exit(1)
}

os.exit()
```

## 实用工具

项目还包含了一些使用的工具：`formatter`和`highlighter`。

formatter工具能够格式化monkey语言。
highlighter工具能够语法高亮monkey语言（提供两种输出：命令行和html）。

你也可以将它们合起来使用:

```sh
./fmt xx.my | ./highlight  //输出到屏幕(命令行高亮不只是windows)
```

## 语法高亮

目前，monkey支持三种编辑器的语法高亮:

1. vim

    [vim](misc/vim)


2. emeditor

    [emeditor](misc/emeditor)

3. notepad++

    [notepad++](misc/notepad%2B%2B)

4. Visual Studio Code

    [VSC](misc/vscode)

5. Sublime Text 3

    [Sublime Text 3](misc/SublimeText3)

## 未来计划

下面是对项目的未来计划的描述:

* 改进标准库并增加更多的函数.
* 写更多的测试代码!

## 许可证

MIT

## 备注

如果你喜欢此项目，请点击下面的链接，多多star，fork。谢谢！
[monkey](https://github.com/haifenghuang/monkey)
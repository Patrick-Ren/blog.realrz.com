---
title: 正则表达式
date: "2021-02-15"
language: zh-CN
category: other
---

> 字符串是单个字符排列组合的结果，当我们做字符串匹配的时候就需要一种描述这种排列组合关系的工具或者语言，这就是正则表达式诞生的背景

#### DSL

DSL 是 Domain Specific Language 的缩写，它不是一种通用的语言，而是为了解决某一类特殊领域的问题而发明的专用语言。SQL 和 AWK 这种语言都是 DSL，不能解决通用的问题，但是在自己的领域能够大大提升工作效率。

正则表达式 (regular expression) 就可以看作是一个 DSL, 有着自己的语法，目的清晰明确，着力解决字符匹配问题。

#### 标志 (indicator)

正则表达式本身也是一个字符串, 一般用斜杠(/)进行标记: /正则表达式内容/，这告诉执行环境：这是一个正则表达式，你应该按照相关的规则去进行解析。

#### 元字符和字面字符

正则表达式中的所有字符可以分为两类：元字符(meta character) 和字面字符(literal character)

元字符是一类有特殊意义的字符, 这里列出元字符的类别（后面会详细说明）:

1. 边界：caret(^) 和 dollar($) 表示目标的开始和结尾
2. wildcard: 星号(\*), 加号(+), 问号(?), 点(.)
3. 字符集合的标记: []
4. 分组标记: ()
5. 逻辑标记: 或(|)
6. 转义字符: 反斜杠(\\), 因为特殊字符默认有特殊含义，这时要表示这些特殊字符就需要进行转义

字面字符比较容易理解，就是字符本身， a 就是 a, 1 就是 1

#### 边界 (bounding)

可以将要匹配的字符的称为目标, 正则表达式其实就是在描述目标的模式(pattern), 通俗一点说就是目标长的样子。

一个很基本的 pattern 就是字符边界: starts with 和 ends with

在正则表达式中用 caret(^) 表示字符串的开头，用 dollar($) 表示字符串的结尾 (Vim 用户应该很熟悉吧)

#### wildcard

wildcard 是一种特殊的字符，相当于占位符，用在正则表达式里可以表示一定数量的任意字符。正则表达式中一些主要的 wildcard 有:

1. 星号(\*) 放在某个字符后面表示该字符的数量为 0 个或者多个
2. 加号(+) 放在某个字符后面表示该字符的数量为 1 个
3. 问号(?) 放在某个字符后面表示该字符的数量为 0 个或 1 个
4. 点(.) 表示任意一个字符

#### 字符集合 (character set)

在正则表达式中用方括号([])表示一系列字符的集合, e.g.

[afg] 表示一个字符，这个字符必须是小写 a 或 f 或 g

用 hyphen (-) 可以表示 range, e.g.

- [a-f] 表示一个字符，这个字符必须在英文字母表中 a 到 f 之间（小写）
- [A-Z] 表示任意一个大写英文字符
- [1-9] 表示任意一个阿拉伯数字
- [a-zA-Z] 表示任意一个英文字母

集合同样支持非操作，在逻辑上表示“不在此集合中”的字符, 在集合前面追加一个感叹号(!)。

- [!2-6] 表示一个字符，这个字符不是数字 2,3,4,5,6
- [!2-6a-z] 表示一个字符，这个字符不是数字 2,3,4,5,6 也不是小写字母

#### 字符集合 + wildcard

还没来得及写 :)

#### 例子

重要: 下面的例子中使用了 grep (linux 下一种命令行程序), 它会扫描输入中的每一行，并打印其中符合搜索条件(正则表达式)的行。grep 不使用斜杠(/)来标记正则表达式，因为 grep 基于正则表达式已经是个共识，没必要再进行标记。

假设当前文件夹下有个名为 test.txt 的文件，其中内容如下 (每一行的结尾都没有空格):

```
I
walked into a
little sidewalk
and saw she
walk away.
A while latter,
I started to work on
the work I left yesterday
about the works of Shakespeare.
```

1. grep -E "walk" test.txt

```
walked into a
little sidewalk
walk away.
```

2. grep -E "^walk" test.txt

```
walked into a
walk away.
```

3. grep -E "walk$" test.txt

```
little sidewalk
```

4. grep -E "w[a-z]\*k" test.txt
5. grep -E "w[a-z]+k" test.txt
6. grep -E "w[a-z].k" test.txt
7. grep -E "w..k" test.txt
8. grep -E "walk|work" test.txt

```
walked into a
little sidewalk
walk away.
I started to work on
the work I left yesterday
about the works of Shakespeare.
```

9. grep -E "^I|e$|^walk" test.txt

```
I
walked into a
and saw she
walk away.
I started to work on
```

10. grep -E "works?" test.txt

```
I started to work on
the work I left yesterday
about the works of Shakespeare.
```
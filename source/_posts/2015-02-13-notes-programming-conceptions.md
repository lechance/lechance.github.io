---
title: Conceptions of Java
date: 2015-02-13 20:39:13
categories:
- Java
tags:
- java
- note
---
#### Programming Conceptions

独立的地址空间使得进程共享状态信息变得更加困难。为了共享信息，他们必须使用显式的ＩＰＣ(interprocess communication, IPC)机制。基于进程设计的另一个缺点是，他们往往比较慢，因为进程控制和IPC的开销都很高．

一个线程(thread)就是运行在一个进程上下文中的逻辑流．线程由内核调度．每个线程都有它自己的线程上下文(thread context)，包含一个唯一的整数线程ID(Thread ID,TID),栈，栈指针，程序计数器,通用目的寄存器和条件码．所有运行在一个进程里的线程共享该进程的虚拟地址空间．
基于线程的逻辑流结合了基于进程和基于I/O多路复用的流的特性．

thread routine 线程例程

分离线程：在任何一个时间点上，线程是可结合的（joinable）或者是可以分离的（detached）的．一个可结合的线程能够被其他线程收回和杀死．在被其他线程收回之前，它的存储资源（例如栈）是不释放的．相反，一个分离的线程是不能被其他线程回收和杀死的．他的存储器资源在它终止时由系统自动释放．
默认情况下，线程是被创建成可结合的．为了避免存储器泄露，每个线程都应该要么被其他线程显式地收回，要么通过调用pthread_detached函数被分离．

#### Operator in Java

##### Arithmetic Operators

Mathematical (or arithmetic) operators are used in performing mathematical operations in an arithmetic expression. The various arithmetic operators are listed in Table SR2-1.


```sh
					Operator							Symbol

					Addition					  		+
					Increment					  		++
					Division					  		/
					Unary Negation					  		~
					Unary Minus							-
					Multiplication					  		*
					Logical NOT							!
					Unary  Plus							+
					Subtraction							-
					Decrement						  	--
					Remainder					  		%
					Typecasting					  		(type)
```

TABLE SR2-1.   Arithmetic Operators

##### Unary Operators

As the name indicates, unary operators operate on a single operand. We discuss the four unary
operators in this section. You have already seen the use of the unary minus `(-)` operator in the
previous example. The unary minus operator negates the operand. Likewise, a unary plus `(+)`
operator is available in Java.
Tip:The unary plus operator is included for symmetry only and has no
effect on the sign of its operand.
The increment (++) and decrement (--) operators increment or decrement the value of their
operand by 1. These are also called “auto” operators because they operate and assign the result
automatically. The implicit operand for both the operators is 1, which means that the given
operand value is either increased or decreased by exactly 1, depending on the operation.

##### Post and Pre Operators

The two “auto” operators can be applied to a variable as a suffix or a prefix. Depending on which
side of the variable they are applied, they produce different results.

String Concatenation
In Java, the + operator, when applied to a string data type, performs concatenation of strings.
Tip
In other languages, the use of the + operator for string concatenation
is considered a feature of operator overloading. Java does not support
operator overloading in the sense that a developer cannot redefine
the meaning of any of the operators. The string concatenation is
equivalent to operator overloading defined internally in Java.

##### Type Conversions
Oftentimes, in a program statement, you assign the value of one data type to another. For example,
you may assign an integer variable to a float type, or vice versa. When you assign an integer to a
float type, there is no loss of magnitude.

*Caution:*
>Converting int to float may result in loss of precision.

##### Bitwise Operators
Bitwise operators are used to test, set, or shift actual bits in a byte or a word that corresponds to
the char and int data types. These operators cannot perform operations on data types such as
float, double, and boolean. Table SR2-4 lists the various bitwise operators.

``` sh
				Operator					   Meaning

				&					  		AND
				>>					  		Right-shift
				~					  		NOT
				<<					  		Left-shift
				^					  		XOR
				|					  		OR
				>>>					  		Right-shift with zero fill
``` 

TABLE SR2-4.	  Bitwise Operators

These operators perform the well-known Boolean operations, as their names suggest.

##### Logical Conditional Operators

As mentioned earlier, logical operators can take one, two, or three operands. The lists of operators
that fall in this category are given in Table SR2-5. The first two operators operate on two operands,
the NOT operator operates on a single operand, and the ternary conditional operator takes three
operands.

``` sh
Operator						 Meaning
&&					  		AND
!					  		NOT
||					  		OR
?:					  		ternary condition
``` 

TABLE SR2-5.    Logical Operators

##### Relational Operators
Relational operators and used to compare two values.The output of the comparison is always a
Boolean value, which is true or false. The various relational operators are listed in Table SR2-6.
These operators generally operate on numeric and character data types.

``` sh

Operation                Operator

Greater than                >
Not equal to                !=
Less than or equal to       <=
Greater than or equal to    >=
Less than                   <
Equal to                    ==
			instanceof
``` 

TABLE SR2-6.   Relational Operators

##### Assignment Operators

The basic assignment operator is the equal to (=)operator,where the right-side value of the expression is assigned to the variable defined on the left-side of the expression.
The various assignment operators are listed in Table SR2-7.


``` sh
Operation	(操作)					 Operator(操作符)
Assignment					          =
Add and assign					          +=
Subtract and assign					  -=
Multiply and assign					  *=
Divide and assign					  /=
Modulo divide and assign				  %=
Left shift and assign					  <<=
Right shift and assign					  >>=
Unsigned right shift and assign				  >>>=
Bitwise logical AND and assign				  &=
Bitwise OR and assign					  |=
Bitwise exclusive OR and assign			          ^=
``` 

TABLE SR2-7.            Assignment Operators

##### Separators

The following nine ASCII characters are the separators (punctuators) in Java:
```
Separator : one of
( )  {  }  [  ]   ;   ,   .
```
They carry special meaning when used in program statements.For example, the parentheses are used in a function call and typecasting. The curly braces are used for compounding statements. The square brackets are used in array declarations. The semicolon is used in terminating a program statement; a comma is used in separating multiple statements and declarations. Finally, a dot is used in defining the package hierarchy or calling object methods. You will learn the use of these separators in the book as we delve deeper into the language syntax.

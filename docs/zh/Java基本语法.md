## Java基本语法

### （一）关键字和保留字

关键字的定义和特点
定义：被java语言赋予了特殊含义，用作专门用途的字符串。

特点：关键字中所有字母都为小写。关键字不能用作变量名，方法名，类名，包名和参数。               

2.保留字
定义:现在java尚未使用，但以后版本可能会作为关键字使用。自己使用时应避免使用。

### （二）标识符

标识符
凡是可以自己命名的地方都叫标识符。例如：包名，类名，方法等。

定义合法标识符规则

```
1.有26个英文字母大小写，0-9，_或$组成。
2.不能以数字开头。
3.不可以使用关键字和保留字，但能包含关键字和保留字。
4.严格区分大小写。
5.标识符不能包含空格。
```

java中的名称命名规范

```
1.包名：多单词组成时所有字母都小写：xxxyyyzzz
2.类名、接口名：多单词组成时，所有单词的首字母大写：XxxYyyZzz
3.变量名、方法名：多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写：xxxYyyZzz
4.常量名：所有字母都大写。多单词时每个单词用下划线连接：XXX_YYY_ZZZ
```

### （三）变量

变量的概念

```
1.内存中的一个储存区域。
2.该区域的数据可以在同一类型范围内不断变化。
3.变量是程序中最基本的存储单元。包含变量类型、变量名和存储的值
```

变量的作用

```
用于内存中保存数据。
```

变量的声明

```
语法：<数据类型> <变量名称>
for example：int a;
```

变量的赋值

```
语法：<变量名称> = <值>
for example：a = 10;
```

注意：java中每个变量必须要先声明，后使用。

变量的声明和赋值

```
语法：<数据类型> <变量名> = <初始化值>
for example：int a = 10;
```

在方法体外，类体内声明的变量称为成员变量。

在方法体内部声明的变量称为局部变量。

二者在初始化方面的异同
同：都有生命周期。 异：局部变量除形参外，需要初始化。

### （四）基本数据类型

##### 自动类型转换（隐式）

```
1.特点：代码不需要进行特殊处理，自动完成
2.规则：数据范围从小到大
```

例如：int类型自动转化为long类型

```
long b = 10;
```


注意：

```
1.byte,short,char之间不会相互转换，他们三者在计算时首先转换为int类型。
2.boolean类型不能与其它数据类型运算。
3.当把任何基本数据类型的值和字符串(String)进行连接运算时(+)，基本数据类型的值将自动转化为字符串(String)类型。
```

##### 强制类型转换（显示）

1.特点：代码需要进行特殊的格式处理，不能自动完成

2.规则：数据范围由小到大

格式如下：

```
int b = (int)10L;
```

```
整数类型：byte，short，int，long
注意：java的整型常亮默认为int型，声明long型常量须后加 'l’或’L’

浮点类型：float、double
注意：Java 的浮点型常量默认为double型，声明float型常量，须后加‘f’或‘F’

字符类型：char
注意：char类型是可以进行运算的。因为它都对应有Unicode码。

布尔类型：boolean
注意：boolean类型数据只允许取值true和false，无null。

boolean类型不可以转换为其它的数据类型。
```

### （五）运算符

运算符的分类

##### 1.算术运算符


自增(自减)运算符：++,–

例：a++:先赋值后运算

```
int a = 1;
int b = a++;
System.out.print(a);//a=2
System.out.print(b);//b=1
```

例：++a：先运算后赋值

```
int a = 1;
int b = ++a;
System.out.print(a);//a=2
System.out.print(b);//b=2
2.赋值运算符
```

##### 2.赋值运算符

##### 3.比较运算符

比较运算符，或者叫（关系运算符）


注意：

```
1.>、 < 、 >= 、 <= 只支持左右两边操作数是数值类型
2.== 、 != 两边的操作数既可以是数值类型，也可以是引用类型
3.== 比较基本数据类型时，比较的是值。比较引用数据类型时比较的是地址。
```

##### 4.逻辑运算符


“&”和“&&”的区别

单&时，左边无论真假，右边都进行运算；

双&时，如果左边为真，右边参与运算，如果左边为假，那么右边不参与运算。

“|”和“||”的区别同理，||表示：当左边为真，右边不参与运算。

##### 5.位运算符


位运算是直接对整数的二进制进行的运算

##### 6.三元运算符

语法形式：布尔表达式 ? 表达式1：表达式2

布尔表达式为true,运算结果为表达式1。反之，结果为表达式2。

```
int a = (10>3) ? 5 : 10;//10>3为true，a=5
int a = (10<3) ? 5 : 10;//10<3为false,a=10
```

##### 运算符优先级

### （六）程序流程控制

```
结构化程序设计中规定的三种基本流程结构，分别为：顺序结构，分支结构，循环结构。
```

##### 顺序结构

程序由上向下执行。

```
public class Test{
        int num1 = 12;
        int num2 = num1 + 2;
}//java中定义成员变量时采用合法的向前引用。
```

##### 分支结构

分支语句if-else

if语句三种格式：

分支语句switch-case

```java
public class SwitchTest {
    public static void main(String args[]) {
        int i = 1;
        switch (i) {
        case 0:
            System.out.println("one");
            break;
        case 1:
            System.out.println("two");
            break;
        default:
            System.out.println("other");
            break;
        }
    }
}
```


注意：

```
1.switch(表达式)中表达式的值必须是下述几种类型之一：
	byte，short，char，int，枚举 (jdk 5.0)，String (jdk 7.0)；
2.break语句用来在执行完一个case分支后使程序跳出switch语句块；
	如果没有break，程序会顺序执行到switch结尾
```

##### 循环结构

循环语句由四个组成部分

```
1.初始化部分
2.循环条件部分
3.循环体部分
4.迭代部分
```

循环结构：for循环

```
public class ForLoop {
    public static void main(String args[]) {
        int result = 0;
        for (int i = 1; i <= 100; i++) {//1.初始化部分 2.循环条件部分 3.迭代部分
            result += i;//循环体部分
        }
        System.out.println("result=" + result);
    }
}
```

循环结构：while循环

```
public class WhileLoop {
    public static void main(String args[]) {
        int result = 0;
        int i = 1;//1.初始化部分
        while (i <= 100) {//循环条件
            result += i;//循环体部分
            i++;//迭代部分
        }
        System.out.println("result=" + result);
    }
}
```

循环结构：do-while循环

```
public class DoWhileLoop {
    public static void main(String args[]) {
        int result = 0, i = 1;//初始化部分
        do {
            result += i;//循环体部分
            i++;//迭代部分
        } while (i <= 100);//循环条件部分
            System.out.println("result=" + result);
        }
}
```

注意：while与do…while的区别

```
while: 先判断 再执行 条件不成立 循环体 一遍都不执行
do…while: 先执行 再判断 条件不成立 循环体 至少执行一遍
```

### （七）特殊关键字

##### break语句

break语句用于终止某个语句块的执行

```
public class BreakTest{
		public static void main(String args[]){
	    for(int i = 0; i<10; i++){ 
	     	if(i==3)
		      break;//当条件成立时，终止for循环	
	    	System.out.println(" i =" + i);
	    }
	    System.out.println("Game Over!");
		}
}
```

##### continue语句

continue只能使用在循环结构中，用于跳过其所在循环语句块的一次执行，继续下一次循环

```
public class ContinueTest {
	public static void main(String args[]){
	     for (int i = 0; i < 100; i++) {
	      	  if (i%10==0)
			        continue;//跳出成立的循环
		      System.out.println(i);
		  }  
    }  
} 
```

注意：

```
1.return：并非专门用于结束循环的，它的功能是结束一个方法。
当一个方法执行到一个return语句时，这个方法将被结束。
2.与break和continue不同的是，return直接结束整个方法，不管这个return处于多少层循环之内
```


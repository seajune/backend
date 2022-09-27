- [基础](#基础)
    - [简介](#简介)
    - [特点](#特点)
    - [标准交互输入](#标准交互输入)
    - [数据类型](#数据类型)
        - [数组](#数组)
        - [Object对象](#Object对象)
    - [变量](#变量)
        - [全局变量](#全局变量)
        - [静态变量](#静态变量)
    - [常量](#常量)
    - [引用](#引用)
    - [运算符](#运算符)
        - [错误控制运算符](#错误控制运算符)
        - [执行运算符](#执行运算符)
        - [字符串运算符](#字符串运算符)
        - [数组运算符](#数组运算符)
        - [??和?:的区别](#??和?:的区别)
    - [流程控制](#流程控制)
        - [switch](#switch)
        - [include](#include)
        - [require](#require)
        - [include_once](#include_once)
        - [require_once](#require_once)
        - [goto](#goto)
    - [函数](#函数)
        - [可变数量的参数列表](#可变数量的参数列表)
        - [可变函数](#可变函数)
        - [匿名函数](#匿名函数)
    - [类与对象](#类与对象)
        - [构造函数](#构造函数)
        - [析构函数](#析构函数)
        - [访问控制（可见性）](#访问控制（可见性）)
        - [范围解析操作符（::）](#范围解析操作符（::）)
        - [parent、static、self和this](#parent、static、self和this)
        - [抽象类](#抽象类)
        - [对象接口](#对象接口)
        - [遍历对象](#遍历对象)
        - [Final关键字](#Final关键字)
        - [对象比较](#对象比较)
    - [命名空间](#命名空间)
        - [定义命名空间](#定义命名空间)
        - [使用命名空间](#使用命名空间)
        - [全局空间](#全局空间)
        - [名称解析](#名称解析)
    - [异常处理](#异常处理)
    - [生成器](#生成器)
    - [上下文](#上下文)
    - [内存管理](#内存管理)
    - [包管理工具](#包管理工具)
        - [Composer](#Composer)
            - [下载](#下载)
            - [局部安装](#局部安装)
            - [全局安装](#全局安装)
            - [Packagist镜像](#Packagist镜像)
                - [镜像用法](#镜像用法)
                - [镜像原理](#镜像原理)
                - [解除镜象](#解除镜象)
            - [基本用法](#基本用法)
                - [composer.json：项目安装](#composer.json：项目安装)
                - [安装依赖包](#安装依赖包)
                - [composer.lock：锁文件](#composer.lock：锁文件)
            - [命令行](#命令行)
            - [composer.json架构](#composer.json架构)
- [注意](#注意)
    - [php中字符串和整数比较的操作方法](#php中字符串和整数比较的操作方法)
    - [比较浮点数](#比较浮点数)
    - [删除数组的键和重建索引](#删除数组的键和重建索引)
    - [php和nginx的关系，为什么需要用到nginx？](#php和nginx的关系，为什么需要用到nginx？)
    - [LNMP](#LNMP)

---
# 基础
## 简介
PHP是解释型语言，动态语言类型。

PHP是类C语言。

[PHP解析过程](https://www.php.cn/php-ask-430514.html)<br>
[PHP底层的运行机制与原理](https://www.cnblogs.com/wanglijun/p/8830932.html)<br>
[用户对动态PHP网页访问过程，以及nginx解析php步骤](https://blog.csdn.net/riuhazen/article/details/78684584)

## 特点
* 可以被嵌入HTML语言。
* 跨平台。
* 效率高，消耗相当少的系统资源。
* 面向对象。

## 标准交互输入
```php
<?php
// ask for input
fwrite(STDOUT, "Enter your name: ");

// get input
$name = trim(fgets(STDIN));　// 接收用户输入

// write input back
fwrite(STDOUT, "Hello, $name!");
?>
```
```shell
//运行结果
Enter your name: davy
Hello, davy!
```
* STDIN：标准的输入设备。
* STDOUT：标准的输出设备。
* STDERR：标准的错误设备。

可以在PHP脚本里使用这三个常量，以接受用户的输入，或者显示处理和计算的结果。

## 数据类型
标量类型：
* boolean（布尔型）
* integer（整型）
* float（浮点型，也称作 double)
* string（字符串）

复合类型：
* [array（数组）](#数组)
* object（对象）
* callable（可调用）

特殊类型：
* resource（资源）
* NULL（无类型）

### **数组**
用 array() 新建一个数组，它接受任意数量用逗号分隔的 键（key） => 值（value）对。
```php
array(  key =>  value
     , ...
     )
// 键（key）可是是一个整数 integer 或字符串 string
// 值（value）可以是任意类型的值
```
如果对给出的值没有指定键名，则取当前最大的整数索引值，而新的键名将是该值加一。如果指定的键名已经有了值，则该值会被覆盖。<br>
注意所使用的最大整数键名不一定当前就在数组中。它只要在上次数组重新生成索引后曾经存在过就行了。
### Object对象

## 变量
变量以$符号开始。
### 全局变量
在函数中访问全局变量加关键字global。
### 静态变量
使用static声明。仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不丢失。

## 常量
使用define()函数定义。

## 引用
PHP的引用允许用两个变量来指向同一个内容。

## 运算符
### 错误控制运算符
@符号在PHP中用作错误控制操作符。当表达式附加@符号时，将忽略该表达式可能生成的错误消息。如果启用了track_errors功能，则表达式生成的错误消息将保存在变量$php_errormsg中。每个错误都会覆盖此变量。
### 执行运算符
执行运算符：反引号（``）。PHP将反引号中的内容作为shell命令来执行，并将其输出信息返回。<br>
反引号不能在双引号字符串中使用。
### 字符串运算符
* 连接运算符（“.”），它返回其左右参数连接后的字符串。
* 连接赋值运算符（“.=”），它将右边参数附加到左边的参数之后。
### 数组运算符
### ??和?:的区别
* $a ?? 0 等同于 isset($a) ? $a : 0。
* $a ?: 0 等同于 $a ? $a : 0。
* empty: 判断一个变量是否为空(null、false、00、0、’0′、』这类，都会返回true)。
* isset: 判断一个变量是否设置(值为false、00、0、’0′、』这类，也会返回true)。

## 流程控制
### switch
continue语句作用到switch上的作用类似于 break。如果在循环中有一个switch并希望continue到外层循环中的下一轮循环，用continue 2。<br>
switch/case作的是松散比较。
### **include**
被包含文件先按参数给出的路径寻找，如果没有给出目录（只有文件名）时则按照include_path（php.ini文件中配置）指定的目录寻找。如果在include_path下没找到该文件则include最后才在调用脚本文件所在的目录和当前工作目录下寻找。如果最后仍未找到文件则include结构会发出一条警告；这一点和require不同，后者会发出一个致命错误。<br>
当一个文件被包含时，其中所包含的代码继承了 include 所在行的变量范围。从该处开始，调用文件在该行处可用的任何变量在被调用的文件中也都可用。不过所有在包含文件中定义的函数和类都具有全局作用域。
```php
vars.php
<?php

$color = 'green';
$fruit = 'apple';

?>

test.php
<?php

echo "A $color $fruit"; // A

include 'vars.php';

echo "A $color $fruit"; // A green apple

?>
```
### **require**
require和include几乎完全一样，除了处理失败的方式不同之外。require在出错时产生E_COMPILE_ERROR级别的错误。换句话说将导致脚本中止而include只产生警告（E_WARNING），脚本会继续运行。
### include_once
include_once语句在脚本执行期间包含并运行指定文件。此行为和include 语句类似，唯一区别是如果该文件中已经被包含过，则不会再次包含。如同此语句名字暗示的那样，只会包含一次。<br>
include_once 可以用于在脚本执行期间同一个文件有可能被包含超过一次的情况下，想确保它只被包含一次以避免函数重定义，变量重新赋值等问题。
### require_once
require_once语句和require语句完全相同，唯一区别是PHP会检查该文件是否已经被包含过，如果是则不会再次包含。
### goto
goto操作符可以用来跳转到程序中的另一位置。该目标位置可以用目标名称加上冒号来标记，而跳转指令是goto之后接上目标位置的标记。PHP中的goto有一定限制，目标位置只能位于同一个文件和作用域，也就是说无法跳出一个函数或类方法，也无法跳入到另一个函数。也无法跳入到任何循环或者switch结构中。可以跳出循环或者switch，通常的用法是用goto代替多层的break。

## 函数
PHP不支持函数重载，也不可能取消定义或者重定义已声明的函数。
### 可变数量的参数列表
PHP在用户自定义函数中支持可变数量的参数列表，由...语法实现。
```php
<?php
function sum(...$numbers) {
    $acc = 0;
    foreach ($numbers as $n) {
        $acc += $n;
    }
    return $acc;
}

echo sum(1, 2, 3, 4);
?>
```
### 可变函数
如果一个变量名后有圆括号，PHP将寻找与变量的值同名的函数，并且尝试执行它。可变函数可以用来实现包括回调函数，函数表在内的一些用途。
```php
<?php
function foo() {
    echo "In foo()<br />\n";
}

function bar($arg = '') {
    echo "In bar(); argument was '$arg'.<br />\n";
}

// 使用 echo 的包装函数
function echoit($string)
{
    echo $string;
}

$func = 'foo';
$func();        // This calls foo()

$func = 'bar';
$func('test');  // This calls bar()

$func = 'echoit';
$func('test');  // This calls echoit()
?>
```
### 匿名函数

## **类与对象**
### 构造函数
如果子类中定义了构造函数则不会隐式调用其父类的构造函数。要执行父类的构造函数，需要在子类的构造函数中调用 parent::__construct()。如果子类没有定义构造函数则会如同一个普通的类方法一样从父类继承（假如没有被定义为 private 的话）。
### 析构函数
析构函数即使在使用exit()终止脚本运行时也会被调用。在析构函数中调用exit()将会中止其余关闭操作的运行。
### 访问控制（可见性）
对属性或方法的访问控制，是通过在前面添加关键字 public（公有），protected（受保护）或 private（私有）来实现的。被定义为公有的类成员可以在任何地方被访问。被定义为受保护的类成员则可以被其自身以及其子类和父类访问。被定义为私有的类成员则只能被其定义所在的类访问。<br>
如果用 var 定义，则被视为公有。<br>
同一个类的对象即使不是同一个实例也可以互相访问对方的私有与受保护成员。这是由于在这些对象的内部具体实现的细节都是已知的。
### 范围解析操作符（::）
范围解析操作符，可以用于访问静态成员，类常量，还可以用于覆盖类中的属性和方法。<br>
当在类定义之外引用到这些项目时，要使用类名。
### **parent、static、self和this**
这三个特殊的关键字是用于在类定义的内部对其属性或方法进行访问的。
* parent：parent::访问父类。
* static：定义类中静态变量。
* self：self::访问类中声明成const（常量）或者static的变量和方法。
* this：伪变量，指向当前对象的指针。$this->访问类中不为const（常量）或者static的变量和方法。
### 抽象类
定义为抽象的类不能被实例化。任何一个类，如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。被定义为抽象的方法只是声明了其调用方式（参数），不能定义其具体的功能实现。<br>
继承一个抽象类的时候，子类必须定义父类中的所有抽象方法；另外，这些方法的访问控制必须和父类中一样（或者更为宽松）。例如某个抽象方法被声明为受保护的，那么子类中实现的方法就应该声明为受保护的或者公有的，而不能定义为私有的。此外方法的调用方式必须匹配，即类型和所需参数数量必须一致。例如，子类定义了一个可选参数，而父类抽象方法的声明里没有，则两者的声明并无冲突。
### 对象接口
接口（interface）<br>
使用接口（interface），可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。<br>
接口是通过 interface关键字来定义的，就像定义一个标准的类一样，但其中定义所有的方法都是空的。
接口中定义的所有方法都必须是公有，这是接口的特性。<br>
需要注意的是，在接口中定义一个构造方法是被允许的。在有些场景下这可能会很有用，例如用于工厂模式时。<br>
接口也可以继承，通过使用extends操作符。

实现（implements）<br>
要实现一个接口，使用implements操作符。类中必须实现接口中定义的所有方法，否则会报一个致命错误。类可以实现多个接口，用逗号来分隔多个接口的名称。
```php
<?php

// 声明一个'iTemplate'接口
interface iTemplate
{
    public function setVariable($name, $var);
    public function getHtml($template);
}


// 实现接口
// 下面的写法是正确的
class Template implements iTemplate
{
    private $vars = array();
  
    public function setVariable($name, $var)
    {
        $this->vars[$name] = $var;
    }
  
    public function getHtml($template)
    {
        foreach($this->vars as $name => $value) {
            $template = str_replace('{' . $name . '}', $value, $template);
        }
 
        return $template;
    }
}

// 下面的写法是错误的，会报错，因为没有实现 getHtml()：
// Fatal error: Class BadTemplate contains 1 abstract methods
// and must therefore be declared abstract (iTemplate::getHtml)
class BadTemplate implements iTemplate
{
    private $vars = array();
  
    public function setVariable($name, $var)
    {
        $this->vars[$name] = $var;
    }
}
?>
```

常量<br>
接口中也可以定义常量。接口常量和类常量的使用完全相同，但是不能被子类或子接口所覆盖。
### 重载
PHP所提供的重载（overloading）是指动态地创建类属性和方法。是通过**魔术方法**（magic methods）来实现的。与大多数面向对象语言中“重载”的定义不一样。

**当调用当前环境下未定义或不可见的类属性或方法时，重载方法会被调用**。

所有的重载方法都必须被声明为public。

这些魔术方法的参数都不能通过引用传递。

* 属性重载<br>
public __set ( string $name , mixed $value ) : void<br>
public __get ( string $name ) : mixed<br>
public __isset ( string $name ) : bool<br>
public __unset ( string $name ) : void<br>
在给不可访问属性赋值时，__set() 会被调用。<br>
读取不可访问属性的值时，__get() 会被调用。<br>
当对不可访问属性调用 isset() 或 empty() 时，__isset() 会被调用。<br>
当对不可访问属性调用 unset() 时，__unset() 会被调用。<br>
参数 $name 是指要操作的变量名称。__set() 方法的 $value 参数指定了 $name 变量的值。

属性重载只能在对象中进行。在静态方法中，这些魔术方法将不会被调用。所以这些方法都不能被声明为static。
* 方法重载<br>
public __call ( string $name , array $arguments ) : mixed<br>
public static __callStatic ( string $name , array $arguments ) : mixed<br>
在对象中调用一个不可访问方法时，__call()会被调用。<br>
在静态上下文中调用一个不可访问方法时，__callStatic() 会被调用。<br>
$name 参数是要调用的方法名称。$arguments 参数是一个枚举数组，包含着要传递给方法 $name 的参数。
```php
<?php
class MethodTest 
{
    public function __call($name, $arguments) 
    {
        // 注意: $name 的值区分大小写
        echo "Calling object method '$name' "
             . implode(', ', $arguments). "\n";
    }

    /**  PHP 5.3.0之后版本  */
    public static function __callStatic($name, $arguments) 
    {
        // 注意: $name 的值区分大小写
        echo "Calling static method '$name' "
             . implode(', ', $arguments). "\n";
    }
}

$obj = new MethodTest;
$obj->runTest('in object context');

MethodTest::runTest('in static context');  // PHP 5.3.0之后版本
?>
//输出
Calling object method 'runTest' in object context
Calling static method 'runTest' in static context
```

### 遍历对象
PHP5提供了一种定义对象的方法使其可以通过单元列表来遍历，例如用foreach语句。默认情况下，所有可见属性都将被用于遍历。
```php
<?php
class MyClass
{
    public $var1 = 'value 1';
    public $var2 = 'value 2';
    public $var3 = 'value 3';

    protected $protected = 'protected var';
    private   $private   = 'private var';

    function iterateVisible() {
       echo "MyClass::iterateVisible:\n";
       foreach($this as $key => $value) {
           print "$key => $value\n";
       }
    }
}

$class = new MyClass();

foreach($class as $key => $value) {
    print "$key => $value\n";
}
echo "\n";

$class->iterateVisible();
?>

//输出
var1 => value 1
var2 => value 2
var3 => value 3

MyClass::iterateVisible:
var1 => value 1
var2 => value 2
var3 => value 3
protected => protected var
private => private var
```
### Final关键字
如果父类中的方法被声明为final，则子类无法覆盖该方法。如果一个类被声明为final，则不能被继承。<br>
属性不能被定义为 final，只有类和方法才能被定义为 final。
```php
<?php
class BaseClass {
   public function test() {
       echo "BaseClass::test() called\n";
   }
   
   final public function moreTesting() {
       echo "BaseClass::moreTesting() called\n";
   }
}

class ChildClass extends BaseClass {
   public function moreTesting() {
       echo "ChildClass::moreTesting() called\n";
   }
}
// Results in Fatal error: Cannot override final method BaseClass::moreTesting()
?>
```
### 对象比较
当使用比较运算符（\==）比较两个对象变量时，比较的原则是：如果两个对象的属性和属性值都相等，而且两个对象是同一个类的实例，那么这两个对象变量相等。<br>
而如果使用全等运算符（===），这两个对象变量一定要指向某个类的同一个实例（即同一个对象）。

## 命名空间
### 定义命名空间
将全局的非命名空间中的代码与命名空间中的代码组合在一起，只能使用大括号形式的语法。全局代码必须用一个不带名称的namespace语句加上大括号括起来。
```php
<?php
declare(encoding='UTF-8');
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```
### 使用命名空间
```php
<?php
namespace Foo\Bar;
include 'file1.php';

const FOO = 2;
function foo() {}
class foo
{
    static function staticmethod() {}
}

/* 非限定名称 */
foo(); // 解析为 Foo\Bar\foo resolves to function Foo\Bar\foo
foo::staticmethod(); // 解析为类 Foo\Bar\foo的静态方法staticmethod。resolves to class Foo\Bar\foo, method staticmethod
echo FOO; // resolves to constant Foo\Bar\FOO

/* 限定名称 */
subnamespace\foo(); // 解析为函数 Foo\Bar\subnamespace\foo
subnamespace\foo::staticmethod(); // 解析为类 Foo\Bar\subnamespace\foo,
                                  // 以及类的方法 staticmethod
echo subnamespace\FOO; // 解析为常量 Foo\Bar\subnamespace\FOO
                                  
/* 完全限定名称 */
\Foo\Bar\foo(); // 解析为函数 Foo\Bar\foo
\Foo\Bar\foo::staticmethod(); // 解析为类 Foo\Bar\foo, 以及类的方法 staticmethod
echo \Foo\Bar\FOO; // 解析为常量 Foo\Bar\FOO
?>
```
### 全局空间
如果没有定义任何命名空间，所有的类与函数的定义都是在全局空间，与PHP引入命名空间概念前一样。在名称前加上前缀\表示该名称是全局空间中的名称，即使该名称位于其它的命名空间中时也是如此。
```php
<?php
namespace A\B\C;

/* 这个函数是 A\B\C\fopen */
function fopen() { 
     /* ... */
     $f = \fopen(...); // 调用全局的fopen函数
     return $f;
} 
?>
```
### 名称解析
```php
<?php
namespace A;
use B\D, C\E as F;

// 函数调用

foo();      // 首先尝试调用定义在命名空间"A"中的函数foo()
            // 再尝试调用全局函数 "foo"

\foo();     // 调用全局空间函数 "foo" 

my\foo();   // 调用定义在命名空间"A\my"中函数 "foo" 

F();        // 首先尝试调用定义在命名空间"A"中的函数 "F" 
            // 再尝试调用全局函数 "F"

// 类引用

new B();    // 创建命名空间 "A" 中定义的类 "B" 的一个对象
            // 如果未找到，则尝试自动装载类 "A\B"

new D();    // 使用导入规则，创建命名空间 "B" 中定义的类 "D" 的一个对象
            // 如果未找到，则尝试自动装载类 "B\D"

new F();    // 使用导入规则，创建命名空间 "C" 中定义的类 "E" 的一个对象
            // 如果未找到，则尝试自动装载类 "C\E"

new \B();   // 创建定义在全局空间中的类 "B" 的一个对象
            // 如果未发现，则尝试自动装载类 "B"

new \D();   // 创建定义在全局空间中的类 "D" 的一个对象
            // 如果未发现，则尝试自动装载类 "D"

new \F();   // 创建定义在全局空间中的类 "F" 的一个对象
            // 如果未发现，则尝试自动装载类 "F"

// 调用另一个命名空间中的静态方法或命名空间函数

B\foo();    // 调用命名空间 "A\B" 中函数 "foo"

B::foo();   // 调用命名空间 "A" 中定义的类 "B" 的 "foo" 方法
            // 如果未找到类 "A\B" ，则尝试自动装载类 "A\B"

D::foo();   // 使用导入规则，调用命名空间 "B" 中定义的类 "D" 的 "foo" 方法
            // 如果类 "B\D" 未找到，则尝试自动装载类 "B\D"

\B\foo();   // 调用命名空间 "B" 中的函数 "foo" 

\B::foo();  // 调用全局空间中的类 "B" 的 "foo" 方法
            // 如果类 "B" 未找到，则尝试自动装载类 "B"

// 当前命名空间中的静态方法或函数

A\B::foo();   // 调用命名空间 "A\A" 中定义的类 "B" 的 "foo" 方法
              // 如果类 "A\A\B" 未找到，则尝试自动装载类 "A\A\B"

\A\B::foo();  // 调用命名空间 "A\B" 中定义的类 "B" 的 "foo" 方法
              // 如果类 "A\B" 未找到，则尝试自动装载类 "A\B"
?>
```

## 异常处理
```php
<?php
function inverse($x) {
    if (!$x) {
        throw new Exception('Division by zero.');
    }
    return 1/$x;
}

try {
    echo inverse(5) . "\n";
} catch (Exception $e) {
    echo 'Caught exception: ',  $e->getMessage(), "\n";
} finally {
    echo "First finally.\n";
}

try {
    echo inverse(0) . "\n";
} catch (Exception $e) {
    echo 'Caught exception: ',  $e->getMessage(), "\n";
} finally {
    echo "Second finally.\n";
}

// Continue execution
echo "Hello World\n";
?>
```

## 生成器
实现一个xrange() 生成器, 只需要足够的内存来创建 Iterator 对象并在内部跟踪生成器的当前状态，这样只需要不到1K字节的内存。

当一个生成器被调用的时候，它返回一个可以被遍历的对象。当你遍历这个对象的时候(例如通过一个foreach循环)，PHP 将会在每次需要值的时候调用生成器函数，并在产生一个值之后保存生成器的状态，这样它就可以在需要产生下一个值的时候恢复调用状态。<br>
一旦不再需要产生更多的值，生成器函数可以简单退出，而调用生成器的代码还可以继续执行，就像一个数组已经被遍历完了。

yield关键字<br>
yield会返回一个值给循环调用此生成器的代码并且只是暂停执行生成器函数。
```php
<?php
function gen_one_to_three() {
    for ($i = 1; $i <= 3; $i++) {
        //注意变量$i的值在不同的yield之间是保持传递的。
        yield $i;
    }
}

$generator = gen_one_to_three();
foreach ($generator as $value) {
    echo "$value\n";
}
?>
```

## 上下文
[PHP 请求上下文相关总结](https://www.jb51.net/article/210961.htm)
## 内存管理
[参考一](https://www.shangyexinzhi.com/article/4671892.html)<br>
[参考二](https://zhuanlan.zhihu.com/p/343695712)

## 包管理工具
### Composer
[参考官网](https://pkg.phpcomposer.com/)<br>
Composer是PHP用来管理依赖（dependency）关系的工具。可以在项目中声明所依赖的外部工具库（libraries），Composer会安装这些依赖的库文件。
#### 下载
```bash
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" #下载安装脚本 － composer-setup.php － 到当前目录。
```
```bash
php composer-setup.php #执行安装过程。
```
```bash
php -r "unlink('composer-setup.php');" #删除安装脚本。
```
#### 局部安装
上述下载Composer的过程正确执行完毕后，可以将composer.phar文件复制到任意目录（比如项目根目录下），然后通过 php composer.phar 指令即可使用Composer了！
#### 全局安装
全局安装是将Composer安装到系统环境变量PATH所包含的路径下面，然后就能够在命令行窗口中直接执行composer命令了。
```bash
sudo mv composer.phar /usr/local/bin/composer #Mac或Linux系统
```
#### Packagist镜像
##### 镜像用法
有两种方式启用本镜像服务：
* 系统全局配置：即将配置信息添加到Composer的全局配置文件config.json 中。
```bash
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
* 单个项目配置：将配置信息添加到某个项目的composer.json文件中。
打开控制台（Linux、Mac用户），进入项目的根目录（也就是 composer.json文件所在目录），执行如下命令：
```bash
composer config repo.packagist composer https://packagist.phpcomposer.com
```
上述命令将会在当前项目中的composer.json文件的末尾自动添加镜像的配置信息。也可以手工添加：
```json
"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://packagist.phpcomposer.com"
    }
}
```
##### 镜像原理
一般情况下，安装包的数据（主要是zip文件）一般是从github.com上下载的，安装包的元数据是从packagist.org上下载的。<br>
然而，由于众所周知的原因，国外的网站连接速度很慢，并且随时可能被“墙”甚至“不存在”。<br>
“Packagist 中国全量镜像”所做的就是缓存所有安装包和元数据到国内的机房并通过国内的CDN进行加速，这样就不必再去向国外的网站发起请求，从而达到加速 composer install 以及 composer update 的过程，并且更加快速、稳定。因此，即使 packagist.org、github.com 发生故障（主要是连接速度太慢和被墙），仍然可以下载、更新安装包。
##### 解除镜象
如果需要解除镜像并恢复到packagist官方源，请执行以下命令：
```bash
composer config -g --unset repos.packagist
```
执行之后，composer会利用默认值（也就是官方源）重置源地址。<br>
将来如果还需要使用镜像的话，只需要根据前面的“镜像用法”中介绍的方法再次设置镜像地址即可。
#### 基本用法
##### composer.json：项目安装
要开始在项目中使用Composer，只需要一个composer.json文件。该文件包含了项目的依赖和其它的一些元数据。
```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```
require 需要一个包名称（例如 monolog/monolog）映射到包版本（例如 1.0.*）的对象。
##### 安装依赖包
```bash
php composer.phar install
```
把第三方的代码放到一个指定的目录vendor。<br>
如果正在使用Git来管理项目，可能要添加vendor到 .gitignore 文件中。<br>
将创建一个 composer.lock 文件到项目的根目录中。
##### composer.lock：锁文件
在安装依赖后，Composer将把安装时确切的版本号列表写入composer.lock文件。这将锁定该项目的特定版本。<br>
提交应用程序的 composer.lock （包括 composer.json）到版本库中。这是非常重要的，因为 install 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 composer.json 文件中的定义）。如果不存在 composer.lock 文件，Composer 将读取 composer.json 并创建锁文件。<br>
这意味着如果你的依赖更新了新的版本，你将不会获得任何更新。此时要更新你的依赖版本请使用 update 命令。这将获取最新匹配的版本（根据你的 composer.json 文件）并将新版本更新进锁文件。
```bash
php composer.phar update
```
如果只想安装或更新一个依赖，可以白名单它们：
```bash
php composer.phar update monolog/monolog [...]
```
#### 命令行
[参考](https://docs.phpcomposer.com/03-cli.html)
#### composer.json架构
[参考](https://docs.phpcomposer.com/04-schema.html)

[更多详见](https://docs.phpcomposer.com/)

# 注意
## php中字符串和整数比较的操作方法
字符串和整数进行比较的时候，会把字符串转换成整数然后进行比较，转换会从第一个字符开始如果不是整数就转换成0。
## 比较浮点数
要测试浮点数是否相等，要使用一个仅比该数值大一丁点的最小误差值。该值也被称为机器极小值（epsilon）或最小单元取整数，是计算中所能接受的最小的差别值。
## 删除数组的键和重建索引
unset() 函数允许删除数组中的某个键。但要注意数组将不会重建索引。如果需要删除后重建索引，可以用 array_values() 函数。
## php和nginx的关系，为什么需要用到nginx？
nginx：监听端口，将请求转发。
## LNMP
LNMP：Linux+Nginx+MySQL+PHP
# GO核心编程

## 一  基本结构与基本类型

### 1.1 文件名、关键字与标识符

#### 1.1.1  文件名

> ```go
> .go后缀的文件名均为小写 多部分组成需用_下划线
> ```

#### 1.1.2  关键字

|   default   |  break   |  func  | interface | select |
| :---------: | :------: | :----: | :-------: | :----: |
|    defer    |   case   |   go   |    map    | struct |
|    else     |   chan   |  goto  |  package  | switch |
| fallthrough |  const   |   if   |   range   |  type  |
|     for     | continue | import |  return   |  var   |

#### 1.1.3  标识符

|  append   |    bool    |  byte  |  cap  |  close  | complex |
| :-------: | :--------: | :----: | :---: | :-----: | :-----: |
| complex64 | complex128 | uint16 | copy  |  false  | float32 |
|  float64  |    imag    |  int   | int8  |  int16  | uint32  |
|   int32   |   int64    |  iota  |  len  |  make   |   new   |
|    nil    |   panic    | uint64 | print | println |  real   |
|  recover  |   string   |  true  | uint  |  uint8  | uintptr |

### 1.2  常量

#### 1.2.1  常量定义格式

```go
const identifier [type] = value
```

- 显式类型定义： const b string="abc"
- 隐式类型定义： const b="abc"

*注：常量的值必须是能够在编译时就能够确定的*

#### 1.2.2  常量枚举

```go
const(
	Unknown=0
    Female=1
    Male=2
)
```

iota使用

```go
const(
	Unknown=iota  //Unknown=0
    Female=iota	  //Female=1
    Male=iota     //Male=2
)
//第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1，并且没有赋值的常量默认会应用上一行的赋值表达式
const(
	a=iota       //a=0
    b            //b=1
    c            //c=2
)
//每遇到一个新的常量块或单个常量声明时 iota被置为0
```

### 1.3  变量

#### 1.3.1 变量定义格式

``` go
var identifier [type]=value
```

示例

```go
var a int
var b bool
var str string
//或
var (
	a int
    b bool
    str string
)
```

变量短命名法(函数体内局部变量尽量使用短命法)

```go
a:=1
b:=true
str:="abc"
```

#### 1.3.2  值类型与引用类型

>```go
>值类型：int、float、bool 、string 等基本类型，数组、结构体
>```

> ```go
> 引用类型：指针、数组切片、map、chanel（通道）、接口
> ```

### 1.4 基本类型和运算符

#### 1.4.1 基本类型

```txt
 整数型
        int8（-128 -> 127）
        int16（-32768 -> 32767）
        int32（-2,147,483,648 -> 2,147,483,647）
        int64（-9,223,372,036,854,775,808 -> 9,223,372,036,854,775,807）
 无符号整数型
        uint8（0 -> 255）      byte
        uint16（0 -> 65,535）
        uint32（0 -> 4,294,967,295） rune
        uint64（0 -> 18,446,744,073,709,551,615）
  浮点型
        float32（+- 1e-45 -> +- 3.4 * 1e38）
        float64（+- 5 * 1e-324 -> 107 * 1e308）
		注：float32 精确到小数点后 7 位，float64 精确到小数点后 15 位。math函数中传参类型多为 
		float64，go中无double型
  布尔型
        bool
  字符串型
  		string
  复数型
        complex64(32位实数和虚数)
        complex128(64位实数和虚数)
```

格式化字符

```go
%d:格式化整数 %g:格式化浮点型(%f:浮点数输出,%e:科学计数法输出)  %0nd:长度为n的整型 %.nf:保留n个小数
```

类型别名

> ```go
> type  TZ   int      var a,b TZ
> ```

#### 1.4.2 运算符

位运算

```go
&   与运行
|	或运算
^	异或运算
&^	位清除(指定位置上的值设置为0)
<<  位左移
>>  位右移
```

逻辑运算符

```go
==  !=  >=  <=  >   <  && ||
```

算术运算符

运算符优先级

```go
优先级 	运算符
 7 		   ^ !
 6 		   * / % << >> & &^
 5 		   + - | ^
 4 		   == != < <= >= >
 3 		   <-
 2 		   &&
 1 		   ||
```

### 1.5  字符串

```
strings包：对string字符串进行处理 
strconv包：对字符串类型进行转换
```

#### 1.5.1 字符串里转义字符及拼接

转义字符

- `\n`：换行符
- `\r`：回车符
- `\t`：tab 键
- `\u` 或 `\U`：Unicode 字符
- `\\`：反斜杠自身

字符串拼接

```go
package main

func main(){
    //方式1： +
    str1:="abc"
    str2:="def"
    fmt.Println(str1+str2)   
    
    //方式2： strings.Join(str []string,sep string) string    
  	str=[]string{"a","c","d","e"}
    fmt.Println(strings.Join(str,""))  //abcbdbe
    
    
    //方式3：效率更高
   	var buffer bytes.Buffer
	buffer.WriteString("Hello")
	buffer.WriteString("World")
	fmt.Println(buffer.String())
}
```

#### 1.5.2 strings包

```txt
常用strings包内方法：   
	strings.HasPrefix(s, prefix string) bool        字符串s是否以prefix前缀  返回bool型
    strings.HasSuffix(s, suffix string) bool        字符串s是否以suffix后缀
    strings.Contains(s, substring string) bool      字符串s是否包含substring
    strings.Index(s, str string) int				返回字符串str出现在s处的索引位置
    strings.LastIndex(s, str string) int			返回字符串str最后一次出现在s处的索引位置
    strings.IndexRune(s string, r rune) int 		非ASCII在字符串中出现的位置
    strings.Replace(str,old,new,n)string 			将str字符串中老的前n个字符串替换成新的字符串
    strings.Count(s,str string)	int					统计字符串s中出现的非重叠次数
    strings.Repeat(s, count int) string 			字符串重复次数
    strings.ToLower(s)	string						字符串小写修改
	strings.ToUpper(s)	string	   					字符串大写修改
    strings.TrimSpace(s) string						剔除字符串中开头和结尾的空白符号
    strings.Trim(s, str string)                     剔除字符串中开头和结尾含str的字符串
```

#### 1.5.3  strconv包

```txt
strconv.Itoa(i int) string  			整数转换为字符串
strconv.FormatFloat(f float64, fmt byte, prec int, bitSize int) string 浮点数转位字符串
		- fmt 表示格式（其值可以是 'b'、'e'、'f' 或 'g'）
		- prec 表示精度
		- bitSize 则使用 32 表示 float32，用 64 表示 float64。
strconv.Atoi(s string) (i int, err error)		 将字符串转换为 int 型。
strconv.ParseFloat(s string, bitSize int) (f float64, err error)  将字符串转换为 float64 型
```

### 1.6  时间和日期

```go
package main

import (
	"fmt"
	"time"
)
func main(){
	t:=time.Now()
	fmt.Printf("%02d.%02d.%04d",t.Day(),t.Month(),t.Year())
}
```

### 1.7  指针

```go
package main
import "fmt"
/*
	指针
		 定义指针变量：var p *int 指向int型的指针
         p=&i  指向变量i的地址, &取地址符
		 指针的格式化标识符:%p
 */
func main(){
	var p *int
	i:=50
	p=&i
    *p=100
	fmt.Println(p,*p,i) // 0xc00000a0b8 100 100
}
```

*注：Go语言中指针不支持指针运算*

## 二  控制结构

```txt
- if-else 结构
- switch 结构
- select 结构，用于 channel 的选择
- for结构
```

### 2.1 if-else结构

```go
package main
import(
	"fmt"
    "strconv"
)
func main(){
    a:="abc"
	if a,err:=strconv.Atoi(a); err!=nil{  //测试多返回值函数的错误
		fmt.Println(err)
	}else{
		fmt.Println(a)
	}
}
```

### 2.2  switch结构 

```go
switch var1 {
	case val1:
		...
	case val2:
		...
	default:
		...
}
var1可以是任意类型,case从上至下逐一比对，直到匹配某个case或default为止，结束 每条case不需break
若case1执行完想继续执行case2，可使用fallthrough

func main(){
    i:=1
	switch i {
	case 1:
		fmt.Println(1)
		fallthrough
	case 2:
		fmt.Println(2)
	default:
		fmt.Println(3)
	}
}	
输出：
1
2
```

### 2.3  for结构

```go
var buffer bytes.Buffer
func main(){
    str:="G"
	for i:=0;i<25;i++{
		if i==0{
			fmt.Println(str)
		}else{
            buffer.WriteString(str)
            fmt.Println(buffer.String())
		}
	}
   	str="hello world"
	fmt.Println(len(str))
    for i,val:=range str{           //for-range格式打印,i:index,v:value
		fmt.Printf("%d,%c\n",i,val)
	}
}
```

### 2.4  continue和break

```txt
continue：只用于for结构
break：只退出最内层for循环
```

## 三  函数

```txt
普通函数
匿名函数
方法
```

### 3.1  函数参数传递类型

```txt
值传递
引用传递 ：切片、map、接口、通道
```

```go
package main
import "fmt"

func Multiply(a, b int, reply *int) {
    *reply = a * b
}

func main() {
    n := 0
    reply := &n
    Multiply(10, 5, reply)
    fmt.Println("Multiply:", *reply) // Multiply: 50
}
```

### 3.2  传递变长参数

> ```go
> 函数的最后一个参数是采用 ...type 的形式，那么这个函数就可以处理一个变长的参数
> ```

```go
package main
import "fmt"

func main(){
    fmt.Println(min(2,3,1,7,8,10))
    slice := []int{7,9,3,5,1}
	x:= min(slice...)   //slice...展开
	fmt.Println(x)
}

func min(s ...int) int{
	if len(s)==0{
		return 0
	}
	minValue:=s[0]
	for i:=1;i<len(s);i++{
		if s[i]<minValue{
			minValue=s[i]
		}
	}
	return minValue
}
```

若变长参数类型没有被指定，则可以使用默认的空接口

```go
func main(){
    TypeCheck(1.23,2,"str",true)
}


func TypeCheck(values ...interface{}){
	for _,val:=range values{
		switch val.(type){
		case int:
			fmt.Println("The type is int,the value is:",val)
		case float64:
			fmt.Println("The type is int,the value is:",val)
		case string:
			fmt.Println("The type is int,the value is:",val)
		case bool:
			fmt.Println("The type is int,the value is:",val)
		default:
			fmt.Println("")
		}
	}
}
```

### 3.3  defer

```txt
defer
    将函数推迟到最外层执行，当多个defer时采用逆序执行，类似于栈
defer常用的使用场景
    关闭文件流  defer file.Close()
    解锁一个加锁程序 mu.Lock()  defer mu.Unlock()
    打印最终报告  printHeader() defer printFooter()
    关闭数据库连接 defer disconnectFromDB()
```

```go
package main
import "fmt"

func main() {
	doDBOperations()
}

func connectToDB() {
	fmt.Println("ok, connected to db")
}

func disconnectFromDB() {
	fmt.Println("ok, disconnected from db")
}

func doDBOperations() {
	connectToDB()
	fmt.Println("Defering the database disconnect.")
	defer disconnectFromDB() //function called here with defer
	fmt.Println("Doing some DB operations ...")
	fmt.Println("Oops! some crash or network error ...")
	fmt.Println("Returning from function here!")
	return //terminate the program
}
//输出
ok, connected to db
Defering the database disconnect.
Doing some DB operations ...
Oops! some crash or network error ...
Returning from function here!
ok, disconnected from db
```

### 3.4  内置函数

```txt
close  	             用于管道通信,用于关闭通信
len	                 用于返回某个类型的长度或数量（字符串、数组、切片、map 和管道）
cap 	             是容量的意思，用于返回某个类型的最大容量（只能用于切片和 map）
new、make             new 和 make 均是用于分配内存：new 用于值类型和用户定义的类型，如自定义结构体；
                     make 用于内置引用类型（切片、map 和管道）
copy、append         用于复制和连接切片
panic、recover       两者均用于错误处理机制
print、println       底层打印函数（详见第 4.2 节），在部署环境中建议使用 fmt 包
complex、real imag   用于创建和操作复数
```

### 3.5  递归函数

#### 3.5.1 斐波那契数列

```go
func fibo(n int) int{
	if n<=1{
		return 1
	}else{
		return  fibo(n-1)+fibo(n-2)
	}
}
```

#### 3.5.2 10-1打印

```go
func print10to1(n int) {
	if n==1{
		fmt.Println(n)
		return
	}else  if n<=10{
		fmt.Println(n)
		print10to1(n-1)
	}
}
```

#### 3.5.3 阶乘

```go
func nMulti(n int) int{
	if n==1{
		return n
	}else{
		return n*nMulti(n-1)
	}
}
```

### 3.6  匿名函数

> 将函数赋给一个变量，再通过变量来调用匿名函数

```go
//匿名函数使用方式1：
a:=func(n1,n2 int) int{
    return n1-n2
}
res:=a(10,30)
fmt.Println(res)
//匿名函数使用方式2：
res:=func(n1,n2 int) int{
    return n1-n2
}(10,30)
fmt.Println(res)
```

### 3.7  闭包

> ```go
> 闭包就是一个函数和与其相关的引用环境组合成的一个整体
> ```

```go
package main

import "fmt"

func main(){
    for i:=0;i<3;i++{
        defer func(){
            fmt.Println(i)      //输出结果为3,3,3
        }()
    }
    
    for i:=0;i<3;i++{
        defer func(val int){
            fmt.Println(val)   //输出为2,1,0
        }(i)
    }
}
```

*注：闭包对捕获的外部变量并不是按值方式访问，而是以引用的方式访问*

闭包举例：

```go
package main

import "fmt"

func main() {
	var f = Adder()
	fmt.Print(f(1), " - ")
	fmt.Print(f(20), " - ")
	fmt.Print(f(300))
}

func Adder() func(int) int {
	var x int
	return func(delta int) int {
		x += delta
		return x
	}
}
//输出结果
1-21-321
函数Adder()被赋值为f，类型为func(int) int,也可理解为f是指向func(delta int)int{}函数的地址
```

闭包理解：

* 可以这么理解，闭包是类，函数是操作，n是字段，函数和n构成闭包
* 反复调用函数时，字段只初始化一次，因此每调用一次就进行累加

练习：用闭包实现斐波那契数列

```go
package main

import "fmt"

func main(){
    f:=fibo()
	for i:=0;i<10;i++{
		fmt.Println(f(i))
	}
}
func fibo() func(int) int{
    var f1,f2 int=1,1
    return func(n int) int{
        if n<=1{
            return 1
        }
        f1,f2=f2,f1+f2
        return f2
    }
}
```

### 3.8  计算函数执行时间

```go
package main

import (
	"fmt"
	"time"
)
func main(){
	start := time.Now()
	fmt.Println(start)
    
	...some code...
    
	end := time.Now()
	fmt.Println(end)
	delta:=end.Sub(start)
	fmt.Println(delta)
}
```

### 3.9  使用内存缓存来提升性能

```go
package main

const LIM=50
var fiboTemp [LIM]uint64
func main(){
    for i:=0;i<LIM;i++{
        fmt.Println(fibo(i))
    }
}
func fibo(n int) (res uint64){
    if fiboTemp[n]!=0{
        return fiboTemp[n]
    }
    if n<=1{
        res=1
    }else{
        res=fibo(n-1)+fibo(n-2)
    }
    fiboTemp[n]=res
    return 
}
```

## 四  数组与切片

### 4.1  数组声明方式

```go
数组 (值类型)
    数组赋值 == 内存拷贝
数组常量定义
    1. var arr=[10]int{1,2,3,4,5}
    2. var arr=[...]int{1,2,3,4,5}         若(...)省略，则arr为切片
    3. var arr=[5]string{1:"aaa",2:"bbb"}

注：一个大数组传递给函数会消耗很多内存。有两种方法可以避免这种现象
	1.  传递数组的指针
	2.  使用数组的切片(常用)
```

### 4.2  切片

```txt
切片是引用类型，所以它们不需要使用额外的内存并且比使用数组更有效率，所以在Go代码中切片比数组更常用
```

切片的申明格式

> ```go
> var identifier []type
> ```

切片的基本特点

```go
多个切片共享原数组数据，且修改数据均指向原数组
    var arr=[6]int
    var slice=arr[2:5]     
	//len=3,cap=4  
    0<=len(slice)<= cap(slice)
    make创建数组切片
	make([]type,len,cap)==new([cap]type)[0:len]

len(slice)指切片的开头至切片的末尾长度
cap(slice)指切片的开头至原数组的末尾长度
```

*1.slice、map以及channel都是golang内建的一种引用类型，三者在内存中存在多个组成部分， 需要对内存组成部分初始化后才能使用，而make就是对三者进行初始化的一种操作方式*

*2. new 获取的是存储指定变量内存地址的一个变量，对于变量内部结构并不会执行相应的初始化操作， 所以slice、map、channel需要make进行初始化并获取对应的内存地址，而非new简单的获取内存地址*

for-range结构

```go
package main
import "fmt"
func main(){
    var arr=[]int{1,2,3,4}
    slice1:=arr[:]
    for i,val:=range arr{
        fmt.Println(val)
    }
}
//其中,若只打印索引：for i:=range arr{},若只打印数值,for _,val:=range arr{}
```

切片重组

```go
//通常切片创建时比相关数组小
slice1 := make([]type, start_length, capacity)
slice1=slice1[0:len(slice1)+1]
切片可以反复扩展，直至占据整个数组，此时len=cap
```

```go
package main
import "fmt"

func main() {
	slice1 := make([]int, 0, 10)
	// load the slice, cap(slice1) is 10:
	for i := 0; i < cap(slice1); i++ {
		slice1 = slice1[0:i+1]
		slice1[i] = i
		fmt.Printf("The length of slice is %d\n", len(slice1))
	}

	// print the slice:
	for i := 0; i < len(slice1); i++ {
		fmt.Printf("Slice at %d is %d\n", i, slice1[i])
	}
}
```

切片的复制(copy)与追加(append)

```txt
如果想增加切片的容量，我们必须创建一个新的更大的切片，将原切片全部拷贝进来(copy)。下面的代码描述了从拷贝切片的 copy 函数和向切片追加新元素的 append 函数。
```

```go
package main
import "fmt"
func main(){
    arrFrom:=[]int{1,2,3}
	arrTo:=make([]int,10,10)
	n:=copy(arrTo,arrFrom)
	s11:=append(arrFrom, 999,999)
    fmt.Println(arrFrom)    //arrFrom={1,2,3}
    fmt.Println(s1)         //arrFrom={1,2,3,999,999}
    fmt.Println(arrTo)		//arrTo={1,2,3,0,0,0,0,0,0,0}
    fmt.Println("The copy num is:",n)  //The copy num is:3
}
```

copy函数返回拷贝的元素个数

> ```go
> copy(dst,src type)  int  返回拷贝的元素个数
> ```

append函数返回追加元素后新的切片

> ```go
> append(s[]T, x ...T) []T
> ```

*其中 append 方法将 0 个或多个具有相同类型 s 的元素追加到切片后面并且返回新的切片；追加的元素必须和原切片的元素同类型。如果 s 的容量不足以存储新增元素，append 会分配新的切片来保证已有切片元素和新增元素的存储。因此，返回的切片可能已经指向一个不同的相关数组了。append 方法总是返回成功，除非系统内存耗尽。*

```go
//切片2,追加到切片1 后面
slice1:=make([]int,5)
slice2:=[]int{1,2,3}
slice1=append(slice1,slice2...)
```

append的常见用法：

```go
1. 将切片 b 的元素追加到切片 a 之后：a = append(a, b...)

2. 复制切片 a 的元素到新的切片 b 上：b := make([]T, len(a))   copy(b, a)

3. 删除位于索引 i 的元素：a = append(a[:i], a[i+1:]...)

4. 切除切片 a 中从索引 i 至 j 位置的元素：a = append(a[:i], a[j:]...)

5. 为切片 a 扩展 j 个元素长度：a = append(a, make([]T, j)...)

6. 在索引 i 的位置插入元素 x：a = append(a[:i], append([]T{x}, a[i:]...)...)

7. 在索引 i 的位置插入长度为 j 的新切片：a = append(a[:i], append(make([]T, j), a[i:]...)...)

8. 在索引 i 的位置插入切片 b 的所有元素：a = append(a[:i], append(b, a[i:]...)...)

9. 取出位于切片 a 最末尾的元素 x：x, a = a[len(a)-1], a[:len(a)-1]

10. 将元素 x 追加到切片 a：a = append(a, x)
```

## 五  Map

### 5.1 map的申明格式

```
map是引用类型，map创建不要用new创建，容易引发空指针，可用make创建
```

> ```go
> var map1 map[keytype]valuetype 
> var map1 map[string]int
> 
> map1=make(map[keytype]valuetype)
> map1=make(map[string]int)
> ```

*注：map中key是键值唯一的可以用==,!=等操作符去比较，因此数组、切片、结构体等不可用于键值、但基本数据类型、指针和接口类型的可以作为键值*

### 5.2  测试键值对存在及元素删除

若只是为了判断键值是否存在，而不关系值大小：

> ```go
> _, ok := map1[key1]    // 如果key1存在则ok == true，否则ok为false
> ```

与if混合使用

> ```go
> if _,ok:=map1[key1];ok{}
> ```

删除键值

> ```go
> delete(map1,key1)
> ```

```go
package main
import "fmt"

func main() {
	var value int
	var isPresent bool

	map1 := make(map[string]int)
	map1["New Delhi"] = 55
	map1["Beijing"] = 20
	map1["Washington"] = 25
	value, isPresent = map1["Beijing"]
	if isPresent {
		fmt.Printf("The value of \"Beijing\" in map1 is: %d\n", value)
	} else {
		fmt.Printf("map1 does not contain Beijing")
	}

	value, isPresent = map1["Paris"]
	fmt.Printf("Is \"Paris\" in map1 ?: %t\n", isPresent)
	fmt.Printf("Value is: %d\n", value)

	// delete an item:
	delete(map1, "Washington")
	value, isPresent = map1["Washington"]
	if isPresent {
		fmt.Printf("The value of \"Washington\" in map1 is: %d\n", value)
	} else {
		fmt.Println("map1 does not contain Washington")
	}
}

//输出结果：
The value of "Beijing" in map1 is: 20
Is "Paris" in map1 ?: false
Value is: 0
map1 does not contain Washington
```

for-range 配套用法

```go
for key,value:=range map1{
    ...
}
```

*注：对map1进行遍历，获得的数据是乱序的*

### 5.3  map排序

```go
package main

import (
	"fmt"
	"sort"
)

/*
	map排序 (针对key键值排序)
 */
func main(){
	map1:=map[string]int{
		"o":1,
		"t":2,
		"b":3,
		"a":4,
		"d":5,
	}
	fmt.Println("排序前：")
	index:=0
	key:=make([]string,len(map1))
	for k,val:=range map1{
		key[index]=k
		index++
		fmt.Printf("key=%s,value=%d\n",k,val)
	}
	sort.Strings(key)
	fmt.Println("排序后：")
	for _,val:=range key{
		fmt.Printf("key=%s,value=%d\n",val,map1[val])
	}
}

//输出结果：
排序前：
key=d,value=5
key=o,value=1
key=t,value=2
key=b,value=3
key=a,value=4
排序后：
key=a,value=4
key=b,value=3
key=d,value=5
key=o,value=1
key=t,value=2
```

## 六  结构体与方法

> ```go
> 结构体是值类型
> ```

### 6.1  结构体定义

```go
 type 类型名 struct {
        字段名 字段类型
        字段名 字段类型
        …
 }
```

* 类型名：标识自定义结构体的名称，在同一个包内不能重复。
* 字段名：表示结构体字段名。结构体中的字段名必须唯一。
* 字段类型：表示结构体字段的具体类型。

```go
type Person struct{
    name string
    city string
    age uint8
}
```

### 6.2  结构体实例化

基本实例化

```go
func main() {
    var p1 Person
    p1.name = "pprof.cn"
    p1.city = "北京"
    p1.age = 18
    fmt.Printf("p1=%v\n", p1)  //p1={pprof.cn 北京 18}
    fmt.Printf("p1=%#v\n", p1) //p1=main.person{name:"pprof.cn", city:"北京", age:18}
}
```

取结构体地址实例化

```go
func main(){
    var p2=new(Person)     //用new实例化结构体，p2是指向该结构体的指针
	p2.age=18
	p2.city="上海"
	p2.name="zhangsan"
    fmt.Printf("p2=%v",p2) //p2=&Person{zhangsan  上海  18}
}
```

### 6.3  构造函数

Go语言的结构体没有构造函数，我们可以自己实现。 例如，下方的代码就实现了一个person的构造函数。 因为struct是值类型，如果结构体比较复杂的话，值拷贝性能开销会比较大，所以该构造函数返回的是结构体指针类型。

```go
func newPerson(name, city string, age int8) *Person {
    return &Person{
        name: name,
        city: city,
        age:  age,
    }
}
```

### 6.4  方法

方法的一般格式

```go
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}
```

* 接收者变量：接收者中的参数变量名在命名时，官方建议使用接收者类型名的第一个小写字母，而不是self、this之类的命名。例如，Person类型的接收者变量应该命名为 p，Connector类型的接收者变量应该命名为c等。
* 接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型。
* 方法名、参数列表、返回参数：具体格式与函数定义相同。

```go
package main
import "fmt"
type Person struct{
    name  string
    City  string
    Age   uint8
}

func(p Person) Dream(){
    fmt.Printf("%s want to practice go\n",p.name)
}

func main(){
    p:=Person{"zhangsan","上海",18}
    p.Dream()
}
```

指针类型的接收者

```go
func(p *Person) Dream(){
    fmt.Printf("%s want to practice go\n",p.name)
}
//对于 setter 方法使用 Set 前缀，对于 getter 方法只使用成员的大写
func (p *Person) Name() string{
	return p.name
}

func(p *Person) SetName(newName string){
	 p.name=newName
}
```

什么时候应该使用指针类型的接收者？

* 需要修改接收者中的值
* 接收者是拷贝代价比较大的大对象
* 保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者

*注：在Go语言中，接收者的类型可以是任何类型，不仅仅是结构体*

### 6.5  结构体中的匿名字段

结构体允许其成员字段在声明时没有字段名而只有类型，这种没有名字的字段就称为匿名字段。

```go
type Person struct {
    string
    int
}

func main() {
    p1 := Person{
        "pprof.cn",
        18,
    }
    fmt.Printf("%#v\n", p1)        //main.Person{string:"pprof.cn", int:18}
    fmt.Println(p1.string, p1.int) //pprof.cn 18
}
```

*注：匿名字段默认采用类型名作为字段名，结构体要求字段名称必须唯一，因此一个结构体中同种类型的匿名字段只能有一个。*

### 6.6  嵌套结构体

```go
type Address struct {
	Province string
	City  string
}

type User struct{
	Name string
	Age uint8
	Address
}

func main(){
    user:=User{}
	user.Name="zhangsan"
	user.Age=18
	user.Province="上海"
	user.City="浦东新区"
	fmt.Printf("%#v",user)
}
```

嵌套结构体字段名冲突(指定具体内嵌结构体字段)

```go
type Address struct {
	Province string
	City  string
	CreateTime string
}
type Email struct {
	Account string
	CreateTime string
}
type User struct{
	Name string
	Age uint8
	Address
	Email
}

func main(){
    user:=User{}
	user.Name="zhangsan"
	user.Age=18
	user.Province="上海"
	user.City="浦东新区"
	user.Address.CreateTime="2021-06-15  16:06:38"    //字段冲突，指定具体内嵌结构体
	user.Email.CreateTime="2021-06-15  16:06:38"	  //字段冲突，指定具体内嵌结构体
	fmt.Printf("user=%#v",user)
}
```

### 6.7  结构体字段的可见性

> ```go
> 结构体中字段大写开头表示可公开访问，小写表示私有（仅在定义当前结构体的包中可访问）
> ```

### 6.8  结构体的序列化

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Student struct{
	ID  int
	Name string
	Gender string
}

type Class struct {
	Title string
	Students []*Student
}

func main(){
	c:=&Class{
		Title:    "9527",
		Students: make([]*Student,0,73),
	}

	for i:=0;i<71;i++{
		stu:=&Student{}
		stu.Name=fmt.Sprintf("stu%03d",i+1)
		stu.ID=i+1
		if i%2==0{
			stu.Gender="男"
		}else{
			stu.Gender="女"
		}
		c.Students=append(c.Students,stu)
	}
	str,err:=jsonMarshal(c)
	if err!=nil{
		fmt.Println("json marshal error :",err)
		return
	}
	fmt.Println(str)
	c1:=&Class{}
	c1,err=unMarshalJson(str)
	if err!=nil{
		fmt.Println("json unmarshal error:",err)
	}
	fmt.Printf("%#v\n",c1)
}

func jsonMarshal(c *Class) (str string,err error) {
	data,err:=json.Marshal(c)
	if err!=nil{
		return "",err
	}
	str=string(data)
	return str,nil
}

func unMarshalJson(str string) (*Class,error){
	c:=&Class{}
	err:=json.Unmarshal([]byte(str),c)
	if err!=nil{
		return nil,err
	}
	return c,nil
}
```

### 6.9  结构体字段标签

```go
type Student struct {
    ID     int    `json:"id"` //通过指定tag实现json序列化该字段时的key
    Gender string 			  //json序列化是默认使用字段名作为key
    name   string 			  //私有不能被json包访问
}

func main() {
    s1 := Student{
        ID:     1,
        Gender: "女",
        name:   "pprof",
    }
    data, err := json.Marshal(s1)
    if err != nil {
        fmt.Println("json marshal failed!")
        return
    }
    fmt.Printf("json str:%s\n", data) //json str:{"id":1,"Gender":"女"}
}
```

### 6.10  String()方法和格式化字符

```go
package main

import (
	"fmt"
	"strconv"
)

/*
	类型String()方法和格式化字符
 */

type TwoElems struct{
	a int
	b int
}
func main(){
	te:=new(TwoElems)
	te.a=12
	te.b=13
	fmt.Printf("two1 is:%v\n",te)
}

func(te *TwoElems) String() string{
	return "("+strconv.Itoa(te.a)+"/"+strconv.Itoa(te.b)+")"
}
```

## 七   接口与反射

### 7.1  接口

>  ```go
>  接口是一种类型
>  ```

接口定义格式

```go
 type 接口类型名 interface{
        方法名1( 参数列表1 ) 返回值列表1
        方法名2( 参数列表2 ) 返回值列表2
        …
 }

接口名称定义规范：
    1、加 er
    2、以 able 结尾
    3、以 I 开头
```

* 接口名：使用type将接口定义为自定义的类型名。Go语言的接口在命名时，一般会在单词后面添加er，如有写操作的接口叫Writer，有字符串功能的接口叫Stringer等。接口名最好要能突出该接口的类型含义。
* 方法名：当方法名首字母是大写且这个接口类型名首字母也是大写时，这个方法可以被接口所在的包（package）之外的代码访问。
* 参数列表、返回值列表：参数列表和返回值列表中的参数变量名可以省略。

接口实现

> ```go
> 一个类型只要全部实现了接口类型中的全部方法，那么就实现了这个接口。换言之，接口就是一个需要实现的方法列表
> ```

```go
package main
type Shaper interface {
	Area() float64
	Perimeter() float64
}
type Square struct{
	side float64
}

type Rectangle struct{
	length,width float64
}
func main(){
	var shaper Shaper
	sq:=new(Square)
	sq.side=5.4

	shaper=sq
	fmt.Printf("side is %f , area is %f\n",sq.side,shaper.Area())
	fmt.Printf("side is %f , perimeter is %f\n",sq.side,shaper.Perimeter())

	square:=&Square{6}
	rectangle:=Rectangle{3,4}
	shapers:=[]Shaper{square,rectangle}
	for _,val:=range shapers{
		fmt.Printf("area is %f\n",val.Area())
		fmt.Printf("perimeter is %f\n", val.Perimeter())
	}

}

func (sq *Square) Area() float64{
	return sq.side*sq.side
}

func (sq *Square) Perimeter() float64{
	return sq.side*4
}

func (rec Rectangle) Area() float64{
	return rec.width*rec.length
}

func (rec Rectangle) Perimeter() float64{
	return (rec.width+rec.length)*2
}
```

### 7.2  类型断言

```txt
类型断言：如何检测和转换接口变量的类型
	断言1：
        	var val interface
			t,ok:=val.(T)
	断言2：
			type-switch
			switch t:val.(type){
			case type1:
			case type2:
			default:
			}
	测试某个值是否实现了一个接口
		t,ok:=val.(interface)
```

type-switch类型断言

```go
type  Square struct{
	side float64
}
type Circle struct{
	radius float64
}
type Shaper interface{
	Area() float64
}

func main(){
	 var areaInf Shaper
	 sq1:=new(Square)
	 sq1.side=5
	 areaInf=sq1

	 if t,ok:=areaInf.(*Square);ok{
	 	fmt.Printf("The areaInf type is Square, Area is %f\n",t.Area())
	 }
	 if t,ok:=areaInf.(*Circle);ok{
		fmt.Printf("The areaInf type is Circle, Area is %f\n",t.Area())
	 }else{
		 fmt.Println("The areaInf type is not Circle")
	 }

	cir:=new(Circle)
	cir.radius=2
	areaInf=cir
    switch t:=areaInf.(type) {
    case *Square:
        fmt.Printf("The areaInf type is Square, Area is %f\n",t.Area())
    case *Circle1:
        fmt.Printf("The areaInf type is Circle, Area is %f\n",t.Area())
    case nil:
        fmt.Printf("The areaInf type is nil")
    default:
        fmt.Printf("no Exception is type")
    }
}

func (sq *Square) Area() float64{
	return  sq.side*sq.side
}

func(ci *Circle) Area() float64{
	return math.Pi*ci.radius*ci.radius
}
```

### 7.3  方法集与接口

使用方法集与接口

* 指针方法可以通过指针调用
* 值方法可以通过值调用
* 接收者是值的方法可以通过指针调用，因为指针会首先被解引用
* 接收者是指针的方法不可以通过值调用，因为存储在接口中的值没有地址

举例:

```go
type Shaper interface{
    Area()  float64
}

type Square struct{
    side float64
}

type Circle struct{
    radius float64
}

func(c Circle) Area() float64{
    return math.Pi*c.radius*c.radius
}

func(s *Square) Area() float64{
    return s.side*s.side
}

func main(){
   var shaper Shaper
	circle:=Circle{radius:2.1}
	shaper=circle
    fmt.Printf("area is %f \n",shaper.Area())   //接收者是值方法可以通过值调用

	circle1:=new(Circle)
	circle1.radius=2
	shaper=circle1
    fmt.Printf("area is %f \n",shaper.Area())   //接收者是值方法可以通过指针调用

	square:=new(Square)
	square.side=4
	shaper=square
    fmt.Printf("area is %f \n",shaper.Area())   //接收者是指针方法可以通过指针调用

	square1:=Square{}
	square1.side=4
    shaper=square1							    //编译出错
    fmt.Printf("area is %f \n",shaper.Area())   //接收者是指针方法不可以通过值调用   
}
```

### 7.4  接口嵌套

```go
type Buffer byte 
type ReadWrite interface {
	Read(b Buffer) bool
	Write(b Buffer) bool
}

type Lock interface {
	Lock()
	Unlock()
}

type File interface {
	ReadWrite
	Lock
	Close()
}
func main(){

}
```

Sorter接口排序实现（冒泡）

```go
package main

import "fmt"

/*
	使用Sorter接口排序 (冒泡排序)
 */

type Sorter interface {
	Len() int
	Less(i,j int) bool
	Swap(i,j int)
}
type IntArr []int

func main(){
	var data IntArr=[]int{2,10,32,23,1,0,-200}
	fmt.Println("排序前:",data)
	sort(data)
	fmt.Println("排序后:",data)
}

func sort(data Sorter){
	blFlag:=false
	for i:=1;i<data.Len();i++{
		for j:=0;j<data.Len()-i;j++{
			if data.Less(j+1,j){
				data.Swap(j,j+1)
				blFlag=true
			}
		}
		if !blFlag{
			return
		}
		blFlag=false
	}
}
func (intArr IntArr) Len() int{
	return len(intArr)
}

func (intArr IntArr) Less(i,j int) bool{
	return intArr[i]<intArr[j]
}

func (intArr IntArr) Swap(i,j int){
	intArr[i],intArr[j]=intArr[j],intArr[i]
}
```

### 7.5  空接口

空接口定义

* 空接口是指没有定义任何方法的接口。因此任何类型都实现了空接口。

* 空接口类型的变量可以存储任意类型的变量。

空接口应用

函数传参：

```go
type specialString string
var whatIsThis specialString ="hello"

func typeSwitch(any interface{}){
    switch v:=any.(type) {
		case bool:
			fmt.Printf("the type is bool , value is %v\n",v)
		case int:
			fmt.Printf("the type is int , value is %v\n",v)
		case string:
			fmt.Printf("the type is string , value is %v\n",v)
		case specialString:
			fmt.Printf("the type is specialString , value is %v\n",v)
		default:
			fmt.Printf("no type ")
		}
}
func main(){
    typeSwitch(whatIsThis)
}
```

空接口作为map值：

```go
var studentInfo = make(map[string]interface{})
studentInfo["name"] = "李白"
studentInfo["age"] = 18
studentInfo["married"] = false
fmt.Println(studentInfo)
```

空接口作为树中data参数

```go
type Node struct{
	left *Node
	right *Node
	data interface{}
}

func NewNode(left,right *Node) *Node{
	return &Node{left,right,nil}
}

func (n *Node)setData(data interface{}){
	n.data=data
}

func main(){
	
	root:=NewNode(nil,nil)
	root.setData("root Data")

	a:=NewNode(nil,nil)
	a.setData(2)

	b:=NewNode(nil,nil)
	b.setData(3)

	root.left=a
	root.right=b	
}
```

### 7.6  反射

类型的反射

```go
func TypeOf(i interface{}) Type      //反射对象的类型
func ValueOf(i interface{}) Value	 //反射对象的值

var x float64=3.4
reflect.TypeOf(x)    //float64
v:=reflect.ValueOf(x)   //3.4

v.Kind 返回底层类型        v.Interface()  还原接口值
```

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var x float64 = 3.4
	fmt.Println("type:", reflect.TypeOf(x))  	  //type: float64
	v := reflect.ValueOf(x)					   
	fmt.Println("value:", v)				  	  //value: 3.4
	fmt.Println("type:", v.Type())			      //type: float64
	fmt.Println("kind:", v.Kind())			      //kind: float64
	fmt.Println("value:", v.Float())      	      //value: 3.4
	fmt.Println(v.Interface())                    //3.4
	fmt.Printf("value is %5.2e\n", v.Interface()) //value is 3.40e+00
	y := v.Interface().(float64)                  
	fmt.Println(y)								  //3.4
}

```

通过反射修改设置值

```go
v.CanSet()  //判断value值能否修改

func main(){
    package main

import (
	"fmt"
	"reflect"
)

func main(){
	var x float64=3.4
	v:=reflect.ValueOf(&x)    //函数传递参数是值拷贝，修改的是副本
	
	fmt.Println(v.CanSet())   //false
	
	v=v.Elem()                //不调用v.Elem值不可设置
	
	fmt.Println(v.CanSet())   //true
	
	v.SetFloat(3.1415)
	fmt.Println(x)            //3.1415
    
```

结构体内部的字段和方法反射

```go
NumField() 				返回结构体内的字段数量
Field(i)   				返回每一个字段的值
Method(n).Call(nil)  	调用签名在结构体上的方法
```

```go
package main

import (
	"fmt"
	"reflect"
)

type NotKnownType struct{
	s1,s2,s3 string
}

var secret interface{}=NotKnownType{"Aao","Go","Oberson"}

func (n NotKnownType) String() string{
	return n.s1+"-"+n.s2+"-"+n.s3
}

func main(){
	value:=reflect.ValueOf(secret)   

	fmt.Println(reflect.TypeOf(secret))  		 //main.NotKnownType
	fmt.Println(value.Kind())					 //struct

	for i:=0;i<value.NumField();i++{
        fmt.Printf("Field %d: %v\t",i,value.Field(i))  
	}										 //Field 0:Aao  Field 1:Go  Field 2:Oberson
	results:=value.Method(0).Call(nil)
	fmt.Println(results)					 //[Aao-Go-Oberson]
}
```

*注：结构中只有被导出字段（首字母大写）才是可设置的*

通过反射修改字段的值

```go
package main

import (
	"fmt"
	"reflect"
)

type T struct {
	A int
	B string
}

func main() {
	t := T{23, "skidoo"}
	s := reflect.ValueOf(&t).Elem()
	typeOfT := s.Type()
	for i := 0; i < s.NumField(); i++ {
		f := s.Field(i)
		fmt.Printf("%d: %s %s = %v\t", i,
			typeOfT.Field(i).Name, f.Type(), f.Interface())
	}										   //0: A int = 23    1: B string = skidoo
	s.Field(0).SetInt(77)
	s.Field(1).SetString("Sunset Strip")
	fmt.Println("t is now", t)                 //t is now {77 Sunset Strip}
}
```

## 八  IO操作

### 8.1  输入输出底层原理

终端其实是一个文件，相关实例如下：

- `os.Stdin`：  标准输入的文件实例，类型为`*File`
- `os.Stdout`：标准输出的文件实例，类型为`*File`
- `os.Stderr`：标准错误输出的文件实例，类型为`*File`

```go
package main

import "os"

func main() {
    var buf [16]byte
    os.Stdin.Read(buf[:])
    os.Stdin.WriteString(string(buf[:]))
}
```

### 8.2  文件操作相关API

- ```go
  func Create(name string) (file *File, err Error)
  ```

  - 根据提供的文件名创建新的文件，返回一个文件对象，默认权限是0666

- ```go
  func NewFile(fd uintptr, name string) *File
  ```

  - 根据文件描述符创建相应的文件，返回一个文件对象

- ```go
  func Open(name string) (file *File, err Error)
  ```

  - 只读方式打开一个名称为name的文件

- ```go
  func OpenFile(name string, flag int, perm uint32) (file *File, err Error)
  ```

  - 打开名称为name的文件，flag是打开的方式，只读、读写等，perm是权限

- ```go
  func (file *File) Write(b []byte) (n int, err Error)
  ```

  - 写入byte类型的信息到文件

- ```go
  func (file *File) WriteAt(b []byte, off int64) (n int, err Error)
  ```

  - 在指定位置开始写入byte类型的信息

- ```go
  func (file *File) WriteString(s string) (ret int, err Error)
  ```

  - 写入string信息到文件

- ```go
  func (file *File) Read(b []byte) (n int, err Error)
  ```

  - 读取数据到b中

- ```go
  func (file *File) ReadAt(b []byte, off int64) (n int, err Error)
  ```

  - 从off开始读取数据到b中

- ```go
  func Remove(name string) Error
  ```

  - 删除文件名为name的文件

### 8.3  打开和关闭文件

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	file,err:=os.Open("./main.go") //打开文件
	
	defer file.Close()  //关闭文件
	if err!=nil{
		fmt.Println("open file failed,err:",err)
		return
	}
}
```

### 8.4  写文件

```go
package main

import (
	"fmt"
	"os"
	"strconv"
)

func main() {
	file,err:=os.Create("./read.txt")
	if err!=nil {
		fmt.Println("create file error,err:",err)
	}
	defer file.Close()

	for i:=0;i<5;i++{
		file.WriteString("00"+strconv.Itoa(i)+"\n")
		file.Write([]byte("ab\n"))
	}
}
```

### 8.5  读文件

> ```go
> 文件读取可以用file.Read()和file.ReadAt()，读到文件末尾会返回io.EOF的错误
> ```

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	file,err:=os.Open("./read.txt")
	if err!=nil{
		fmt.Println("file open failed,err:",err)
		return
	}
	defer file.Close()

	var buf [128]byte
	var content []byte
	for{
		n,err:=file.Read(buf[:])
		if err==io.EOF{
			break
		}

		if err!=nil{
			fmt.Println("file read err,err:",err)
		}

		content=append(content,buf[:n]...)
	}
	fmt.Println(string(content))
}
```

### 8.6  拷贝文件

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main(){
	copyFile("./read_copy.txt","./read.txt")
}

func copyFile(dst,src string){
	file,err:=os.Open(src)      //打开源文件
	defer file.Close()
	if err!=nil{
		fmt.Println("file open failed,err:",err)
		return
	}

	file2,err:=os.Create(dst)  //创建目的文件
	defer file2.Close()
	if err!=nil{
		fmt.Println("file create failed,err:",err)
		return
	}

	buf:=make([]byte,1024)    //字节数组切片，缓冲数组

	for{
		n,err:=file.Read(buf)

		if err==io.EOF{     
			break
		}

		if err!=nil{
			fmt.Println("file read failed,err",err)
			return
		}
		file2.Write(buf[:n])
	}
}
```

### 8.7 bufio包

> ```go
> bufio包实现了带缓冲区的读写，是对文件读写的封装;bufio缓冲写数据
> ```

|    模式     |   含义   |
| :---------: | :------: |
| os.O_WRONLY |   只写   |
| os.O_CREATE | 创建文件 |
| os.O_RDONLY |   只读   |
|  os.O_RDWR  |   读写   |
| os.O_TRUNC  |   清空   |
| os.O_APPEND |   追加   |

使用buffio缓冲读取数据

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
)

func main(){
	writeFile()
	readFile()
}

func writeFile(){
	//参数2：打开模式，所有模式d都在上面
	//参数3: 是权限控制 一般设置成0666
	file,err:=os.OpenFile("./read.txt",os.O_CREATE|os.O_WRONLY,0666)

	if err!=nil{
		fmt.Println("file open failed,err:",err)
		return
	}

	defer file.Close()
	writer:=bufio.NewWriter(file)

	for i:=0;i<10;i++{
		writer.WriteString("hello111\n")
	}
	//刷新缓冲区，强制写出
	writer.Flush()
}

func readFile(){

	file,err:=os.Open("./read.txt")
	defer file.Close()
	if err!=nil{
		fmt.Println("open file failed,err:",err)
		return
	}

	reader:=bufio.NewReader(file)

	for{
		line,_,err:=reader.ReadLine()
		if err==io.EOF{
			break
		}
		if err!=nil{
			return
		}
		fmt.Println(string(line))
	}
}
```

### 8.8  ioutil工具包读写文件

```go
package main

import (
	"fmt"
	"io/ioutil"
)

func main(){
	writeFile()
}

func writeFile(){
	
	 buf,err:=ioutil.ReadFile("./read.txt")  //缓冲数组切片
	 
	 if err!=nil{
	 	fmt.Println("read file failed  error:",err)
	 	return 
	 }
	 err= ioutil.WriteFile("./read_copy.txt",buf,0666)     //0666固定参数
	if err!=nil{
		fmt.Println("write file err:",err)
		return
	}
}
```

## 九  错误处理与测试

### 9.1  案例引出

```go
package main

import (
	"fmt"
)

func main(){
	var arr=[...]int{1,2,3,4,5,6,7}
	for i:=0;i<10;i++{
		fmt.Println(arr[i])
	}
}
// panic: runtime error: index out of range [7] with length 7
```

上诉程序总结

* 默认情况下，当发生错误后panic，程序就会退出(崩溃)
* 当发生错误后，可以捕获到错误，并进行处理，保证程序可以继续执行。还可以在捕获错误后，给管理员或用户发送邮件
* 引出错误处理机制

### 9.2  错误处理机制基本说明

* 1）Go语言追求简洁优雅，所以Go不支持try...catch...finally 这种处理
* 2)  Go中引入的处理方式 <font color='red'>defer , panic ,recover</font>
* 3)  Go中可以在panic中抛出一个异常，然后在defer中通过recover捕获异常，最后对异常进行处理

9.1 案例中使用defer+recover处理

```go
package main

import (
	"fmt"
	"os"
)

func main(){
	var arr=[...]int{1,2,3,4,5,6,7}

	defer func() {
		if err:=recover();err!=nil{
			fmt.Fprint(os.Stdout,"cal error :",err)
		}
	}()
	
	for i:=0;i<10;i++{
		fmt.Println(arr[i])
	}
}
//cal error :runtime error: index out of range [7] with length 7
```

### 9.3  自定义错误

* 1）errors.New("错误说明")
* 2）panic 内置函数，接收一个interface{}类型的值作为参数，可以接收error类型的变量，输出错误信息，并退出程序

```go
func readConf(name string) error{
    if name=="config.ini"{
        return nil
    }else{
        return errors.New("读取文件出错...")
    }
}

func test02(){
    name:="config.ini"
    err:=readConf(name)
    if err!=nil{
        panic(err)
    }
    fmt.Println("继续执行...")
}
```

### 9.4 测试

#### 单元测试

单元测试重点在于发现程序设计或实现的逻辑错误，使问题尽早暴露，便于问题的定位和解决

在`*_test.go`文件中有三种类型的函数，单元测试函数、基准测试函数和示例函数

|   类型   |         格式          |              作用              |
| :------: | :-------------------: | :----------------------------: |
| 测试函数 |   函数名前缀为Test    | 测试程序的一些逻辑行为是否正确 |
| 基准函数 | 函数名前缀为Benchmark |         测试函数的性能         |
| 示例函数 |  函数名前缀为Example  |       为文档提供示例文档       |

go test命令会遍历所有的`*_test.go`文件中符合上述命名规则的函数，然后生成一个临时的main包用于调用相应的测试函数，然后构建并运行、报告测试结果，最后清理测试中生成的临时文件。

golang单元测试对文件名和方法名，参数都有严格要求。

* 文件名必须以xx_test.go命名
* 方法必须是Test[a-z]开头
* 方法参数必须 t *testing.T
* 使用go test执行单元测试

##### 测试函数

测试函数基本格式：

```go
import "testing"
func TestName(t *testing.T){
    ...
}
```

SplitStr函数

```go
package splittest

import "strings"

func SplitStr(s, seq string) []string {
	i := strings.Index(s, seq)
	var result []string
	for i > -1 {
		if i != 0 {
			result = append(result, s[:i]) //需将i=0的情况排除否则容易将空值添加到元素
		}
		s = s[i+1:]
		i = strings.Index(s, seq)
	}
	if !strings.EqualFold(s, "") {
		result = append(result, s)
	}
	return result
}

```

测试函数

```go
package splittest
import (
	"fmt"
	"reflect"
	"testing"
)
func TestSplitStr(t *testing.T) {
	got:=SplitStr("abcd,,,efg,,hi,,",",")
	want:=[]string{"abcd","efg","hi"}
	if !reflect.DeepEqual(got,want){
		fmt.Println(len(got),len(want))
		t.Errorf("except:%v,got:%v is not equal",want,got)
	}
}
```

*注：测试函数必须以Test开头+待测试函数，且该函数首字母大写*

其中*testing.T拥有的方法有

```go
func (c *T) Error(args ...interface{})
func (c *T) Errorf(format string, args ...interface{})
func (c *T) Fail()
func (c *T) FailNow()
func (c *T) Failed() bool
func (c *T) Fatal(args ...interface{})
func (c *T) Fatalf(format string, args ...interface{})
func (c *T) Log(args ...interface{})
func (c *T) Logf(format string, args ...interface{})
func (c *T) Name() string
func (t *T) Parallel()
func (t *T) Run(name string, f func(t *T)) bool
func (c *T) Skip(args ...interface{})
func (c *T) SkipNow()
func (c *T) Skipf(format string, args ...interface{})
func (c *T) Skipped() bool
```

测试覆盖率

> ```go
> go test -cover
> ```

##### 基准测试

基准测试一般用例

```go
func BenchmarkName(b *testing.B){
    // ...
}
```

基准测试以Benchmark为前缀，需要一个`*testing.B`类型的参数b，基准测试必须要执行b.N次，这样的测试才有对照性，b.N的值是系统根据实际情况去调整的，从而保证测试的稳定性。

testing.B拥有的方法

```go
func (c *B) Error(args ...interface{})
func (c *B) Errorf(format string, args ...interface{})
func (c *B) Fail()
func (c *B) FailNow()
func (c *B) Failed() bool
func (c *B) Fatal(args ...interface{})
func (c *B) Fatalf(format string, args ...interface{})
func (c *B) Log(args ...interface{})
func (c *B) Logf(format string, args ...interface{})
func (c *B) Name() string
func (b *B) ReportAllocs()
func (b *B) ResetTimer()
func (b *B) Run(name string, f func(b *B)) bool
func (b *B) RunParallel(body func(*PB))
func (b *B) SetBytes(n int64)
func (b *B) SetParallelism(p int)
func (c *B) Skip(args ...interface{})
func (c *B) SkipNow()
func (c *B) Skipf(format string, args ...interface{})
func (c *B) Skipped() bool
func (b *B) StartTimer()
func (b *B) StopTimer()
```

```go
func BenchmarkSplitStr(t *testing.B){
	for i:=0;i<t.N;i++{
		SplitStr("枯藤老树昏鸦","老")
	}
}
```

命令行运行

> ```go
> go test -bench=SplitStr(待测试函数的名称)
> ```

```go
输出结果：
PS D:\goproject\src\gocode\go_project1\split_test> go test -bench=SplitStr
goos: windows
goarch: amd64
pkg: goproject/src/gocode/go_project1/split_test
cpu: Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz  
BenchmarkSplitStr-8(GOMAXPROCS值)     11174256           106.6 ns/op(每次调用该函数耗费的时间)
PASS
ok      goproject/src/gocode/go_project1/split_test     1.389s
```

> ```go 
> go test -bench=Split -benchmem  (-benchmem参数，内存分配统计数据)
> ```

性能测试函数

```go
func benchmark(b *testing.B, size int){/* ... */}
func Benchmark10(b *testing.B){ benchmark(b, 10) }
func Benchmark100(b *testing.B){ benchmark(b, 100) }
func Benchmark1000(b *testing.B){ benchmark(b, 1000) }
```

以斐波那契数列为例

Fibonaccio函数

```go
package fib

func Fibonaccio(n int) int{
	if n<2{
		return n
	}
	return Fibonaccio(n-1)+Fibonaccio(n-2)
}
```

性能比较函数

```go
package fib

import "testing"

func benchmarkFibnaccio(t *testing.B, size int) {
	for i:=0;i<t.N;i++{
		Fibonaccio(size)
	}
}

func BenchmarkFibnaccio1(b *testing.B) {
	benchmarkFibnaccio(b,1)
}

func BenchmarkFibnaccio2(b *testing.B) {
	benchmarkFibnaccio(b,2)
}


func BenchmarkFibnaccio3(b *testing.B) {
	benchmarkFibnaccio(b,3)
}

func BenchmarkFibnaccio10(b *testing.B) {
	benchmarkFibnaccio(b,10)
}

func BenchmarkFibnaccio20(b *testing.B) {
	benchmarkFibnaccio(b,20)
}

func BenchmarkFibnaccio40(b *testing.B) {
	benchmarkFibnaccio(b,40)
}
```

命令行运行

> ```go
> go test -bench=Fibnaccio
> ```

```go
运行结果
goos: windows
goarch: amd64
pkg: goproject/src/gocode/go_project1/fib
cpu: Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz
BenchmarkFibnaccio1-8           881800518(运行次数)               1.335 ns/op  (每次运行时间)
BenchmarkFibnaccio2-8           274323900              			 4.362 ns/op
BenchmarkFibnaccio3-8           171198456                        10.41 ns/op
BenchmarkFibnaccio10-8           2244517                         587.0 ns/op
BenchmarkFibnaccio20-8             21577                         50192 ns/op
BenchmarkFibnaccio40-8               2                        806505300 ns/op
PASS
ok      goproject/src/gocode/go_project1/fib    11.436s
```

*默认情况下，每个基准测试至少运行1秒。如果在Benchmark函数返回时没有到1秒，则b.N的值会按1,2,5,10,20,50，…增加，并且函数再次运行。*

##### 示例函数

示例函数名以Example为前缀。它们既没有参数也没有返回值。配合文档使用

格式如下：

```go
func ExampleName() {
    // ...
}
```

```go
func ExampleSplitStr() {

	fmt.Println(SplitStr("abcd,efg,,",","))
	//Output:
	//[abcd efg]
}
```

```go
API server listening at: 127.0.0.1:44788
PASS
Process exiting with code: 0
```

#### 压力测试

压力测试在于测试函数的性能

一般格式

> ```go
> func BenchmarkXXX(b *testing.B) { ... }
> ```

go test不会默认执行压力测试的函数，如果要执行压力测试需要带上参数-test.bench，语法:-test.bench="test_name_regex",例如`go test -test.bench=".*"`表示测试全部的压力测试函数

案例：

```go
package div

import "fmt"

func Div(a, b float64) (float64, error) {
	if b == 0 {
		return 0, fmt.Errorf("除数不能为0！")
	}
	return a / b, nil
}
```

```go
package div

import "testing"

func BenchmarkDiv(b *testing.B) {
	for i := 0; i < b.N; i++ {
		Div(4, 5)
	}
}
```

```GO
API server listening at: 127.0.0.1:48938
goos: windows
goarch: amd64
pkg: goproject/src/gocode/go_project1/div
cpu: Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz
BenchmarkDiv-8   	501249363	         2.464 ns/op
PASS
```

## 十  协程与通道

### 10.1  并发与并行

进程和线程

```txt
A. 进程是程序在操作系统中的一次执行过程，系统进行资源分配和调度的一个独立单位。
B. 线程是进程的一个执行实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位。
C.一个进程可以创建和撤销多个线程;同一个进程中的多个线程之间可以并发执行。
```

并发和并行

```
A. 多线程程序在一个核的cpu上运行，就是并发。
B. 多线程程序在多个核的cpu上运行，就是并行。
```

协程和线程

```
协程：独立的栈空间，共享堆空间，调度由用户自己控制，本质上有点类似于用户级线程，这些用户级线程的调度也是自己 
	 实现的。
线程：一个线程上可以跑多个协程，协程是轻量级的线程。
```

*Go语言的并发模型是CSP（Communicating Sequential Processes），提倡通过通信共享内存而不是通过共享内存而实现通信*

### 10.2  协程

协程启动: 只需在函数执行前加go

单个协程启动

```go
package main

func main(){
    go hello()
    fmt.Println("main go routine done")
}
func hello(){
    fmt.Println("hello goroutine...")
}
//输出结果
main go routine done 
```

```
如果不在 main() 中等待，协程会随着程序的结束而消亡
```

```go
package main

func main(){
    go hello()
    fmt.Println("main go routine done")
    time.Sleep(1e9)
}
func hello(){
    fmt.Println("hello goroutine...")
}
//输出结果
main go routine done
hello goroutine...
```

多个协程启动

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup             //sync包

func main() {
	for i:=0;i<10;i++{
		wg.Add(1)                 //开启1个协程，任务数加1
		go hello(i)
	}
	wg.Wait()                     //所有协程任务完成后，释放，否则阻塞
}

func hello(i int){
	defer wg.Done()				  //当前协程任务完成，则任务数减1
	fmt.Printf("第%d个协程执行hello函数\n",i)
}
//输出结果：
第9个协程执行hello函数
第5个协程执行hello函数
第3个协程执行hello函数
第1个协程执行hello函数
第7个协程执行hello函数
第8个协程执行hello函数
第0个协程执行hello函数
第4个协程执行hello函数
第6个协程执行hello函数
第2个协程执行hello函数
```

goroutine与线程

```txt
OS线程(操作系统线程)一般都有固定的栈内存(通常为2MB),一个goroutine的栈在其生命周期开始时只有很小的栈(典型情况下2KB)，goroutine的栈不是固定的，他可以按需增大和缩小，goroutine的栈大小限制可以达到1GB，虽然极少会用到这个大。所以在Go语言中一次创建十万左右的goroutine也是可以的。
```

goroutine调度

GPM是Go语言运行时（runtime）层面的实现，是go语言自己实现的一套调度系统。区别于操作系统调度OS线程

- G很好理解，就是个goroutine的，里面除了存放本goroutine信息外 还有与所在P的绑定等信息。
- P管理着一组goroutine队列，P里面会存储当前goroutine运行的上下文环境（函数指针，堆栈地址及地址边界），P会对自己管理的goroutine队列做一些调度（比如把占用CPU时间较长的goroutine暂停、运行后续的goroutine等等）当自己的队列消费完了就去全局队列里取，如果全局队列里也消费完了会去其他P的队列里抢任务。
- M（machine）是Go运行时（runtime）对操作系统内核线程的虚拟， M与内核线程一般是一一映射的关系， 一个groutine最终是要放到M上执行的

```txt
P与M一般也是一一对应的。他们关系是： P管理着一组G挂载在M上运行。当一个G长久阻塞在一个M上时，runtime会新建一个M，阻塞G所在的P会把其他的G 挂载在新建的M上。当旧的G阻塞完成或者认为其已经死掉时 回收旧的M。

P的个数是通过runtime.GOMAXPROCS设定（最大256），Go1.5版本之后默认为物理线程数。 在并发量大的时候会增加一些P和M，但不会太多，切换太频繁的话得不偿失。

单从线程调度讲，Go语言相比起其他语言的优势在于OS线程是由OS内核来调度的，goroutine则是由Go运行时（runtime）自己的调度器调度的，这个调度器使用一个称为m:n调度的技术（复用/调度m个goroutine到n个OS线程）。 其一大特点是goroutine的调度是在用户态下完成的， 不涉及内核态与用户态之间的频繁切换，包括内存的分配与释放，都是在用户态维护着一块大的内存池， 不直接调用系统的malloc函数（除非内存池需要改变），成本比调度OS线程低很多。 另一方面充分利用了多核的硬件资源，近似的把若干goroutine均分在物理线程上， 再加上本身goroutine的超轻量，以上种种保证了go调度方面的性能。
```

### 10.3  协程间的信道

channel分无缓冲通道和带缓冲通道。其中，无缓冲通道也称同步通道，通道的接收和发送不在同一个协程上进行

通道定义

> ```go
> var identifier chan datatype
> ```

channel基本特点

* 作用：用于协程之间通信，通道服务于通信的两个目的：1)值的交换，2)同步的

* 结构：FIFO(先进先出)

* 数据类型：引用类型   make创建

* 通用操作符：`<-`

  ```go
  ch<-int1    表示:用通道ch接收变量int1
  int2:=<-ch  表示:变量int2从通道ch(一元运算的前缀操作符)接收数据
  <-ch        表示:单独调用获取通道的下一个值
  
  var send_only chan<- int 		// channel can only receive data
  var recv_only <-chan int		// channel can only send data
  ```

* 1、无缓存的Channel上的发送操作总在对应的接收操作完成前发生
* 2、在同一个Goroutine上执行2个操作很容易导致<font color='red'>死锁</font>
* 3、close(chan) 关闭通道，接收者则会收到Channel返回的初值

```go
package main

import (
	"fmt"
)

var blFlag=make(chan bool)      //make创建通道实例
func main(){
	ch:=make(chan int)
	go sendData(ch)
	go recvData(ch)
	<-blFlag

}

func sendData(ch  chan<-int){
	ch<-1
	ch<-2	
	close(ch)
}

func recvData(ch <-chan int){
	for{
		if val,ok:=<-ch;ok{
			fmt.Println(val)
		}else{
			blFlag<-true
		}
	}
}
```

练习1：素数筛(输出0-n中素数)

```go
package main

import "fmt"

func main(){
	printPrime(1000)
}

func generateNum() (in chan int){
	in=make(chan int)
	go func() {
		for i:=2;;i++{
			in<-i
		}
	}()
	return in
}


func filterPrime(in chan int,prime int) (out chan int){
	out=make(chan int)
	go func() {
		var input int
		for{
			input=<-in
			if input%prime!=0{
				out<-input
			}
		}
	}()
	return out
}

func printPrime(n int){
	ch:=generateNum()
	for i:=0;;i++{
		prime:=<-ch
		if prime>n{
			break
		}
		ch=filterPrime(ch,prime)
		fmt.Printf("0-%d中第%d个素数是%d\n",n,i+1,prime)
	}
}
```

练习2：改进斐波那契数列

```go
package main

import "fmt"

func main(){
	x:=fibo()
	for i:=0;i<40;i++{
		fmt.Println(<-x)
	}
}
//斐波那契数列:f1,f2,f1+f2   其中a用于存储f2, b用于存储f1, c用于存储f1+f2
func dup33(in chan int) (chan int,chan int,chan int){
	a,b,c:=make(chan int,2),make(chan int,2),make(chan int,2)
	go func() {
		for{
			x:=<-in
			a<-x
			b<-x
			c<-x
		}
	}()
	return a,b,c
}

func fibo() (chan int){
	x:=make(chan int,2)
	a,b,out:=dup33(x)

	go func(){
		x<-0
		x<-1
		<-a
		for{
			x<-<-a+<-b
		}
	}()
	<-out
	return out
}
```

练习3：计算随机数每位数相加

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

type Job struct {
	Id int
	RandInt int
}

type Result struct {
	job *Job
	sum int
}

func main(){
	jobChan:=make(chan *Job,128)
	resultChan:=make(chan *Result,128)
	go func() {
		var id int
		for{
			id++
			job:=new(Job)
			job.Id=id
			job.RandInt=rand.Int()
			jobChan<-job
		}
	}()
	createPool(64,jobChan,resultChan)

	go func() {
		for {
			result:=<-resultChan
			fmt.Printf("Job:id=%d,randInt=%d,sum=%d\n",result.job.Id,result.job.RandInt,result.sum)
		}
	}()

	time.Sleep(10*time.Second)
}

func createPool(num int,jobChan chan *Job,resultChan chan *Result){
	for i:=0;i<num;i++{
		go func() {
			for job:=range jobChan{
				rand_num:=job.RandInt
				temp:=0
				sum:=0
				for rand_num!=0{
					temp=rand_num%10
					sum+=temp
					rand_num/=10
				}
				r:=new(Result)
				r.sum=sum
				r.job=job
				resultChan<-r
			}
		}()
	}
}
```

### 10.4  select

```
使用select切换协程
   1)如果都阻塞了，会等待直到其中一个可以处理
   2)如果多个可以处理，随机选择一个
   3)如果没有通道操作可以处理并且写了 default 语句，它就会执行：default 永远是可运行的（这就是准备好 
     了，可以执行）
    select{
    case u:=<-ch1:
    case v:=<-ch2:
    default:
    }
```

```go
package main

import (
	"fmt"
	"time"
)

func main(){
	ch1:=make(chan int)
	ch2:=make(chan int)
	go pumpMulti(ch1)
	go pumpAdd(ch2)
	go sunk(ch1,ch2)
	time.Sleep(10*time.Second)
}

func pumpMulti(ch chan  int){
	for i:=0;;i++{
		ch<-i*2
	}
}

func pumpAdd(ch chan int){
	for i:=0;;i++{
		ch<-i+5
	}
}

func sunk(ch1,ch2 chan int){
	for{
		select {
		case u:=<-ch1:
			fmt.Printf("此时正运行ch1,val=%d\n",u)
		case v:=<-ch2:
			fmt.Printf("此时正运行ch2,val=%d\n",v)
		}
	}
}
```

### 10.5  定时器

```go
package main

import (
	"fmt"
	"time"
)

func main() {
    
    //fmt.Println(time.Now())
	//ticker:=time.NewTicker(5*time.Second)
	//fmt.Println(<-ticker.C)
	//fmt.Println("定时结束")
    
	// 1.获取ticker对象
	ticker := time.NewTicker(2* time.Second)
	i := 0
	// 子协程
	go func() {
		for {
			//<-ticker.C
			i++
			fmt.Println(<-ticker.C)   //2秒到了执行一次
			if i == 5 {
				//停止
				ticker.Stop()
			}
		}
	}()
	time.Sleep(10*time.Second)
}
```

### 10.6  协程和恢复

```go
func server(workChan <-chan *Work) {
    for work := range workChan {
        go safelyDo(work)   // start the goroutine for that work
    }
}

func safelyDo(work *Work) {
    defer func() {
        if err := recover(); err != nil {
            log.Printf("Work failed with %s in %v", err, work)
        }
    }()
    do(work)
}
//如果 do(work) 发生 panic，错误会被记录且协程会退出并释放，而其他协程不受影响.
```

### 10.7  惰性生成器

案例：

```go
package main

import "fmt"
var resume chan int

func main(){
    resume=intergers()
	fmt.Println(generator())
	fmt.Println(generator())
	fmt.Println(generator())
}
func intergers() chan int{
	yield:=make(chan int)
	go func() {
		for i:=0;;i++{
			yield<-i
		}
	}()
	return yield
}
func generator() int{
	return <-resume
}
```

int型通道生成器，通道读取的数据在读取之前就已生成，并不是程序被调用时生成的

惰性生成器

```go
package main

import (
    "fmt"
)

type Any interface{}
type EvalFunc func(Any) (Any, Any)

func main() {
    evenFunc := func(state Any) (Any, Any) {
        os := state.(int)
        ns := os + 2
        return os, ns
    }
    
    even := BuildLazyIntEvaluator(evenFunc, 0)
    
    for i := 0; i < 10; i++ {
        fmt.Printf("%vth even: %v\n", i, even())
    }
}

func BuildLazyEvaluator(evalFunc EvalFunc, initState Any) func() Any {
    retValChan := make(chan Any)
    loopFunc := func() {
        var actState Any = initState
        var retVal Any
        for {
            retVal, actState = evalFunc(actState)
            retValChan <- retVal
        }
    }
    retFunc := func() Any {
        return <- retValChan
    }
    go loopFunc()
    return retFunc
}

func BuildLazyIntEvaluator(evalFunc EvalFunc, initState Any) func() int {
    ef := BuildLazyEvaluator(evalFunc, initState)
    return func() int {
        return ef().(int)
    }
}
```

### 10.8  常见的并发模型

#### 10.8.1  单例模型

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"       //原子操作
)

type singleton struct {

}

var(
	instance *singleton
	initialized uint32
)

func Instance() *singleton{
	if atomic.LoadUint32(&initialized)==1{
		return instance
	}
	mu.Lock()
	defer mu.Unlock()
	if instance==nil{
		defer atomic.StoreUint32(&initialized,1)
		instance=&singleton{}
	}
	return instance
}
```

#### 10.8.2  互斥锁实现同步

```go
func model1(){
	var mu sync.Mutex
	mu.Lock()
	go func(){
		fmt.Println("Hello")
		mu.Unlock()
	}()
	mu.Lock() 
    //第二个mu.Lock()会因为第一个mu.Lock()加锁而造成阻塞，当协程里面执行mu.unlock后才继续占用锁
}
```

#### 10.8.2  无缓冲通道实现同步

```go
func model2(){
	done:=make(chan bool)
	go func(){
		fmt.Println("World")
		done<-true  // close(done) 也可以接收者的值则为默认值
	}()
	<-done
}
```

#### 10.8.3  带缓冲通道

```go
//(等待n个协程任务完成退出)
func model3(){
	done:=make(chan int,10)
	for i:=0;i<cap(done);i++{
		go func(){
			fmt.Println("Hello world")
			done<-1
		}()
	}
	//等待n个线程完成
	for i:=0;i<cap(done);i++{
		<-done
		fmt.Println(i)
	}
}
```

#### 10.8.4  sync.WaitGroup

```go
func model4(n int){
	var sw sync.WaitGroup
	for i:=0;i<n;i++{
		sw.Add(1)   //增加等待事件个数
		go func(){
			fmt.Println("Hello world!")
			sw.Done()      //表示完成一个事件
		}()
		sw.Wait()		   //表示等待所有事件完成
	}
}
```

#### 10.8.5  生产者消费者模型

```go
func model5(){
	ch:=make(chan int,64)
	go producer(2,ch)
	go producer(5,ch)
	go consumer(ch)
}

func consumer(out <-chan int){
	for v:=range out{
		fmt.Println(v)
	}
}
func producer(factor int, in chan<-int){
	for i:=0;;i++{
		in<-i*factor
	}
}
```

#### 10.8.6  发布订阅模型

```go
package main

import (
	"fmt"
	"strings"
	"sync"
	"time"
)

//并发模型6:发布订阅模型
type(
	subscriber chan interface{}  //订阅者为一个管道
	topicFunc func(v interface{}) bool  //主题为一个过滤器
)
type Publisher struct {
	m sync.RWMutex //读写锁
	buffer int  //订阅队列的缓存大小
	timeout time.Duration //发布超时时间
	subscribers map[subscriber] topicFunc  //订阅者信息
}

func main(){
	p:=NewPublisher(100*time.Millisecond,10)
	defer p.Close()
	all :=p.Subscribe()
	golang:=p.SubscribeTopic(func(v interface{}) bool{
		if s,ok:=v.(string);ok{
			return strings.Contains(s,"golang")
		}
		return false
	})
	p.Publish("hello world!")
	p.Publish("hello golang!")
	go func(){
		for msg:=range all{
			fmt.Println("all:",msg)
		}
	}()
	go func(){
		for msg:=range golang{
			fmt.Println("golang:",msg)
		}
	}()

	time.Sleep(3*time.Second)
}

//构建一个发布者对象,可以设置发布超时时间和缓存队列的长度
func NewPublisher(publishTimeout time.Duration,buffer int )*Publisher{
	return &Publisher{
		buffer: buffer,
		timeout:publishTimeout,
		subscribers:make(map[subscriber]topicFunc),
	}
}
//添加一个新的订阅者，订阅全部主题
func (p *Publisher) Subscribe() chan interface{}{
	return p.SubscribeTopic(nil)
}
//添加一个新的订阅者，订阅过滤器晒选后的主题
func (p *Publisher)SubscribeTopic(topic topicFunc) chan interface{}{
	ch:=make(chan interface{},p.buffer)
	p.m.Lock()
	p.subscribers[ch]=topic
	p.m.Unlock()
	return ch
}
//退出订阅
func (p *Publisher) Evict(sub chan interface{}){
	p.m.Lock()
	defer p.m.Unlock()
	delete(p.subscribers,sub)
	close(sub)
}

//发布一个主题
func(p *Publisher) Publish(v interface{}){
	p.m.RLock()
	defer p.m.RUnlock()

	var wg sync.WaitGroup
	for sub,topic:=range p.subscribers{
		wg.Add(1)
		go p.sendTopic(sub,topic,v,&wg)
	}
	wg.Wait()
}
//关闭发布者对象，同时关闭所有的订阅者管道
func (p *Publisher) Close(){
	p.m.Lock()
	defer p.m.Unlock()

	for sub:=range p.subscribers{
		delete(p.subscribers,sub)
		close(sub)
	}
}
//发送主题，可以容忍一定的超时
func (p *Publisher) sendTopic(
	sub subscriber, topic topicFunc, v interface{},wg *sync.WaitGroup,){
	defer wg.Done()
	if topic!=nil && !topic(v){
		return
	}
	select{
	case sub<-v:
	case <-time.After(p.timeout):
	}
}
```

#### 10.8.7  素数筛（改进版）

```go
//当main函数不再使用管道时后台Goroutine有泄露的风险。可通过context包来避免这个问题
func GenerateNumNew(ctx context.Context) chan int{
	ch:=make(chan int)
	go func(){
		for i:=2;;i++{
			select {
			case <-ctx.Done():
				return
			case ch<-i:
			}
		}
	}()
	return ch
}

func PrimeFilterNew(ctx context.Context, ch<-chan int,prime int) chan int{
	out:=make(chan int)
	go func() {
		for{
			if i:=<-ch;i%prime!=0{
				select {
				case <-ctx.Done():
					return
				case out<-i:
				}
			}
		}
	}()
	return out
}
```

## 十一  常见标准包

### 11.1  fmt包

#### 11.1.1  输出

>  ```go
>  Print
>  ```

系列函数会将内容输出到系统的标准输出，区别在于Print函数直接输出内容，Printf函数支持格式化输出字符串，Println函数会在输出内容的结尾添加一个换行符

```go
func Print(a ...interface{}) (n int, err error)
func Printf(format string, a ...interface{}) (n int, err error)
func Println(a ...interface{}) (n int, err error)
```

> ```go
> Fprint
> ```

Fprint系列函数会将内容输出到一个io.Writer接口类型的变量w中，我们通常用这个函数往文件中写入内容。

```go
func Fprint(w io.Writer, a ...interface{}) (n int, err error)
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```

```go
// 向标准输出写入内容
fmt.Fprintln(os.Stdout, "向标准输出写入内容")
fileObj, err := os.OpenFile("./xx.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
if err != nil {
    fmt.Println("打开文件出错，err:", err)
    return
}
name := "枯藤"
// 向打开的文件句柄中写入内容
fmt.Fprintf(fileObj, "往文件中写如信息：%s", name)
```

> ```go
> Sprint
> ```

Sprint系列函数会把传入的数据生成并返回一个字符串

```go
func Sprint(a ...interface{}) string
func Sprintf(format string, a ...interface{}) string
func Sprintln(a ...interface{}) string
```

```go
package main

import "fmt"

func main(){
	s1:=fmt.Sprint("枯藤")
	name:="古藤"
	age:=18
	s2:=fmt.Sprintf("name=%s,age=%d",name,age)
	s3:=fmt.Sprintln("古藤")
	fmt.Println(s1,s2,s3)
}
// 枯藤 name=古藤,age=18 古藤
```

> ```go
> Errorf
> ```

```go
func Errorf(format string, a ...interface{}) error
err := fmt.Errorf("这是一个错误")
```

#### 11.1.2  输入

Go语言fmt包下有fmt.Scan、fmt.Scanf、fmt.Scanln三个函数，可以在程序运行过程中从标准输入获取用户的输入。

> ```go
> fmt.Scan
> ```

```go
func Scan(a ...interface{}) (n int, err error)
```

- Scan从标准输入扫描文本，读取由空白符分隔的值保存到传递给本函数的参数中，换行符视为空白符。
- 本函数返回成功扫描的数据个数和遇到的任何错误。如果读取的数据个数比提供的参数少，会返回一个错误报告原因。

```go
func main() {
    var (
        name    string
        age     int
        married bool
    )
    fmt.Scan(&name, &age, &married)
    fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
}
```

> ```go
> fmt.Scanf
> ```

```go
func Scanf(format string, a ...interface{}) (n int, err error)
```

- Scanf从标准输入扫描文本，根据format参数指定的格式去读取由空白符分隔的值保存到传递给本函数的参数中。
- 本函数返回成功扫描的数据个数和遇到的任何错误。

```go
func main() {
    var (
        name    string
        age     int
        married bool
    )
    fmt.Scanf("1:%s 2:%d 3:%t", &name, &age, &married)
    fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
}
```

> ```go
> fmt.Scanln
> ```

```go
func Scanln(a ...interface{}) (n int, err error)
```

- Scanln类似Scan，它在遇到换行时才停止扫描。最后一个数据后面必须有换行或者到达结束位置。
- 本函数返回成功扫描的数据个数和遇到的任何错误。

```go
func main() {
        var (
            name    string
            age     int
            married bool
        )
        fmt.Scanln(&name, &age, &married)
        fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
    }
```

> ```go
> bufio.NewReader
> ```

```go
func bufioDemo() {
    reader := bufio.NewReader(os.Stdin) // 从标准输入生成读对象
    fmt.Print("请输入内容：")
    text, _ := reader.ReadString('\n') // 读到换行
    text = strings.TrimSpace(text)
    fmt.Printf("%#v\n", text)
}
```

> ```go
> Fscan
> ```

这几个函数功能分别类似于fmt.Scan、fmt.Scanf、fmt.Scanln三个函数，只不过它们不是从标准输入中读取数据而是从io.Reader中读取数据。

```go
func Fscan(r io.Reader, a ...interface{}) (n int, err error)
func Fscanln(r io.Reader, a ...interface{}) (n int, err error)
func Fscanf(r io.Reader, format string, a ...interface{}) (n int, err error)
```

> ```go
> Sscan
> ```

这几个函数功能分别类似于fmt.Scan、fmt.Scanf、fmt.Scanln三个函数，只不过它们不是从标准输入中读取数据而是从指定字符串中读取数据。

```go
func Sscan(str string, a ...interface{}) (n int, err error)
func Sscanln(str string, a ...interface{}) (n int, err error)
func Sscanf(str string, format string, a ...interface{}) (n int, err error)
```

### 11.2  time包

#### 11.2.1  获取时间类型

> ```go
> time.Now()
> ```

#### 11.2.2  获取时间戳

> ```go
> time.Now().Unix()       //秒级时间戳
> time.Now().UnixNano()   //纳秒级时间戳
> ```

#### 11.2.3  时间戳转时间格式

> ```go
> time.Unix(sec int64 ,nsec int64)
> ```

```go
package main

import (
	"fmt"
	"time"
)
func main(){
	now:=time.Now()	
	fmt.Println(now)              //2021-06-21 09:26:53.4424679 +0800 CST m=+0.002992301
    
	timestamp:=now.Unix()
	fmt.Println(timestamp)		  //1624238813

	timeObj:=time.Unix(timestamp,0)
	fmt.Println(timeObj)		  //2021-06-21 09:26:53 +0800 CST
}
```

#### 11.2.4  时间间隔

```go
const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)
```

#### 11.2.5  时间操作

#### Add

我们在日常的编码过程中可能会遇到要求时间+时间间隔的需求，Go语言的时间对象有提供Add方法如下：

```go
func (t Time) Add(d Duration) Time
```

举个例子，求一个小时之后的时间：

```go
func main() {
    now := time.Now()
    later := now.Add(time.Hour) // 当前时间加1小时后的时间
    fmt.Println(later)
}
```

#### Sub

求两个时间之间的差值：

```go
func (t Time) Sub(u Time) Duration
```

返回一个时间段t-u。如果结果超出了Duration可以表示的最大值/最小值，将返回最大值/最小值。要获取时间点t-d（d为Duration），可以使用t.Add(-d)。

#### Equal

```
func (t Time) Equal(u Time) bool
```

判断两个时间是否相同，会考虑时区的影响，因此不同时区标准的时间也可以正确比较。本方法和用t==u不同，这种方法还会比较地点和时区信息。

#### Before

```go
func (t Time) Before(u Time) bool
```

如果t代表的时间点在u之前，返回真；否则返回假。

#### After

```go
func (t Time) After(u Time) bool
```

如果t代表的时间点在u之后，返回真；否则返回假。

### 11.2.6. 定时器

使用time.Tick(时间间隔)来设置定时器，定时器的本质上是一个通道（channel）。

```go
func tickDemo() {
    ticker := time.Tick(time.Second) //定义一个1秒间隔的定时器
    for i := range ticker {
        fmt.Println(i)//每秒都会执行的任务
    }
}
```

### 11.2.7. 时间格式化

时间类型有一个自带的方法Format进行格式化，需要注意的是Go语言中格式化时间模板不是常见的Y-m-d H:M:S而是使用Go的诞生时间2006年1月2号15点04分（记忆口诀为2006 1 2 3 4）。也许这就是技术人员的浪漫吧。

补充：如果想格式化为12小时方式，需指定PM。

```go
func formatDemo() {
    now := time.Now()
    // 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan
    // 24小时制
    fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))
    // 12小时制
    fmt.Println(now.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
    fmt.Println(now.Format("2006/01/02 15:04"))
    fmt.Println(now.Format("15:04 2006/01/02"))
    fmt.Println(now.Format("2006/01/02"))
}
```

#### 解析字符串格式的时间

```go
now := time.Now()
fmt.Println(now)
// 加载时区
loc, err := time.LoadLocation("Asia/Shanghai")
if err != nil {
    fmt.Println(err)
    return
}
// 按照指定时区和指定格式解析字符串时间
timeObj, err := time.ParseInLocation("2006/01/02 15:04:05", "2019/08/04 14:15:20", loc)
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println(timeObj)
fmt.Println(timeObj.Sub(now))
```

### 11.3 flag包

#### 11.3.1  os.Args

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	fmt.Println("开始执行")
	fmt.Println(os.Args)
	for i, val := range os.Args {
		fmt.Printf("args[%d]=%v\n", i, val)
	}
}
```

*os.Args是字符串切片，切片第一个元素保存的是执行文件的名称*

```go
go build main.go
main.exe   a b c d 
//输出结果
开始执行
./main.go a b c d
args[0]=D:\go_vscode\src\gocode\project1\main\main.exe
args[1]=a
args[2]=b
args[3]=c
args[4]=d
```

#### 11.3.2  定义命令行flag参数

##### flag.Type()     

*用 flag.Type()定义的变量均为引用类型*

```go
package main

import (
	"flag"
	"fmt"
)

func main() {

	user := flag.String("u", "", "用户名")

	password := flag.String("pwd", "", "密码")

	host := flag.String("h", "", "ip地址")

	port := flag.Int("p", 3306, "端口号")

	flag.Parse()        //解析命令行 参数

	fmt.Printf("user=%v,password=%v,host=%v,port=%v", *user, *password, *host, *port)	
}
```

命令行输入，结果输出

```go
.\main  -u=root -p=4327  -h=localhost  -pwd=12345
user=root,password=12345,host=localhost,port=4327
```

##### flag.TypeVar()

```go
package main

import (
	"flag"
	"fmt"
)

func main() {

	var (
		user     string
		password string
		host     string
		port     uint
	)

	flag.StringVar(&user, "u", "", "用户名")
	flag.StringVar(&password, "pwd", "", "密码")
	flag.StringVar(&host, "h", "", "主机")
	flag.UintVar(&port, "p", 3306, "端口号")

	flag.Parse()
	fmt.Printf("user=%v,password=%v,host=%v,port=%v", user, password, host, port)
}
```

命令行输入，结果输出

```go
.\main  -u=root   -h=localhost  -pwd=12345 
user=root,password=12345,host=localhost,port=3306
```

#### 11.3.3  flag其他函数

- `flag.Args()`      //返回命令行参数后的其他参数，以[]string类型
- `flag.NArg()  `     //返回命令行参数后的其他参数个数
- `flag.NFlag()`   //返回使用的命令行参数个数

```go
package main

import (
	"flag"
	"fmt"
)

func main() {

	var (
		user     string
		password string
		host     string
		port     uint
	)

	flag.StringVar(&user, "u", "", "用户名")
	flag.StringVar(&password, "pwd", "", "密码")
	flag.StringVar(&host, "h", "", "主机")
	flag.UintVar(&port, "p", 3306, "端口号")

	flag.Parse()
	fmt.Printf("user=%v,password=%v,host=%v,port=%v\n", user, password, host, port)

	fmt.Println(flag.Args())
	fmt.Println(flag.NArg())
	fmt.Println(flag.NFlag())
}
```

命令行输入，结果输出

```go
.\main  -u=root   -h=localhost  -pwd=12345  1 2 3 4 5 6 a b c 
user=root,password=12345,host=localhost,port=3306
[1 2 3 4 5 6  a b c]
9
3
```

### 11.4 Log包

#### 11.4.1  使用Logger

```go
func main() {
    log.Println("这是一条很普通的日志。")
    v := "很普通的"
    log.Printf("这是一条%s日志。\n", v)
    log.Fatalln("这是一条会触发fatal的日志。")
    log.Panicln("这是一条会触发panic的日志。")
}
```

```go
2021/06/21 13:49:00 这是一条很普通的日志。
2021/06/21 13:49:00 这是一条很普通的日志。
2021/06/21 13:49:00 这是一条会触发fatal的日志。
```

*logger会打印每条日志信息的日期、时间，默认输出到系统的标准错误。Fatal系列函数会在写入日志信息后调用os.Exit(1)。Panic系列函数会在写入日志信息后panic*

#### 11.4.2  配置Logger

默认情况下的logger只会提供日志的时间信息，但是很多情况下我们希望得到更多信息，比如记录该日志的文件名和行号等。log标准库中为我们提供了定制这些设置的方法。

```go
 func Flags() int
 func SetFlags(flag int)
```

Flag选项

````go
const (
    // 控制输出日志信息的细节，不能控制输出的顺序和格式。
    // 输出的日志在每一项后会有一个冒号分隔：例如2009/01/23 01:23:23.123123 /a/b/c/d.go:23: message
    Ldate         = 1 << iota     // 日期：2009/01/23
    Ltime                         // 时间：01:23:23
    Lmicroseconds                 // 微秒级别的时间：01:23:23.123123（用于增强Ltime位）
    Llongfile                     // 文件全路径名+行号： /a/b/c/d.go:23
    Lshortfile                    // 文件名+行号：d.go:23（会覆盖掉Llongfile）
    LUTC                          // 使用UTC时间
    LstdFlags     = Ldate | Ltime // 标准logger的初始值
)
````

```go
package main

import (
	"log"
)
func main() {
	log.SetFlags(log.Llongfile | log.Ltime | log.Ldate)
	log.Println("这是一条很普通的日志。")
}
```

输出结果

```go
2021/06/21 13:54:54 d:/go_vscode/src/gocode/project1/main/main.go:9: 这是一条很普通的日志。
```

#### 11.4.3  配置日志前缀

```go
func Prefix() string
func SetPrefix(prefix string)
```

其中Prefix函数用来查看标准logger的输出前缀，SetPrefix函数用来设置输出前缀。

```go
func main() {
    log.SetFlags(log.Llongfile | log.Lmicroseconds | log.Ldate)
    log.Println("这是一条很普通的日志。")
    log.SetPrefix("[pprof]")
    log.Println("这是一条很普通的日志。")
}
```

```go
2021/06/21 14:04:39 d:/go_vscode/src/gocode/project1/main/main.go:10: 这是一条很普通的日志。
[pprof]2021/06/21 14:04:39 d:/go_vscode/src/gocode/project1/main/main.go:12: 这是一条很普通的日志。
```

#### 11.4.4  日志输出文件

> ```go
> func SetOutput(w io.Writer)
> ```

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {
	logFile, err := os.OpenFile("./go_log.log", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
	defer logFile.Close()
	if err != nil {
		fmt.Println("file open failed,err:", err)
		// return
	}
	log.SetOutput(logFile)
	log.SetFlags(log.Llongfile | log.Ltime | log.Ldate)
	log.SetPrefix("[file error:]")
	log.Println("go  log")
}
```

```go
[file error:]2021/06/21 14:24:37 d:/go_vscode/src/gocode/project1/main/main.go:29: go log 
```

#### 11.4.5  创建logger

> ```go
>  func New(out io.Writer, prefix string, flag int) *Logger
> ```

New创建一个Logger对象。其中，参数out设置日志信息写入的目的地。参数prefix会添加到生成的每一条日志前面。参数flag定义日志的属性（时间、文件等等）。

```go
package main

import (
	"log"
	"os"
)

func main() {
	logFile := log.New(os.Stdout, "[NEW]", log.Llongfile|log.Ltime|log.Ldate)
	logFile.Println("这是一条自定义的logger")
}
```

```go
[NEW]2021/06/21 14:32:12 d:/go_vscode/src/gocode/project1/main/main.go:10: 这是一条自定义的logger
```

### 11.5  context包

案例1: 全局变量方式通知关闭子线程

```go
package main

import (
	"fmt"
	"sync"
	"time"
)
var wg sync.WaitGroup
var exit bool
func main() {
	wg.Add(1)
	go work()
	time.Sleep(3*time.Second)
	exit=true
	wg.Wait()
	fmt.Println("over")
}
func work(){
	for{
		fmt.Println("work...")
		time.Sleep(time.Second)
		if exit{
			break
		}
	}
	wg.Done()
}
```

案例2：通道方式关闭子线程

```go
package main

import (
	"fmt"
	"sync"
	"time"
)


var wg sync.WaitGroup
func main() {
	exitChan:=make(chan bool)
	wg.Add(1)
	go work(exitChan)
	time.Sleep(3*time.Second)
	exitChan<-true
	wg.Wait()
	fmt.Println("over")
}

func work(exitChan  chan bool){
Loop:
	for{
		fmt.Println("work...")
		time.Sleep(time.Second)
		select{
		case <-exitChan:
			break Loop
		default:
		}
	}
	wg.Done()
}
```

案例3：当有两个子线程使用context包（官方推荐）

```go
package main

import (
	"context"
	"fmt"
	"sync"
	"time"
)
var wg sync.WaitGroup
func main() {
	ctx,cancel:=context.WithCancel(context.Background())
	wg.Add(1)
	go work(ctx)
	time.Sleep(3*time.Second)
	cancel()
	wg.Wait()
	fmt.Println("over")
}

func work(ctx context.Context){
	go work2(ctx)
Loop:
	for{
		fmt.Println("work...")
		time.Sleep(time.Second)
		select{
		case <-ctx.Done():
			break Loop
		default:
		}
	}
	wg.Done()
}

func work2(ctx context.Context){
LOOP:
	for {
        fmt.Println("worker2")
        time.Sleep(time.Second)
        select {
        case <-ctx.Done(): // 等待上级通知
            break LOOP
        default:
        }
    }
}
```

#### 11.5.1 Context接口

```go
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}
```

#### 11.5.2  Background()和TODO()

#### 11.5.3  With系列函数

##### WithCancel

> ```go
> func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
> ```

WithCancel返回带有新Done通道的父节点的副本。当调用返回的cancel函数或当关闭父上下文的Done通道时，将关闭返回上下文的Done通道，无论先发生什么情况。

```go
func gen(ctx context.Context) <-chan int {
        dst := make(chan int)
        n := 1
        go func() {
            for {
                select {
                case <-ctx.Done():
                    return // return结束该goroutine，防止泄露
                case dst <- n:
                    n++
                }
            }
        }()
        return dst
    }
func main() {
    ctx, cancel := context.WithCancel(context.Background())
    defer cancel() // 当我们取完需要的整数后调用cancel

    for n := range gen(ctx) {
        fmt.Println(n)
        if n == 5 {
            break
        }
    }
}
```

##### WithDeadline

> ```go
> func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
> ```

返回父上下文的副本，并将deadline调整为不迟于d。如果父上下文的deadline已经早于d，则WithDeadline(parent, d)在语义上等同于父上下文。当截止日过期时，当调用返回的cancel函数时，或者当父上下文的Done通道关闭时，返回上下文的Done通道将被关闭，以最先发生的情况为准。

```go
func main() {
    d := time.Now().Add(50 * time.Millisecond)
    ctx, cancel := context.WithDeadline(context.Background(), d)

    // 尽管ctx会过期，但在任何情况下调用它的cancel函数都是很好的实践。
    // 如果不这样做，可能会使上下文及其父类存活的时间超过必要的时间。
    defer cancel()

    select {
    case <-time.After(1 * time.Second):
        fmt.Println("overslept")
    case <-ctx.Done(): 
        fmt.Println(ctx.Err())
    }
}
//context deadline exceeded
```

上面的代码中，定义了一个50毫秒之后过期的deadline，然后我们调用context.WithDeadline(context.Background(), d)得到一个上下文（ctx）和一个取消函数（cancel），然后使用一个select让主程序陷入等待：等待1秒后打印overslept退出或者等待ctx过期后退出。 因为ctx50秒后就过期，所以ctx.Done()会先接收到值，上面的代码会打印ctx.Err()取消原因。

##### WithTimeout

> ```go
> func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
> ```

取消此上下文将释放与其相关的资源，因此代码应该在此上下文中运行的操作完成后立即调用cancel，通常用于数据库或者网络连接的超时控制。

```go
package main

import (
    "context"
    "fmt"
    "sync"

    "time"
)

// context.WithTimeout

var wg sync.WaitGroup

func worker(ctx context.Context) {
LOOP:
    for {
        fmt.Println("db connecting ...")
        time.Sleep(time.Millisecond * 10) // 假设正常连接数据库耗时10毫秒
        select {
        case <-ctx.Done():  // 50毫秒后自动调用
            break LOOP
        default:
        }
    }
    fmt.Println("worker done!")
    wg.Done()
}

func main() {
    // 设置一个50毫秒的超时
    ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond*50)
    wg.Add(1)
    go worker(ctx)
    time.Sleep(time.Second * 5)
    cancel() // 通知子goroutine结束
    wg.Wait()
    fmt.Println("over")
}
```

##### WithValue

> ```go	
> func WithValue(parent Context, key, val interface{}) Context
> ```

WithValue返回父节点的副本，其中与key关联的值为val。

仅对API和进程间传递请求域的数据使用上下文值，而不是使用它来传递可选参数给函数。

所提供的键必须是可比较的，并且不应该是string类型或任何其他内置类型，以避免使用上下文在包之间发生冲突。WithValue的用户应该为键定义自己的类型。为了避免在分配给interface{}时进行分配，上下文键通常具有具体类型struct{}。或者，导出的上下文关键变量的静态类型应该是指针或接口。

```go
package main

import (
    "context"
    "fmt"
    "sync"

    "time"
)
// context.WithValue
type TraceCode string

var wg sync.WaitGroup

func worker(ctx context.Context) {
    key := TraceCode("TRACE_CODE")
    traceCode, ok := ctx.Value(key).(string) // 在子goroutine中获取trace code
    if !ok {
        fmt.Println("invalid trace code")
    }
LOOP:
    for {
        fmt.Printf("worker, trace code:%s\n", traceCode)
        time.Sleep(time.Millisecond * 10) // 假设正常连接数据库耗时10毫秒
        select {
        case <-ctx.Done(): // 50毫秒后自动调用
            break LOOP
        default:
        }
    }
    fmt.Println("worker done!")
    wg.Done()
}

func main() {
    // 设置一个50毫秒的超时
    ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond*50)
    // 在系统的入口中设置trace code传递给后续启动的goroutine实现日志数据聚合
    ctx = context.WithValue(ctx, TraceCode("TRACE_CODE"), "12512312234")
    wg.Add(1)
    go worker(ctx)
    time.Sleep(time.Second * 5)
    cancel() // 通知子goroutine结束
    wg.Wait()
    fmt.Println("over")
}
```

# GO Web编程


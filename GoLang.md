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

##### Add

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

##### Sub

求两个时间之间的差值：

```go
func (t Time) Sub(u Time) Duration
```

返回一个时间段t-u。如果结果超出了Duration可以表示的最大值/最小值，将返回最大值/最小值。要获取时间点t-d（d为Duration），可以使用t.Add(-d)。

##### Equal

```
func (t Time) Equal(u Time) bool
```

判断两个时间是否相同，会考虑时区的影响，因此不同时区标准的时间也可以正确比较。本方法和用t==u不同，这种方法还会比较地点和时区信息。

##### Before

```go
func (t Time) Before(u Time) bool
```

如果t代表的时间点在u之前，返回真；否则返回假。

##### After

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

## 一  Web介绍





```go
200 OK                        //客户端请求成功
400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
403 Forbidden                 //服务器收到请求，但是拒绝提供服务
404 Not Found                 //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error     //服务器发生不可预期的错误
503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常
```



##  二  Go搭建Web服务器



## 三  Http 客户端实现

### 3.1  Response

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)


func main() {
	requestUrl := "http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName?theCityName=上海"

	response, err := http.Get(requestUrl)

	if err != nil {
		fmt.Println("err:", err)
	}

	defer response.Body.Close()                   //关闭响应

	fmt.Println(response.Body)

	if response.StatusCode == 200 {
		r, err := ioutil.ReadAll(response.Body)   //使用ioutil.ReadAll转换io.ReadCloser类型，输出是byte[]

		if err != nil {
			fmt.Println(err)
		}
		fmt.Println(string(r))
	}
}
```

输出

```go
&{[] 0xc000096040 <nil> <nil>}
<?xml version="1.0" encoding="utf-8"?>
<ArrayOfString xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://WebXml.com.cn/">
  <string>直辖市</string>
  <string>上海</string>
  <string>58367</string>
  <string>58367.jpg</string>
  <string>2021/6/24 15:11:20</string>
  <string>24℃/31℃</string>
  <string>6月24日 多云转阴</string>
  <string>东风3-4级</string>
  <string>1.gif</string>
  <string>2.gif</string>
  <string>今日天气实况：气温：31℃；风向/风力：西北风 1级；湿度：41%；紫外线强度：中等。</string>
  <string>中国人民保险感冒指数：少发，感冒机率较低，避免长期处于空调屋中。
健臻·血糖指数：易波动，气温多变，血糖易波动，请注意监测。
穿衣指数：炎热，建议穿短衫、短裤等清凉夏季服装。
洗车指数：较不宜，风力较大，洗车后会蒙上灰尘。
紫外线指数：中等，涂擦SPF大于15、PA+防晒护肤品。
</string>
  <string>23℃/30℃</string>
  <string>6月25日 阴转小雨</string>
  <string>东南风3-4级转南风4-5级</string>
  <string>2.gif</string>
  <string>7.gif</string>
  <string>23℃/25℃</string>
  <string>6月26日 大雨转小雨</string>
  <string>东南风4-5级转3-4级</string>
  <string>9.gif</string>
  <string>7.gif</string>
  <string>上海简称：沪，位置：上海地处长江三角洲前缘，东濒东海，南临杭州湾，西接江苏，浙江两省，北界长江入海，正当我国南北岸线的中部，北纬31°14′，东经121°29′。面积：总面积7823.5平方公里。人口：人口1000多万。上海丰富的人文资源、迷人的城市风貌、繁华的商业街市和欢乐的节庆活动形成了独特的都市景观。游览上海，不仅能体验到大都市中西合壁、商儒交融、八方来风的氛围，而且能感受到这个城市人流熙攘、车水马龙、灯火璀璨的活力。上海在中国现代史上占有着十分重要的地位，她是中国共产党的诞生地。许多震动中外的历史事件在这里发生，留下了众多的革命遗迹，处处为您讲述着一个个使人永不忘怀的可歌可泣的故事，成为包含民俗的人文景观和纪念地。在上海，每到秋祭，纷至沓来的人们在这里祭祀先烈、缅怀革命历史,已成为了一种风俗。大上海在中国近代历史中，曾是风起云涌可歌可泣的地方。在这里荟萃多少风云人物，散落在上海各处的不同住宅建筑，由于其主人的非同寻常，蕴含了耐人寻味的历史意义。这里曾留下许多革命先烈的足迹。瞻仰孙中山、宋庆龄、鲁迅等故居，会使您产生抚今追昔的深沉遐思，这里还有无数个达官贵人的住宅，探访一下李鸿章、蒋介石等人的公馆，可以联想起主人那段显赫的发迹史。</string>
</ArrayOfString>
response:&{Status:200 OK StatusCode:200 Proto:HTTP/1.1 ProtoMajor:1 ProtoMinor:1 Header:map[Cache-Control:[private, max-age=0] Content-Type:[text/xml; charset=utf-8] Date:[Thu, 24 Jun 2021 07:30:24 GMT] Server:[Microsoft-IIS/7.5] Vary:[Accept-Encoding] X-Aspnet-Version:[2.0.50727] X-Powered-By:[ASP.NET]] Body:0xc00009a020 ContentLength:-1 TransferEncoding:[] Close:false Uncompressed:true Trailer:map[] Request:0xc000142000 TLS:<nil>}
response.Body:&{_:[] body:0xc000096040 zr:0xc0000902c0 zerr:<nil>}
response.Header:map[Cache-Control:[private, max-age=0] Content-Type:[text/xml; charset=utf-8] Date:[Thu, 24 Jun 2021 07:30:24 GMT] Server:[Microsoft-IIS/7.5] Vary:[Accept-Encoding] X-Aspnet-Version:[2.0.50727] X-Powered-By:[ASP.NET]]
response.StatusCode:200
response.Status:200 OK
response.Request:&{Method:GET URL:http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName?theCityName=上海 Proto:HTTP/1.1 ProtoMajor:1 ProtoMinor:1 Header:map[] Body:<nil> GetBody:<nil> ContentLength:0 TransferEncoding:[] Close:false Host:www.webxml.com.cn Form:map[] PostForm:map[] MultipartForm:<nil> Trailer:map[] RemoteAddr: RequestURI: TLS:<nil> Cancel:<nil> Response:<nil> ctx:0xc000014098}
response.Cookies:[]
```

### 3.2  客户端的实现

#### 3.2.1 http.NewRequest

先生成http.client -> 再生成 http.request -> 之后提交请求：client.Do(request) -> 处理返回结果，每一步的过程都可以设置一些具体的参数

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

/*
	http.NewRequest：
	1. 先生成http.client
	2. 再生成http.request
	3. 提交请求: client.Do(request)
	4. 处理返回结果，每一步的过程都可以设置一些具体的参数
*/
func main() {
	urlStr := "http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName?theCityName=上海"

	//1、创建client对象
	client := http.Client{}
	//2、创建request
	request, err := http.NewRequest("GET", urlStr, nil)

	if err != nil {
		log.Fatal(err)
	}

	//3、发送请求
	response, err := client.Do(request)

	if err != nil {
		log.Fatal(err)
	}

	defer response.Body.Close()
	//4、处理返回结果
	if response.StatusCode == 200 {
		body, err := ioutil.ReadAll(response.Body)
		if err != nil {
			log.Fatal(err)
		}
		fmt.Println(string(body))
	}

	fmt.Printf("response:%+v\n", response)

	fmt.Printf("response.Body:%+v\n", response.Body)

	fmt.Printf("response.Header:%+v\n", response.Header)

	fmt.Printf("response.StatusCode:%+v\n", response.StatusCode)

	fmt.Printf("response.Status:%+v\n", response.Status)

	fmt.Printf("response.Request:%+v\n", response.Request)

	fmt.Printf("response.Cookies:%+v\n", response.Cookies())
}
```

#### 3.2.2  调用Client的API

client结构自己也有一些发送api的方法，比如client.get,client.post,client.postform..等等。基本上涵盖了主要的http请求的类型，通常不进行什么特殊的配置的话，这样就可以了，其实client的get或者post方法，也是对http.Newerequest方法的封装，里面还额外添加了req.Header.Set("Content-Type", bodyType）

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {
	urlStr := "http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName?theCityName=上海"

	client := http.Client{}

	response, err := client.Get(urlStr)

	if err != nil {
		log.Fatal(err)
	}

	defer response.Body.Close()

	if response.StatusCode == 200 {
		body, err := ioutil.ReadAll(response.Body)
		if err != nil {
			log.Fatal(err)
		}
		fmt.Println(string(body))
	}
	fmt.Printf("response:%+v\n", response)

	fmt.Printf("response.Body:%+v\n", response.Body)

	fmt.Printf("response.Header:%+v\n", response.Header)

	fmt.Printf("response.StatusCode:%+v\n", response.StatusCode)

	fmt.Printf("response.Status:%+v\n", response.Status)

	fmt.Printf("response.Request:%+v\n", response.Request)

	fmt.Printf("response.Cookies:%+v\n", response.Cookies())
}
```

### 3.3 客户端请求

http客户端请求的主要有三个api : `http.Get`,`http.Post`,`http.PostForm`

| API           | 特点                                              |
| ------------- | ------------------------------------------------- |
| http.Get      | 发送get请求                                       |
| http.Post     | post请求提交指定类型的数据                        |
| http.PostForm | post请求提交application/x-www-form-urlencoded数据 |

#### 3.3.1 Get请求

http中的Get源码

```go
func Get(url string) (resp *Response, err error) {
	return DefaultClient.Get(url)
}
```

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)
func main() {
	response, err := http.Get("http://www.baidu.com")

	if err!=nil{
		log.Fatal(err)
	}
	defer response.Body.Close()
	if response.StatusCode==200{
		body,err:=ioutil.ReadAll(response.Body)
		if err!=nil{
			log.Fatal(err)
		}
		fmt.Println(string(body))
	}
}
```

#### 3.3.2 Post请求

##### Post()方法

client对象的post方法

```go
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"net/url"
)


func main() {
	urlStr := "http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName"

	client := http.Client{}

	param:=&url.Values{
		"theCityName":{"上海"},
	}
	requestBody:=bytes.NewBufferString(param.Encode())

	response,err:=client.Post(urlStr,"application/x-www-form-urlencoded",requestBody)

	if err!=nil{
		log.Fatal(err)
	}
	defer response.Body.Close()

	if response.StatusCode==200{
		body, err := ioutil.ReadAll(response.Body)
		
        if err != nil {
            log.Fatal(err)
        }
        fmt.Println(string(body))
	}
}
```

使用http对象的post方法

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"strings"
)
func main() {
	resp, err := http.Post("http://www.webxml.com.cn/WebServices/WeatherWebService.asmx/getWeatherbyCityName",
		"application/x-www-form-urlencoded",
		strings.NewReader("theCityName=苏州"))

		if err != nil {
			fmt.Println(err)
		}
	
		defer resp.Body.Close()
		body, err := ioutil.ReadAll(resp.Body)
		if err != nil {
			// handle error
			log.Fatal(err)
		}
	
		fmt.Println(string(body))
}
```

*使用post方法，要设置`contentType`，设置成”application/x-www-form-urlencoded”，否则post参数无法传递。*

搭建服务端，客户端post请求

服务端程序

```go
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"log"
	"net/http"
)

/*
	服务端代码
*/

func main(){
	server:=http.NewServeMux()
	server.HandleFunc("/login",login)
	http.ListenAndServe(":2021",server)

}
func login(w http.ResponseWriter,r *http.Request){

	r.ParseForm()
	body,err:=ioutil.ReadAll(r.Body)

	if err!=nil{
		log.Fatal(err)
	}
	fmt.Printf("body:%+v\n",string(body))
	fmt.Printf("r:%+v\n",r)
	fmt.Printf("%+v\n",r.Header)
	fmt.Printf("%+v\n",r.Header["Content-Type"])
	fmt.Printf("%+v\n",r.Cookies())

	if len(r.Form["username"])>0{
		username:=r.Form["username"][0]
		fmt.Println("username",username)
	}

	if len(r.Form["password"])>0{
		password:=r.Form["password"][0]
		fmt.Println("password",password)
	}

	io.WriteString(w,"登录成功")
}
```

客户端程序

```go
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"net/url"
)


func main() {
	urlStr := "http://localhost:2021/login"
	param := &url.Values{
		"username":{"root"},
		"password":{"12345"},
	}
	requestBody:=bytes.NewBufferString(param.Encode())

	response,err:=http.Post(urlStr,"application/x-www-form-urlencoded",requestBody)

	if err!=nil{
		log.Fatal(err)
	}

	defer response.Body.Close()

	if response.StatusCode==200{
		if body,err:=ioutil.ReadAll(response.Body);err!=nil{
			fmt.Println(err)
		}else{
			fmt.Println(string(body))
		}
	}
}
```

##### postForm()方法

客户端程序,服务端还用上述程序

```go
package main

/*
	使用PostForm方法	
*/
import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"net/url"
)


func main() {
	urlStr:="http://localhost:2021/login"
	response, err := http.PostForm(urlStr,url.Values{"username":{"root"},"password":{"123456"}})

	if err!=nil{
		log.Fatal(err)
	}

	defer response.Body.Close()

	if response.StatusCode==200{
		body,err:=ioutil.ReadAll(response.Body)
		if err!=nil{
			log.Fatal(err)
		}
		fmt.Println(string(body))
	}
}
```

#### 3.3.3  复杂的请求

有时需要在请求的时候设置头参数、cookie之类的数据，需要使用http.Do方法

设置请求头：

> ```go
> request.Header.Set("Content-Type", "application/x-www-form-urlencoded")
> ```

设置cookie

方式一：通过设置请求头

> ```go
> request.Header.Set("Cookie", "name=hanru")
> ```

方式二：通过request对象，直接调用AddCookie()方法。

AddCookie源码

```go
func (r *Request) AddCookie(c *Cookie) {
    s := fmt.Sprintf("%s=%s", sanitizeCookieName(c.Name), sanitizeCookieValue(c.Value))
    if c := r.Header.Get("Cookie"); c != "" {
        r.Header.Set("Cookie", c+"; "+s)
    } else {
        r.Header.Set("Cookie", s)
    }
}
```

服务端程序

```go
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"log"
	"net/http"
)


func HandleRequest(w http.ResponseWriter,r *http.Request){
	r.ParseForm()

	body,err:=ioutil.ReadAll(r.Body)

	if err!=nil{
		log.Fatal(err)
	}

	fmt.Println("body",string(body))
	fmt.Printf("r:%+v\n",r)
	fmt.Printf("request:hander:%+v\n",r.Header)
	fmt.Printf("cookies:%+v\n",r.Cookies())
	cookid,err:=r.Cookie("userId")
	if err==nil{
		fmt.Println(cookid.Name,cookid.Value)
	}

	if len(r.Form["username"])!=0{
		username:=r.Form["username"][0]
		fmt.Println("username:",username)
	}

	w.Header().Set("Access-Control-Allow-Origin","*")
	w.Header().Set("Access-Control-Allow-Header","*")
	io.WriteString(w,"login success...")
}

func main(){
	http.HandleFunc("/login",HandleRequest)
	err:=http.ListenAndServe(":2000",nil)
	if err!=nil{
		log.Fatal(err)
	}
}
```

客户端程序

```go
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"net/url"
)
func main() {
	urlStr := "http://localhost:2000/login"

	client := http.Client{}

	param:=url.Values{
		"username":{"haiyi"},
	}

	requestBody:=bytes.NewBufferString(param.Encode())

	request,err:=http.NewRequest("POST",urlStr,requestBody)

	if err!=nil{
		log.Fatal(err)
	}

	//方式一：
	//request.Hander.Set("Cookie","hanru")

	//方式二：
	//request.AddCookie(Cookie)

	cookId:=&http.Cookie{Name: "userId",Value: "1234"}
	cookName:=&http.Cookie{Name:"name",Value: "huaiLi"}
	request.AddCookie(cookId)
	request.AddCookie(cookName)

	request.Header.Set("Content-Type","application/x-www-form-urlencoded")
	response,err:=client.Do(request)

	if err!=nil{
		log.Fatal(err)
	}

	if response.StatusCode==200{
		body, err := ioutil.ReadAll(response.Body)
        if err != nil {
            log.Fatal(err)
        }
        fmt.Println(string(body))
	}
	fmt.Printf("response:%+v\n",response)
	fmt.Printf("response.Header:%+v\n",response.Header)
	fmt.Printf("response.Cookies:%+v\n",response.Cookies())
	fmt.Printf("request.Header:%+v\n",request.Header)
	fmt.Printf("request.Cookie:%+v\n",request.Cookies())

}
```

```go
//客户端输出
response:&{Status:200 OK StatusCode:200 Proto:HTTP/1.1 ProtoMajor:1 ProtoMinor:1 Header:map[Access-Control-Allow-Header:[*] Access-Control-Allow-Origin:[*] Content-Length:[16] Content-Type:[text/plain; charset=utf-8] Date:[Fri, 25 Jun 2021 02:55:56 GMT]] Body:0xc00018e040 ContentLength:16 TransferEncoding:[] Close:false Uncompressed:false Trailer:map[] Request:0xc000142000 TLS:<nil>} 

response.Header:map[Access-Control-Allow-Header:[*] Access-Control-Allow-Origin:[*] Content-Length:[16] Content-Type:[text/plain; charset=utf-8] Date:[Fri, 25 Jun 2021 02:55:56 GMT]]
response.Cookies:[]
request.Header:map[Content-Type:[application/x-www-form-urlencoded] Cookie:[userId=1234; name=huaiLi]]
request.Cookie:[userId=1234 name=huaiLi]
```

```go
//服务端输出
body 
r:&{Method:POST URL:/login Proto:HTTP/1.1 ProtoMajor:1 ProtoMinor:1 Header:map[Accept-Encoding:[gzip] Content-Length:[14] Content-Type:[application/x-www-form-urlencoded] Cookie:[userId=1234; name=huaiLi] User-Agent:[Go-http-client/1.1]] Body:0xc00008a0c0 GetBody:<nil> ContentLength:14 TransferEncoding:[] Close:false Host:localhost:2000 Form:map[username:[haiyi]] PostForm:map[username:[haiyi]] MultipartForm:<nil> Trailer:map[] RemoteAddr:[::1]:51272 RequestURI:/login TLS:<nil> Cancel:<nil> Response:<nil> ctx:0xc00008a100}
request:hander:map[Accept-Encoding:[gzip] Content-Length:[14] Content-Type:[application/x-www-form-urlencoded] Cookie:[userId=1234; name=huaiLi] User-Agent:[Go-http-client/1.1]]
cookies:[userId=1234 name=huaiLi]
userId 1234
username: haiyi
```

## 四  go与表单

### 4.1 处理表单输入

login.html文件

```html
<!DOCTYPE html>
<html  lang="en">
    <head>
        <meta charset="UTF-8">
        <title>
            登录界面
        </title>
    </head>
    <body>
        <form action="http://127.0.0.1:8080/login" method="POST">
            用户名：<input type="text" name="username"><br>
            密&nbsp&nbsp&nbsp码:<input type="password" name="password"><br>
            <input type="submit" value="登录">
        </form>
    </body>
</html>
```

服务端处理程序

```go
package main

import (
	"fmt"
	"html/template"
	"net/http"
	"strings"
)

//服务端程序
func main() {
	http.HandleFunc("/hello", sayhello)
	http.HandleFunc("/login", login)
	err := http.ListenAndServe(":8080", nil)

	if err != nil {
		fmt.Println("ListenAndServer", err)
	}
}

func sayhello(w http.ResponseWriter, r *http.Request) {
	r.ParseForm()

	fmt.Println("r.Form:", r.Form)
	fmt.Println("path", r.URL.Path)
	fmt.Println("scheme", r.URL.Scheme)
	fmt.Println(r.Form["url_long"])

	for k, v := range r.Form {
		fmt.Println("key:", k)
		fmt.Println("val:", strings.Join(v, ""))
	}

	fmt.Fprintf(w, "Hello my route")

}

func login(w http.ResponseWriter, r *http.Request) {
	r.ParseForm()
	fmt.Println("method:", r.Method)
	if r.Method == "GET" {
		t, _ := template.ParseFiles("D:/goproject/src/login.html")
		t.Execute(w, nil)
	} else {
		fmt.Println("username", r.Form["username"])
		fmt.Println("password", r.Form["password"])
	}
}
```

### 4.2  验证表单的输入

#### 4.2.1 必填字段

你想要确保从一个表单元素中得到一个值，例如前面小节里面的用户名，我们如何处理呢？Go有一个内置函数len可以获取字符串的长度，这样我们就可以通过len来获取数据的长度，例如：

```go
if len(r.Form["username"][0])==0{
//为空的处理
}
```

r.Form对不同类型的表单元素的留空有不同的处理， 对于空文本框、空文本区域以及文件上传，元素的值为空值，而如果是未选中的复选框和单选按钮，则根本不会在r.Form中产生相应条目，如果我们用上面例子中的方式去获取数据时程序就会报错。所以我们需要通过r.Form.Get()来获取值，因为如果字段不存在，通过该方式获取的是空值。

但是通过r.Form.Get()只能获取单个的值，如果是map的值，必须通过上面的方式来获取。

#### 4.2.2 数字

你想要确保一个表单输入框中获取的只能是数字，例如，你想通过表单获取某个人的具体年龄是50岁还是10岁，而不是像“一把年纪了”或“年轻着呢”这种描述

如果我们是判断正整数，那么我们先转化成int类型，然后进行处理

```go
getint,err:=strconv.Atoi(r.Form.Get("age"))

if err!=nil{
    //数字转化出错了，那么可能就是不是数字
}

//接下来就可以判断这个数字的大小范围了

if getint >100 {
    //太大了
}
```

还有一种方式就是正则匹配的方式

```go
if m, _ := regexp.MatchString("^[0-9]+$", r.Form.Get("age")); !m  {
    return false
}
```

对于性能要求很高的用户来说，这是一个老生常谈的问题了，他们认为应该尽量避免使用正则表达式，因为使用正则表达式的速度会比较慢。但是在目前机器性能那么强劲的情况下，对于这种简单的正则表达式效率和类型转换函数是没有什么差别的。如果你对正则表达式很熟悉，而且你在其它语言中也在使用它，那么在Go里面使用正则表达式将是一个便利的方式。

> Go实现的正则是RE2，所有的字符都是UTF-8编码的。

#### 4.2.3 中文

有时候我们想通过表单元素获取一个用户的中文名字，但是又为了保证获取的是正确的中文，我们需要进行验证，而不是用户随便的一些输入。对于中文我们目前有效的验证只有正则方式来验证，如下代码所示

```go
if m, _ := regexp.MatchString(`^[\x{4e00}-\x{9fa5}]+$`, r.Form.Get("zhname")); !m {
    return false
}
```

#### 4.2.4 英文

我们期望通过表单元素获取一个英文值，例如我们想知道一个用户的英文名，应该是rubyhan，而不是ruby韩。

我们可以很简单的通过正则验证数据：

```go
if m, _ := regexp.MatchString("^[a-zA-Z]+$", r.Form.Get("enname")); !m{
    return false
}
```

#### 4.2.5 电子邮件地址

你想知道用户输入的一个Email地址是否正确，通过如下这个方式可以验证：

```go
if m, _ := regexp.MatchString(`^([\w\.\_]{2,10})@(\w{1,}).([a-z]{2,4})$`, r.Form.Get("email")); !m{
    fmt.Println("no")
}else{
    fmt.Println("yes")
}
```

#### 4.2.6 手机号码

你想要判断用户输入的手机号码是否正确，通过正则也可以验证：

```go
if m, _ := regexp.MatchString(`^(1[3|5|6|7|8][0-9]\d{8})$`, r.Form.Get("mobile")); !m {
    return false
}
```

#### 4.2.7 下拉菜单

如果我们想要判断表单里面`<select>`元素生成的下拉菜单中是否有被选中的项目。有些时候黑客可能会伪造这个下拉菜单不存在的值发送给你，那么如何判断这个值是否是我们预设的值呢？

我们的select可能是这样的一些元素：

```html
学&nbsp&nbsp&nbsp历：
<!--
selected="selected"
-->
<select name="xueli">
    <option>--请选择--</option>
    <option value="xiaoxue">小学</option>
    <option value="chuzhong">初中</option>
    <option value="gaozhong">高中</option>
    <option value="dazhuan" >大专</option>
    <option value="benke">本科</option>
    <option value="shuoshi">硕士</option>
    <option value="boshi">博士</option>
    <option value="lieshi">烈士</option>
</select>
```

那么我们可以这样来验证：

```go
/**
验证下拉列表
 */
func checkSelect(xueli string) bool {
    slice := []string{"xiaoxue", "chuzhong", "gaozhong", "dazhuan", "benke", "shuoshi", "boshi", "lieshi"}
    for _, v := range slice {

        if v == xueli {
            return true
        }
    }
    return false
}
```

#### 4.2.8 单选按钮

如果我们想要判断radio按钮是否有一个被选中了，我们页面的输出可能就是一个男、女性别的选择，但是也可能一个15岁大的无聊小孩，一手拿着http协议的书，另一只手通过telnet客户端向你的程序在发送请求呢，你设定的性别男值是1，女是2，他给你发送一个3，你的程序会出现异常吗？因此我们也需要像下拉菜单的判断方式类似，判断我们获取的值是我们预设的值，而不是额外的值。

```html
<input type="radio" name="sex" value="male" checked="checked"/>男
<input type="radio" name="sex" value="female"/>女
<input type="radio" name="sex" value="other"/>其他
```

那我们也可以类似下拉菜单的做法一样：

```go
/**
验证单选按钮
 */
func checkSex(sex string) bool {
    slice := []string{"male", "female", "other"}
    for _, v := range slice {
        if v == sex {
            return true
        }
    }
    return false
}
```

#### 4.2.9 复选框

有一项选择兴趣的复选框，你想确定用户选中的和你提供给用户选择的是同一个类型的数据。

```html
爱&nbsp&nbsp&nbsp好：
<input type="checkbox" name="hobby" value="game" checked="checked"/>游戏
<input type="checkbox" name="hobby" value="girl" />女人
<input type="checkbox" name="hobby" value="money" />金钱
<input type="checkbox" name="hobby" value="power" />权利
<br />
```

对于复选框我们的验证和单选有点不一样，因为接收到的数据是一个slice：

```go
/**
验证复选框
 */
func checkHobby(hobby []string) bool {
    slice := []string{"game", "girl", "money", "power"}

    hobby2 := Slice_diff(hobby, slice)

    if hobby2 == nil {
        return true
    }
    return false
}

func Slice_diff(slice1, slice2 []string) (diffslice []string) {
    for _, v := range slice1 {
        if !InSlice(v, slice2) {
            diffslice = append(diffslice, v)
        }
    }
    return
}

/**
判断是一个切片中是否包含指定的数值
 */
func InSlice(val string, slice []string) bool {
    for _, v := range slice {
        if v == val {
            return true
        }
    }
    return false
}
```

#### 4.2.10 身份证号码

如果我们想验证表单输入的是否是身份证，通过正则也可以方便的验证，但是身份证现在都是18位，我们可以进行如下验证：

```go
//验证18位身份证，18位前17位为数字，最后一位是校验位，可能为数字或字符X。
if m, _ := regexp.MatchString(`^(\d{17})([0-9]|X)$`, r.Form.Get("usercard")); !m {
    return false
}
```

上面列出了我们一些常用的服务器端的表单元素验证，希望通过这个引导入门，能够让大家对Go的数据验证有所了解，特别是Go里面的正则处理。

#### 4.2.11 完整代码

1.html页面：register.html

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>验证表单</title>
    </head>
    <body>
        <h1>注册信息</h1>
        <form action="http://127.0.0.1:8080/register" method="post">
            用户名：
            <input type="text" name="username" id="username" />
            <br />
            密&nbsp&nbsp&nbsp码：
            <input type="password" name="pwd" id="pwd" />
            <br />
            中文名：
            <input type="text" name="zhname" id="zhname" />
            <br />
            英文名：
            <input type="text" name="enname" id="enname" />
            <br />
            年&nbsp&nbsp&nbsp龄：
            <input type="text" name="age" id="age" />
            <br />
            性&nbsp&nbsp&nbsp&nbsp别：
            <input type="radio" name="sex" value="male" checked="checked"/>男
            <input type="radio" name="sex" value="female"/>女
            <input type="radio" name="sex" value="other"/>其他
            <br />
            邮&nbsp&nbsp&nbsp箱：
            <input type="text" name="email" id="email" />
            <br />

            手机号码：
            <input type="text" name="mobile" id="mobile" />
            <br />
            身份证号：
            <input type="text" name="usercard" id="usercard">
            <br>
            爱&nbsp&nbsp&nbsp好：
            <input type="checkbox" name="hobby" value="game" checked="checked"/>游戏
            <input type="checkbox" name="hobby" value="girl" />女人
            <input type="checkbox" name="hobby" value="money" />金钱
            <input type="checkbox" name="hobby" value="power" />权利
            <br />
            学&nbsp&nbsp&nbsp历：
            <!--
                selected="selected"
            -->
            <select name="xueli">
                <option>--请选择--</option>
                <option value="xiaoxue">小学</option>
                <option value="chuzhong">初中</option>
                <option value="gaozhong">高中</option>
                <option value="dazhuan" >大专</option>
                <option value="benke">本科</option>
                <option value="shuoshi">硕士</option>
                <option value="boshi">博士</option>
                <option value="lieshi">烈士</option>
            </select>
            <br />
            头&nbsp&nbsp&nbsp像：
            <input type="file" name="myfile" />
            <br />
            个人简介：
            <br />
            <textarea rows="8" cols="70"></textarea>
            <br />
            <!--
                按钮：带事件，不带事件
                    事件：发生了某一件事
                    带事件：按钮被点击，会触发某一件事
                    不带事件：按钮被点击，页面没有反应。配合JavaScript

                button:无事件
                image：无事件
                reset：有事件，清空表单数据
                submit：有事件，提交表单
                    当submit按钮被点击，触发form表单中action属性的路径(服务器地址)

                提交方式：
                get：默认
                    词意：获取，获得
                    url？username=zhangsan&pwd=123456&sex=female..
                    url：请求的路径地址
                    ？
                        前是请求路径
                         后本次请求提交的数据
                        传递的数据：采用名值对的形式
                        参数名=参数值&参数名=参数值。。。

                    不安全：数据暴露了
                    传递少量的数据
                    容易乱码

                post：
                    词意：邮政邮局
                    数据打包之后，传递给服务端
                    数据安全
                    可以传递大量的数据
                    不容易乱码

            -->
            <input type="button" value="按钮" />
            <input type="reset" value="重置"/>
            <input type="image" src="img/qq.gif" />
            <input type="submit" value="提交" />
        </form>
    </body>
</html>
```

2.go文件：demo02_checkform.go

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "strconv"
    "regexp"
)

func register(w http.ResponseWriter, r *http.Request) {
    r.ParseForm()
    //1.验证必填字段
    //username := r.Form["username"][0]
    username := r.Form.Get("username")
    if len(username) == 0 {
        fmt.Println("用户名不能为空！")
        fmt.Fprintf(w, "用户名不能为空！") //这个写入到w的是输出到客户端的
    }
    //2.验证数字
    age, err := strconv.Atoi(r.Form.Get("age"))
    if err != nil {
        //数字转化出错了，那么可能就是不是数字
        fmt.Println("您输入的不是数字！")
        fmt.Fprintf(w, "您输入的不是数字！") //这个写入到w的是输出到客户端的
    }
    //接下来就可以判断这个数字的大小范围了
    if age > 100 || age < 0 {
        //太大了或太小了
        fmt.Println("您输入的年龄太大了或太小了，请输入0-100之间的整数！")
        fmt.Fprintf(w, "您输入的年龄太大了或太小了，请输入0-100之间的整数！") //这个写入到w的是输出到客户端的
    }

    //或者正则表达式
    if m, _ := regexp.MatchString("^[0-9]+$", r.Form.Get("age")); !m {
        fmt.Println("验证有误，您输入的年龄太大了或太小了！")
        fmt.Fprintf(w, "验证有误，您输入的年龄太大了或太小了！")
    }

    //3.验证中文
    if m, _ := regexp.MatchString(`^[\x{4e00}-\x{9fa5}]+$`, r.Form.Get("zhname")); !m {
        fmt.Println("验证有误，请输入中文！")
        fmt.Fprintf(w, "验证有误，请输入中文！")
    }

    //4. 验证英文
    if m, _ := regexp.MatchString("^[a-zA-Z]+$", r.Form.Get("enname")); !m {
        fmt.Println("验证有误，请输入英文！")
        fmt.Fprintf(w, "验证有误，请输入英文！")
    }

    //5. 邮箱
    if m, _ := regexp.MatchString(`^([\w\.\_]{2,10})@(\w{1,}).([a-z]{2,4})$`, r.Form.Get("email")); !m {
        fmt.Println("请输入正确邮箱地址")
        fmt.Fprintf(w, "验证有误，请输入正确邮箱地址！")
    }

    //6. 验证手机号
    if m, _ := regexp.MatchString(`^(1[3|5|6|7|8][0-9]\d{8})$`, r.Form.Get("mobile")); !m {
        fmt.Println("请输入正确手机号码")
        fmt.Fprintf(w, "验证有误，请输入正确手机号码！")
    }

    //7. 下拉菜单
    xueli := r.Form.Get("xueli")
    res1 := checkSelect(xueli)
    if !res1 {
        fmt.Println("请选择正确的下拉列表！")
        fmt.Fprintf(w, "请选择正确的下拉列表！")
    }

    // 8. 单选按钮
    sex := r.Form.Get("sex")
    res2 := checkSex(sex)
    if !res2 {
        fmt.Println("请选择正确的性别！")
        fmt.Fprintf(w, "请选择正确的性别！")
    }

    // 9. 复选框
    hobby := r.Form["hobby"]
    res3 := checkHobby(hobby)
    if !res3 {
        fmt.Println("请选择正确的爱好！")
        fmt.Fprintf(w, "请选择正确的爱好！")
    }

    // 10 身份证号
    //验证18位身份证，18位前17位为数字，最后一位是校验位，可能为数字或字符X。
    if m, _ := regexp.MatchString(`^(\d{17})([0-9]|X)$`, r.Form.Get("usercard")); !m {
        fmt.Println("请选择正确的身份证号！")
        fmt.Fprintf(w, "请选择正确的身份证号！")
    }

    //fmt.Println("验证成功！")
    //fmt.Fprintf(w, "验证成功！")

}

/**
验证下拉列表
 */
func checkSelect(xueli string) bool {
    slice := []string{"xiaoxue", "chuzhong", "gaozhong", "dazhuan", "benke", "shuoshi", "boshi", "lieshi"}
    for _, v := range slice {

        if v == xueli {
            return true
        }
    }
    return false
}

/**
验证单选按钮
 */
func checkSex(sex string) bool {
    slice := []string{"male", "female", "other"}
    for _, v := range slice {
        if v == sex {
            return true
        }
    }
    return false
}

/**
验证复选框
 */
func checkHobby(hobby []string) bool {
    slice := []string{"game", "girl", "money", "power"}

    hobby2 := Slice_diff(hobby, slice)

    if hobby2 == nil {
        return true
    }
    return false
}

func Slice_diff(slice1, slice2 []string) (diffslice []string) {
    for _, v := range slice1 {
        if !InSlice(v, slice2) {
            diffslice = append(diffslice, v)
        }
    }
    return
}

/**
判断是一个切片中是否包含指定的数值
 */
func InSlice(val string, slice []string) bool {
    for _, v := range slice {
        if v == val {
            return true
        }
    }
    return false
}

func main() {
    http.HandleFunc("/register", register)   //设置访问的路由
    err := http.ListenAndServe(":8080", nil) //设置监听的端口
    if err != nil {
        log.Fatal("ListenAndServe: ", err)
    }
}
```

### 4.3 预防跨站脚本

现在的网站包含大量的动态内容以提高用户体验，比过去要复杂得多。所谓动态内容，就是根据用户环境和需要，Web应用程序能够输出相应的内容。动态站点会受到一种名为“跨站脚本攻击”（Cross Site Scripting, 安全专家们通常将其缩写成 XSS）的威胁，而静态站点则完全不受其影响。

攻击者通常会在有漏洞的程序中插入JavaScript、VBScript、 ActiveX或Flash以欺骗用户。一旦得手，他们可以盗取用户帐户信息，修改用户设置，盗取/污染cookie和植入恶意广告等。

对XSS最佳的防护应该结合以下两种方法：一是验证所有输入数据，有效检测攻击(这个我们前面小节已经有过介绍)；另一个是对所有输出数据进行适当的处理，以防止任何已成功注入的脚本在浏览器端运行。

那么Go里面是怎么做这个有效防护的呢？Go的html/template里面带有下面几个函数可以帮你转义

- func HTMLEscape(w io.Writer, b []byte) //把b进行转义之后写到w
- func HTMLEscapeString(s string) string //转义s之后返回结果字符串
- func HTMLEscaper(args ...interface{}) string //支持多个参数一起转义，返回结果字符串

创建go文件，demo03_template.go，代码如下：

```go
package main

import (
    "fmt"
    "html/template"
    "log"
    "net/http"
)

func login(w http.ResponseWriter, r *http.Request) {
    r.ParseForm()
    username := r.Form.Get("username")

    fmt.Println("username:", template.HTMLEscapeString(username)) //输出到服务器端

    fmt.Println("password:", template.HTMLEscapeString(r.Form.Get("password")))

    template.HTMLEscape(w, []byte(username)) //输出到客户端

    //fmt.Fprintf(w, username) //这个写入到w的是输出到客户端的
}

func main() {
    http.HandleFunc("/login", login)      //设置访问的路由
    err := http.ListenAndServe(":8080", nil) //设置监听的端口
    if err != nil {
        log.Fatal("ListenAndServe: ", err)
    }
}
```

如果我们输入的username是`<script>alert()</script>`,那么我们可以在浏览器上面看到输出如下所示：

![yunxing1](http://img.kongyixueyuan.com/yunxing1.png/mark)

或者是：

```go
func login2(w http.ResponseWriter, r *http.Request){
    r.ParseForm()
    username := r.Form.Get("username")
    fmt.Println(username)

    //进行模板解析
    t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
    err = t.ExecuteTemplate(w, "T", username)

    //如果转义失败 抛出对应错误 终止程序
    if err != nil {
        log.Fatal(err)
    }
}
```

运行结果也一样：

![yunxing2](http://img.kongyixueyuan.com/yunxing2.png/mark)

Go的html/template包默认帮你过滤了html标签，但是有时候你只想要输出这个`<script>alert()</script>`看起

来正常的信息，该怎么处理？请使用template.HTML类型。

```go
err = t.ExecuteTemplate(w, "T", template.HTML(username))
```

仅替换一行代码即可：

![daima](http://img.kongyixueyuan.com/daima.png/mark)

浏览器运行结果：

![yunxing3](http://img.kongyixueyuan.com/yunxing3.png/mark)

### 4.4 防止多次递交表单

不知道你是否曾经看到过一个论坛或者博客，在一个帖子或者文章后面出现多条重复的记录，这些大多数是因为用户重复递交了留言的表单引起的。由于种种原因，用户经常会重复递交表单。通常这只是鼠标的误操作，如双击了递交按钮，也可能是为了编辑或者再次核对填写过的信息，点击了浏览器的后退按钮，然后又再次点击了递交按钮而不是浏览器的前进按钮。当然，也可能是故意的——比如，在某项在线调查或者博彩活动中重复投票。那我们如何有效的防止用户多次递交相同的表单呢？

解决方案是在表单中添加一个带有唯一值的隐藏字段。在验证表单时，先检查带有该惟一值的表单是否已经递交过了。如果是，拒绝再次递交；如果不是，则处理表单进行逻辑处理。另外，如果是采用了Ajax模式递交表单的话，当表单递交后，通过javascript来禁用表单的递交按钮。

创建一个html的模板文件test.gtpl，添加代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>防止多次提交</title>
</head>
<body>
    <form action="http://127.0.0.1:8080/login" method="post">
        用户名:<input type="text" name="username"><br>
        密&nbsp&nbsp&nbsp码:<input type="password" name="password"><br>
        <input type="hidden" name="token" value="{{.}}">
        <input type="submit" value="登陆">
    </form>
</body>
</html>
```

创建一个go文件，demo04_server.go，代码如下：

```go
package main

import (
    "fmt"
    "html/template"
    "log"
    "net/http"
    "time"
    "crypto/md5"
    "strconv"
    "io"
    "os"
)

func login(w http.ResponseWriter, r *http.Request) {
    fmt.Println("method:", r.Method) //获取请求的方法
    if r.Method == "GET" {
        crutime := time.Now().Unix()
        h := md5.New()
        io.WriteString(h, strconv.FormatInt(crutime, 10))
        token := fmt.Sprintf("%x", h.Sum(nil))
        fmt.Println("token--->", token)
        t, _ := template.ParseFiles("test.gtpl")
        t.Execute(w, token)
    } else {
        //请求的是登陆数据，那么执行登陆的逻辑判断
        r.ParseForm()
        token := r.Form.Get("token")
        if token != "" {
            //验证token的合法性
            fmt.Println("token:", token)
        } else {
            //不存在token报错
            fmt.Println("token有误。。")
        }
        fmt.Println("username length:", len(r.Form["username"][0]))
        fmt.Println("username:", template.HTMLEscapeString(r.Form.Get("username"))) //输出到服务器端
        fmt.Println("password:", template.HTMLEscapeString(r.Form.Get("password")))
        template.HTMLEscape(w, []byte(r.Form.Get("username"))) //输出到客户端
    }
}

func main() {
    http.HandleFunc("/login", login)         //设置访问的路由
    err := http.ListenAndServe(":8080", nil) //设置监听的端口
    if err != nil {
        log.Fatal("ListenAndServe: ", err)
    }
}
```

我们看到token已经有输出值，你可以不断的刷新，可以看到这个值在不断的变化。这样就保证了每次显示form表单的时候都是唯一的，用户递交的表单保持了唯一性。

我们的解决方案可以防止非恶意的攻击，并能使恶意用户暂时不知所措，然后，它却不能排除所有的欺骗性的动机，对此类情况还需要更复杂的工作。

### 4.5 处理文件上传

你想处理一个由用户上传的文件，比如你正在建设一个类似Instagram的网站，你需要存储用户拍摄的照片。这种需求该如何实现呢？

要使表单能够上传文件，首先第一步就是要添加form的enctype属性，enctype属性有如下三种情况:

```
application/x-www-form-urlencoded 表示在发送前编码所有字符（默认）
multipart/form-data 不对字符编码。在使用包含文件上传控件的表单时，必须使用该值。
text/plain 空格转换为 "+" 加号，但不对特殊字符编码。
```

新建html页面(upload.html)，html页面代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>
    <form enctype="multipart/form-data" action="http://127.0.0.1:8080/upload" method="post">
        <input type="file" name="uploadfile"/><br>
        <input type="hidden" name="token" value="{{.}}"/><br>
        <input type="submit" value="upload"/>
    </form>
</body>
</html>
```

新建go文件(demo05_uploadserver.go)，go文件代码：

```go
package main

import (
    "fmt"
    "html/template"
    "log"
    "net/http"
    "time"
    "crypto/md5"
    "strconv"
    "io"
    "os"
)

func main() {
    http.HandleFunc("/upload", upload)
    err := http.ListenAndServe(":8080", nil) //设置监听的端口
    if err != nil {
        log.Fatal("ListenAndServe: ", err)
    }
}

// 处理/upload 逻辑
func upload(w http.ResponseWriter, r *http.Request) {
    fmt.Println("method:", r.Method) //获取请求的方法
    if r.Method == "GET" {
        crutime := time.Now().Unix()
        h := md5.New()
        io.WriteString(h, strconv.FormatInt(crutime, 10))
        token := fmt.Sprintf("%x", h.Sum(nil))
        t, _ := template.ParseFiles("upload.gtpl")
        t.Execute(w, token)
    } else {
        r.ParseMultipartForm(32 << 20)
        file, handler, err := r.FormFile("uploadfile")
        if err != nil {
            fmt.Println(err)
            return
        }
        defer file.Close()
        fmt.Fprintf(w, "%v", handler.Header)
        f, err := os.OpenFile("./test/"+handler.Filename, os.O_WRONLY|os.O_CREATE, 0666)
        if err != nil {
            fmt.Println(err)
            return
        }
        defer f.Close()
        io.Copy(f, file)
    }
}
```

通过上面的代码可以看到，处理文件上传我们需要调用r.ParseMultipartForm，里面的参数表示maxMemory，调用ParseMultipartForm之后，上传的文件存储在maxMemory大小的内存里面，如果文件大小超过了maxMemory，那么剩下的部分将存储在系统的临时文件中。我们可以通过r.FormFile获取上面的文件句柄，然后实例中使用了io.Copy来存储文件。

获取其他非文件字段信息的时候就不需要调用r.ParseForm，因为在需要的时候Go自动会去调用。而且ParseMultipartForm调用一次之后，后面再次调用不会再有效果。

通过上面的实例我们可以看到我们上传文件主要三步处理：

1. 表单中增加enctype="multipart/form-data"
2. 服务端调用r.ParseMultipartForm,把上传的文件存储在内存和临时文件中
3. 使用r.FormFile获取文件句柄，然后对文件进行存储等处理。

文件handler是multipart.FileHeader，里面存储了如下结构信息：

```go
type FileHeader struct {

    Filename string

    Header textproto.MIMEHeader

    // contains filtered or unexported fields

}
```

我们通过上面的实例代码打印出来上传文件的信息如下：

![yunxing4](http://img.kongyixueyuan.com/yunxing4.png/mark)

### 4.6 客户端上传文件

我们上面的例子演示了如何通过表单上传文件，然后在服务器端处理文件，其实Go支持模拟客户端表单功能支持文件上传，详细用法请看如下示例：

创建一个go文件，来表示客户端：demo06_uploadclient.go，代码如下：

```go
package main

import (
    "bytes"
    "fmt"
    "io"
    "io/ioutil"
    "mime/multipart"
    "net/http"
    "os"
)

func postFile(filename string, targetUrl string) error {
    bodyBuf := &bytes.Buffer{}
    bodyWriter := multipart.NewWriter(bodyBuf)
    //关键的一步操作
    fileWriter, err := bodyWriter.CreateFormFile("uploadfile", filename)
    if err != nil {
        fmt.Println("error writing to buffer")
        return err
    }
    //打开文件句柄操作
    fh, err := os.Open(filename)
    if err != nil {
        fmt.Println("error opening file")
        return err
    }
    //iocopy
    _, err = io.Copy(fileWriter, fh)
    if err != nil {
        return err
    }
    contentType := bodyWriter.FormDataContentType()
    bodyWriter.Close()
    resp, err := http.Post(targetUrl, contentType, bodyBuf)
    if err != nil {
        return err
    }
    defer resp.Body.Close()
    resp_body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        return err
    }
    fmt.Println(resp.Status)
    fmt.Println(string(resp_body))
    return nil
}

// sample usage
func main() {
    target_url := "http://localhost:8080/upload"
    filename := "./正则验证.docx"
    postFile(filename, target_url)
}
```

以上代码详细展示了客户端如何向服务器上传一个文件，客户端通过multipart.Write把文件的文本流写入一个缓存中，然后调用http的Post方法把缓存传到服务器

## 五  go操作MySql

导入包

> ```go
> "database/sql"        _ "github.com/go-sql-driver/mysql"
> ```

Go没有官方提供数据库驱动，而是为开发者开发数据库驱动定义了一些标准接口，开发者可以根据定义的接口来开发相应的数据库驱动，这样做有一个好处，只要按照标准接口开发的代码， 以后需要迁移数据库时，不需要任何修改。

### 5.1  database/sql接口

#### 5.1.1  sql.Register

```go
func init() {
    sql.Register("mysql", &MySQLDriver{})
}

type MySQLDriver struct{}

func (d MySQLDriver) Open(dsn string) (driver.Conn, error) {
	cfg, err := ParseDSN(dsn)
	if err != nil {
		return nil, err
	}
	c := &connector{
		cfg: cfg,
	}
	return c.Connect(context.Background())
}
```

通过init()方法将mysql驱动注册到database\sql

> ```go 
> drivers   = make(map[string]driver.Driver)
> ```

所有sql驱动注册存储在map里

```go
func Register(name string, driver driver.Driver) {
	driversMu.Lock()
	defer driversMu.Unlock()
	if driver == nil {
		panic("sql: Register driver is nil")
	}
	if _, dup := drivers[name]; dup {
		panic("sql: Register called twice for driver " + name)
	}
	drivers[name] = driver
}
```

#### 5.1.2  driver.Driver接口

Driver是一个数据库驱动的接口，他定义了一个method： Open(name string)，这个方法返回一个数据库的Conn接口。

```go
type Driver interface {
	// Open returns a new connection to the database.
	// The name is a string in a driver-specific format.
	//
	// Open may return a cached connection (one previously
	// closed), but doing so is unnecessary; the sql package
	// maintains a pool of idle connections for efficient re-use.
	//
	// The returned connection is only used by one goroutine at a
	// time.
	Open(name string) (Conn, error)
}
```

返回的Conn只能用来进行一次goroutine的操作，也就是说不能把这个Conn应用于Go的多个goroutine里面。如下代码会出现错误

```go
go goroutineA(conn)
go goroutineB(conn)
```

#### 5.1.3 driver.Conn

```go
type Conn interface {
	// Prepare returns a prepared statement, bound to this connection.
	Prepare(query string) (Stmt, error)

	// Close invalidates and potentially stops any current
	// prepared statements and transactions, marking this
	// connection as no longer in use.
	//
	// Because the sql package maintains a free pool of
	// connections and only calls Close when there's a surplus of
	// idle connections, it shouldn't be necessary for drivers to
	// do their own connection caching.
	Close() error

	// Begin starts and returns a new transaction.
	//
	// Deprecated: Drivers should implement ConnBeginTx instead (or additionally).
	Begin() (Tx, error)
}
```

- Prepare函数返回与当前连接相关的执行Sql语句的准备状态，可以进行查询、删除等操作。
- Close函数关闭当前的连接，执行释放连接拥有的资源等清理工作。因为驱动实现了database/sql里面建议的conn pool，所以你不用再去实现缓存conn之类的，这样会容易引起问题。
- Begin函数返回一个代表事务处理的Tx，通过它你可以进行查询,更新等操作，或者对事务进行回滚、递交。

#### 5.1.4  driver.Stmt

```go
Stmt是一种准备好的状态，和Conn相关联，而且只能应用于一个goroutine中，不能应用于多个goroutine。
```

```go
type Stmt interface {
	// Close closes the statement.
	//
	// As of Go 1.1, a Stmt will not be closed if it's in use
	// by any queries.
	Close() error

	// NumInput returns the number of placeholder parameters.
	//
	// If NumInput returns >= 0, the sql package will sanity check
	// argument counts from callers and return errors to the caller
	// before the statement's Exec or Query methods are called.
	//
	// NumInput may also return -1, if the driver doesn't know
	// its number of placeholders. In that case, the sql package
	// will not sanity check Exec or Query argument counts.
	NumInput() int

	// Exec executes a query that doesn't return rows, such
	// as an INSERT or UPDATE.
	//
	// Deprecated: Drivers should implement StmtExecContext instead (or additionally).
	Exec(args []Value) (Result, error)

	// Query executes a query that may return rows, such as a
	// SELECT.
	//
	// Deprecated: Drivers should implement StmtQueryContext instead (or additionally).
	Query(args []Value) (Rows, error)
}
```

- Close函数关闭当前的链接状态，但是如果当前正在执行query，query还是有效返回rows数据。
- NumInput函数返回当前预留参数的个数，当返回>=0时数据库驱动就会智能检查调用者的参数。当数据库驱动包不知道预留参数的时候，返回-1。
- Exec函数执行Prepare准备好的sql，传入参数执行update/insert等操作，返回Result数据。
- Query函数执行Prepare准备好的sql，传入需要的参数执行select操作，返回Rows结果集。

#### 5.1.5  driver.TX

```go
type Tx interface {
	Commit() error           //事务提交
	Rollback() error		 //事务回滚
}
```

#### 5.1.6 driver.Execer

```go
type Execer interface {
   Exec(query string, args []Value) (Result, error)
}
```

如果这个接口没有定义，那么在调用DB.Exec,就会首先调用Prepare返回Stmt，然后执行Stmt的Exec，然后关闭

#### 5.1.7 driver.Result

这个是执行Update/Insert等操作返回的结果接口定义。

```go
type Result interface {

    LastInsertId() (int64, error)

    RowsAffected() (int64, error)
    
}
```

- LastInsertId函数返回由数据库执行插入操作得到的自增ID号。
- RowsAffected函数返回query操作影响的数据条目数。

#### 5.1.8  driver.RowsAffected

```go
type RowsAffected int64

func (RowsAffected) LastInsertId() (int64, error)

func (v RowsAffected) RowsAffected() (int64, error)
```

#### 5.1.9  DB

```go
type DB struct {
    driver driver.Driver              //数据库实现驱动
    dsn    string               //数据库连接、配置参数信息，比如username、host、password等
    numClosed uint64

    mu           sync.Mutex          //锁，操作DB各成员时用到
    freeConn     []*driverConn       //空闲连接
    connRequests []chan connRequest  //阻塞请求队列，等连接数达到最大限制时，后续请求将插入此队列等待可用连接
    numOpen      int                 //已建立连接或等待建立连接数
    openerCh    chan struct{}        //用于connectionOpener
    closed      bool
    dep         map[finalCloser]depSet
    lastPut     map[*driverConn]string // stacktrace of last conn's put; debug only
    maxIdle     int                    //最大空闲连接数
    maxOpen     int                    //数据库最大连接数
    maxLifetime time.Duration          //连接最长存活期，超过这个时间连接将不再被复用
    cleanerCh   chan struct{}
}
```

> `maxIdle`(默认值2)、`maxOpen`(默认值0，无限制)、`maxLifetime(默认值0，永不过期)`可以分别通过`SetMaxIdleConns`、`SetMaxOpenConns`、`SetConnMaxLifetime`设定。

我们可以看到Open函数返回的是DB对象，里面有一个freeConn，它就是那个简易的连接池。它的实现相当简单或者说简陋，就是当执行Db.prepare的时候会defer db.putConn(ci, err),也就是把这个连接放入连接池，每次调用conn的时候会先判断freeConn的长度是否大于0，大于0说明有可以复用的conn，直接拿出来用就是了，如果不大于0，则创建一个conn,然后再返回之。

> 这时mysql还没有建立连接，只是初始化了一个`sql.DB`结构，这是非常重要的一个结构，所有相关的数据都保存在此结构中；Open同时启动了一个`connectionOpener`协程。

#### 5.10 获取连接

上面说了Open时是没有建立数据库连接的，只有等用的时候才会实际建立连接，获取可用连接的操作有两种策略：cachedOrNewConn(有可用空闲连接则优先使用，没有则创建)、alwaysNewConn(不管有没有空闲连接都重新创建)，下面以一个query的例子看下具体的操作：

```go
rows, err := db.Query("select * from userinfo")
```

以下是Query的源代码：

```go
//database/sql/sql.go：
func (db *DB) Query(query string, args ...interface{}) (*Rows, error) {
    var rows *Rows
    var err error
    //maxBadConnRetries = 2
    for i := 0; i < maxBadConnRetries; i++ {
        rows, err = db.query(query, args, cachedOrNewConn)
        if err != driver.ErrBadConn {
            break
        }
    }
    if err == driver.ErrBadConn {
        return db.query(query, args, alwaysNewConn)
    }
    return rows, err
}

func (db *DB) query(query string, args []interface{}, strategy connReuseStrategy) (*Rows, error) {
    ci, err := db.conn(strategy)
    if err != nil {
        return nil, err
    }

    //到这已经获取到了可用连接，下面进行具体的数据库操作
    return db.queryConn(ci, ci.releaseConn, query, args)
}
```

数据库连接由`db.query()`获取：

```go
func (db *DB) conn(strategy connReuseStrategy) (*driverConn, error) {
    db.mu.Lock()
    if db.closed {
        db.mu.Unlock()
        return nil, errDBClosed
    }
    lifetime := db.maxLifetime

    //从freeConn取一个空闲连接
    numFree := len(db.freeConn)
    if strategy == cachedOrNewConn && numFree > 0 {
        conn := db.freeConn[0]
        copy(db.freeConn, db.freeConn[1:])
        db.freeConn = db.freeConn[:numFree-1]
        conn.inUse = true
        db.mu.Unlock()
        if conn.expired(lifetime) {
            conn.Close()
            return nil, driver.ErrBadConn
        }
        return conn, nil
    }

    //如果没有空闲连接，而且当前建立的连接数已经达到最大限制则将请求加入connRequests队列，
    //并阻塞在这里，直到其它协程将占用的连接释放或connectionOpenner创建
    if db.maxOpen > 0 && db.numOpen >= db.maxOpen {
        // Make the connRequest channel. It's buffered so that the
        // connectionOpener doesn't block while waiting for the req to be read.
        req := make(chan connRequest, 1)
        db.connRequests = append(db.connRequests, req)
        db.mu.Unlock()
        ret, ok := <-req  //阻塞
        if !ok {
            return nil, errDBClosed
        }
        if ret.err == nil && ret.conn.expired(lifetime) { //连接过期了
            ret.conn.Close()
            return nil, driver.ErrBadConn
        }
        return ret.conn, ret.err
    }

    db.numOpen++ //上面说了numOpen是已经建立或即将建立连接数，这里还没有建立连接，只是乐观的认为后面会成功，失败的时候再将此值减1
    db.mu.Unlock()
    ci, err := db.driver.Open(db.dsn) //调用driver的Open方法建立连接
    if err != nil { //创建连接失败
        db.mu.Lock()
        db.numOpen-- // correct for earlier optimism
        db.maybeOpenNewConnections()  //通知connectionOpener协程尝试重新建立连接，否则在db.connRequests中等待的请求将一直阻塞，知道下次有连接建立
        db.mu.Unlock()
        return nil, err
    }
    db.mu.Lock()
    dc := &driverConn{
        db:        db,
        createdAt: nowFunc(),
        ci:        ci,
    }
    db.addDepLocked(dc, dc)
    dc.inUse = true
    db.mu.Unlock()
    return dc, nil
}
```

总结一下上面获取连接的过程：

- step1：首先检查下freeConn里是否有空闲连接，如果有且未超时则直接复用，返回连接，如果没有或连接已经过期则进入下一步；
- step2：检查当前已经建立及准备建立的连接数是否已经达到最大值，如果达到最大值也就意味着无法再创建新的连接了，当前请求需要在这等着连接释放，这时当前协程将创建一个channel：chan connRequest，并将其插入db.connRequests队列，然后阻塞在接收chan connRequest上，等到有连接可用时这里将拿到释放的连接，检查可用后返回；如果还未达到最大值则进入下一步；
- step3：创建一个连接，首先将numOpen加1，然后再创建连接，如果等到创建完连接再把numOpen加1会导致多个协程同时创建连接时一部分会浪费，所以提前将numOpen占住，创建失败再将其减掉；如果创建连接成功则返回连接，失败则进入下一步
- step4：创建连接失败时有一个善后操作，当然并不仅仅是将最初占用的numOpen数减掉，更重要的一个操作是通知connectionOpener协程根据db.connRequests等待的长度创建连接，这个操作的原因是：numOpen在连接成功创建前就加了1，这时候如果numOpen已经达到最大值再有获取conn的请求将阻塞在step2，这些请求会等着先前进来的请求释放连接，假设先前进来的这些请求创建连接全部失败，那么如果它们直接返回了那些等待的请求将一直阻塞在那，因为不可能有连接释放(极限值，如果部分创建成功则会有部分释放)，直到新请求进来重新成功创建连接，显然这样是有问题的，所以maybeOpenNewConnections将通知connectionOpener根据db.connRequests长度及可创建的最大连接数重新创建连接，然后将新创建的连接发给阻塞的请求。

注意：如果maxOpen=0将不会有请求阻塞等待连接，所有请求只要从freeConn中取不到连接就会新创建。

另外Query、Exec有个重试机制，首先优先使用空闲连接，如果2次取到的连接都无效则尝试新创建连接。

获取到可用连接后将调用具体数据库的driver处理sql。

#### 5.1.11 释放连接

数据库连接在被使用完成后需要归还给连接池以供其它请求复用，释放连接的操作是：`putConn()`：

```go
func (db *DB) putConn(dc *driverConn, err error) {
    ...

    //如果连接已经无效，则不再放入连接池
    if err == driver.ErrBadConn {
        db.maybeOpenNewConnections()
        dc.Close() //这里最终将numOpen数减掉
        return
    }
    ...

    //正常归还
    added := db.putConnDBLocked(dc, nil)
    ...
}

func (db *DB) putConnDBLocked(dc *driverConn, err error) bool {
    if db.maxOpen > 0 && db.numOpen > db.maxOpen {
        return false
    }
    //有等待连接的请求则将连接发给它们，否则放入freeConn
    if c := len(db.connRequests); c > 0 {
        req := db.connRequests[0]
        // This copy is O(n) but in practice faster than a linked list.
        // TODO: consider compacting it down less often and
        // moving the base instead?
        copy(db.connRequests, db.connRequests[1:])
        db.connRequests = db.connRequests[:c-1]
        if err == nil {
            dc.inUse = true
        }
        req <- connRequest{
            conn: dc,
            err:  err,
        }
        return true
    } else if err == nil && !db.closed && db.maxIdleConnsLocked() > len(db.freeConn) {
        db.freeConn = append(db.freeConn, dc)
        db.startCleanerLocked()
        return true
    }
    return false
}
```

释放的过程：

- step1：首先检查下当前归还的连接在使用过程中是否发现已经无效，如果无效则不再放入连接池，然后检查下等待连接的请求数新建连接，类似获取连接时的异常处理，如果连接有效则进入下一步；
- step2：检查下当前是否有等待连接阻塞的请求，有的话将当前连接发给最早的那个请求，没有的话则再判断空闲连接数是否达到上限，没有则放入freeConn空闲连接池，达到上限则将连接关闭释放。
- step3：(只执行一次)启动connectionCleaner协程定时检查feeConn中是否有过期连接，有则剔除。

有个地方需要注意的是，Query、Exec操作用法有些差异：

a.Exec(update、insert、delete等无结果集返回的操作)调用完后会自动释放连接； b.Query(返回sql.Rows)则不会释放连接，调用完后仍然占有连接，它将连接的所属权转移给了sql.Rows，所以需要手动调用close归还连接，即使不用Rows也得调用rows.Close()，否则可能导致后续使用出错，如下的用法是错误的：

```go
//错误
db.SetMaxOpenConns(1)
db.Query("select * from test")

row,err := db.Query("select * from test") //此操作将一直阻塞

//正确
db.SetMaxOpenConns(1)
r,_ := db.Query("select * from test")
r.Close() //将连接的所属权归还，释放连接
row,err := db.Query("select * from test")
//other op
row.Close()
```

### 5.2  go操作mysql

#### 5.2.1  MySql常用驱动

https://github.com/Go-SQL-Driver/MySQL 支持database/sql，全部采用go写。(最常用)

https://github.com/ziutek/mymysql 			支持database/sql，也支持自定义的接口，全部采用go写。

https://github.com/Philio/GoMySQL			 不支持database/sql，自定义接口，全部采用go写。

#### 5.2.2  建立连接

Open()函数

```go
func Open(driverName, dataSourceName string) (*DB, error) {
    driverName:数据库驱动注册到database/sql时使用的名字：mysql
    dataSourceName:数据库连接信息，包含了数据库的用户名，密码，主机，端口号
    
    //示例
    //db, err := sql.Open("mysql", "用户名:密码@tcp(IP:端口)/数据库?charset=utf8")
}
```

> ```go
> db, err := sql.Open("mysql", "root:111111@tcp(127.0.0.1:3306)/test?charset=utf8")
> ```

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)

func main(){
	db,err:=sql.Open("mysql","root:12345@tcp(127.0.0.1:3306)/usersql?charset=utf8")

	fmt.Println(db)
	if err!=nil{
		fmt.Println("连接失败")
		return
	}
	fmt.Println("连接成功")
	db.Close()
}
```

输出结果

```text
&{0 0xc000006028 0 {0 0} [] map[] 0 0 0xc000048120 false map[] map[] 0 0 0 <nil> 0 0 0 0x490ed0}
连接成功
```

说明：

* sql.Open并不会立即建立一个数据库的网络连接, 也不会对数据库链接参数的合法性做检验, 它仅仅是初始化一个sql.DB对象. 当真正进行第一次数据库查询操作时, 此时才会真正建立网络连接;

* sql.DB表示操作数据库的抽象接口的对象，但不是所谓的数据库连接对象，sql.DB对象只有当需要使用时才会创建连接，如果想立即验证连接，需要用Ping()方法;

* sql.Open返回的sql.DB对象是协程并发安全的.

* sql.DB的设计就是用来作为长连接使用的。不要频繁Open, Close。比较好的做法是，为每个不同的datastore建一个DB对象，保持这些对象Open。如果需要短连接，那么把DB作为参数传入function，而不要在function中Open, Close。

#### 5.2.3  DML操作：增删改

方式一：直接使用Exec函数添加

> ```go 
> func (db *DB) Exec(query string, args ...interface{}) (Result, error) {
> 	return db.ExecContext(context.Background(), query, args...)
> }
> ```

示例代码

```go
result, err := db.Exec("UPDATE userinfo SET username = ?, departname = ? WHERE uid = ?", "王二狗","行政部",2)
```

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
	"log"
)

func main(){
    //步骤一：连接数据库
	db,err:=sql.Open("mysql","root:12345@tcp(localhost:3306)/usersql?charset=utf8")

	if err!=nil{
		fmt.Println("数据库连接失败",err)
		return
	}
	fmt.Println("数据库连接成功")
    //步骤二：插入数据
	_, err=db.Exec("insert into  userinfo(username,departname,created) values(?,?,?)","曹操","魏国","2021-06-28")
	if err!=nil{
		log.Fatal("错误原因：",err)
	}
	db.Close()
}
```

方式二：先使用Prepare获得Stmt，然后调用Exec添加

```go
func (db *DB) Prepare(query string) (*Stmt, error) {
	return db.PrepareContext(context.Background(), query)
}
```

 ```go
 func (s *Stmt) Exec(args ...interface{}) (Result, error) {
 	return s.ExecContext(context.Background(), args...)
 }
 ```

示例代码

```go
stmt,err:=db.Prepare("insert  into userinfo(username,department,created) values(?,?,?)")
stmt.Exec("曹操","魏国","2021-06-28")
```

使用PreparedStatement(预编译)的优点

- PreparedStatement 可以实现自定义参数的查询
- PreparedStatement 通常来说, 比手动拼接字符串 SQL 语句高效.
- PreparedStatement 可以防止SQL注入攻击

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
	"log"
	"time"
)

func main(){
	//步骤一：连接数据库  建立DB对象
	db,err:=sql.Open("mysql","root:12345@tcp(localhost:3306)/usersql?charset=utf8")
	if err!=nil{
		fmt.Println("数据库连接失败",err)
		return
	}
	fmt.Println("数据库连接成功")
	//步骤二：插入一条数据
    stmt,err:=db.Prepare("insert into userinfo(username,departname,created) values(?,?,?)")
	if err!=nil{
		log.Fatal("prepare err",err)
		return
	}
    //步骤三:补充sql语句
	result,err:=stmt.Exec("司马懿","晋国",time.Now())
	if err!=nil{
		log.Fatal("stmt err:",err)
		return
	}
	//步骤四：处理sql语句执行后的结果
	lastInsertId,err:=result.LastInsertId()      //获取刚刚添加数据的id
	rowAffectedId,err:=result.RowsAffected()     //获取执行sql后受影响的行数
	fmt.Printf("last insert id:%v\n",lastInsertId)
	fmt.Printf("affected row id:%v\n",rowAffectedId)
	
    stmt.Close()
	db.Close()
}

```

#### 5.2.4  获取DQL操作：查询

方式一：查询单条语句

```go
func (db *DB) QueryRow(query string, args ...interface{}) *Row {
	return db.QueryRowContext(context.Background(), query, args...)
}
```

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)

func main
	//步骤一:建立连接
	db,err:=sql.Open("mysql","root:12345@tcp(127.0.0.1:3306)/usersql?charset=utf8")
	if err!=nil{
		fmt.Println("数据连接异常",err)
		return
	} 
	var uid, username,departname,created string	
	//步骤二:单条语句查询
	row:= db.QueryRow("select uid, username,departname,created from userinfo where uid=?",3)
	//步骤三:按列扫描赋值
	err= row.Scan(&uid,&username,&departname,&created)
	if err!=nil{
		fmt.Println("查询异常",err)
		return
	}
	fmt.Println(uid,username,departname,created)
}
```

方式二：查询多条数据，并遍历

Query 获取数据，for xxx.Next() 遍历数据

首先使用Query()方法进行查询，如果查询无误，返回Rows，就是所有行的信息，类似结果集。

> ```go
> func (db *DB) Query(query string, args ...interface{}) (*Rows, error)
> ```

通过next方法判断下一条数据是否存在

> ```go
> func (rs *Rows) Next() bool
> ```

*注：每次db.Query操作后，都建议调用rows.Close()。因为 db.Query() 会从数据库连接池中获取一个连接， 这个底层连接在结果集(rows)未关闭前会被标记为处于繁忙状态。当遍历读到最后一条记录时，会发生一个内部EOF错误，自动调用rows.Close()，但如果提前退出循环，rows不会关闭，连接不会回到连接池中，连接也不会关闭, 则此连接会一直被占用。因此通常我们使用 defer rows.Close() 来确保数据库连接可以正确放回到连接池中；不过阅读源码发现rows.Close()操作是幂等操作，即一个幂等操作的特点是其任意多次执行所产生的影响均与一次执行的影响相同，所以即便对已关闭的rows再执行close()也没关系。*

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)
//定义UserInfo结构体
type UserInfo struct {
	uid int
	username string
	departname string
	created string
}
var(
	uid int
	username string
	departname string
	created string
)
func main(){
    //步骤一：新建连接
	db,_:=sql.Open("mysql","root:12345@tcp(127.0.0.1:3306)/usersql?charset=utf8")
	//步骤二：新建查询
    rows,err:=db.Query("select uid,username,departname,created from userinfo ")
	if err!=nil{
		fmt.Println("数据库查询异常,err:",err)
		return
	}
    //步骤四：定义切片，存放结果集
	datas:=make([]UserInfo,0)
	for rows.Next(){
		rows.Scan(&uid,&username,&departname,&created)
		u:=UserInfo{uid,username,departname,created}
		datas=append(datas,u)
	}
	//步骤五：关闭资源
	rows.Close()
	db.Close()
	for _,val:=range datas{
		fmt.Println(val)
	}
}

```

#### 5.2.5  事务

- Begin()：开启事务
- Commit()：提交事务（执行sql)
- Rollback()：回滚

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)

func main(){
	db,_:=sql.Open("mysql","root:12345@tcp(127.0.0.1:3306)/usersql?charset=utf8")
	//事务开启
	tx,_:=db.Begin()
	var aff1,aff2 int64=0,0
	result1,_:=tx.Exec("update account set money=2000 where id=?",1)
	result2,_:=tx.Exec("update account set money=3000 where id=?",2)
	if result1!=nil{
		aff1,_=result1.RowsAffected()
	}
	if result2!=nil{
		aff2,_=result2.RowsAffected()
	}
	fmt.Println(aff1,aff2)
	if aff1==1 && aff2==1{
		tx.Commit()
		fmt.Println("操作成功")
	}else{
		tx.Rollback()
		fmt.Println("操作失败...回滚...")
	}
}
```

一个Tx会在整个生命周期中保存一个连接，然后在调用commit()或Rollback()的时候释放掉。

#### 5.2.6  sql扩展包

> ```go
> go get "github.com/jmoiron/sqlx"
> ```

相比database/sql方法还多了新语法，也就是实现将获取的数据直接转换结构体实现。

- Get(dest interface{}, …) error     (很常用)
- Select(dest interface{}, …) error     (很常用)

```go
package main

import (
	"fmt"
	_ "github.com/go-sql-driver/mysql"
	"github.com/jmoiron/sqlx"
)
//var Db *sqlx.DB
type User struct{
	Uid int
	Username string
	Departname string
	Created string
}
func main(){
	db,err:=sqlx.Open("mysql","root:12345@tcp(127.0.0.1:3306)/usersql?charset=utf8")

	if err!=nil{
		fmt.Println("open mysql failed, err:",err)
		return
	}
    // Db=db
	var users []User
	err=db.Select(&users,"select uid,username,departname,created from userinfo")
	if err!=nil{
		fmt.Println("select error :",err)
		return
	}
	fmt.Println(users)

	var user User
	err=db.Get(&user,"select uid,username,departname,created from userinfo where uid=?",1)
	if err!=nil{
		fmt.Println("get error:",err)
		return
	}
	fmt.Println(user)
}
```

### 5.3 避免SQL注入

案例分析：

```html
<form action="/login" method="POST">
    <p>Username: <input type="text" name="username" /></p>
    <p>Password: <input type="password" name="password" /></p>
    <p><input type="submit" value="登陆" /></p>
</form>
```

后端处理:

```go
username:=r.Form.Get("username")
password:=r.Form.Get("password")
sql:="SELECT * FROM user WHERE username='"+username+"' AND password='"+password+"'"
```

前端输入username，密码任意:

> ```sql
> myuser' or 'foo' = 'foo' --
> ```

此时SQL语句：

```go
SELECT * FROM user WHERE username='myuser' or 'foo'=='foo' --'' AND password='xxx'
```

在sql语句里--是注释的意思：即攻击者在不知道任何合法用户名和密码的情况下成功登录了。

预防措施

```txt
1、严格限制Web应用的数据库的操作权限，给此用户提供仅仅能够满足其工作的最低权限，从而最大限度的减少
注入攻击对数据库的危害。

2、检查输入的数据是否具有所期望的数据格式，严格限制变量的类型，例如使用regexp包进行一些匹配处理，
或者使用strconv包对字符串转化成其他基本类型的数据进行判断。

3、对进入数据库的特殊字符（'"\尖括号&*;等）进行转义处理，或编码转换。Go 的text/template包里面的
HTMLEscapeString函数可以对字符串进行转义处理。

4、所有的查询语句建议使用数据库提供的参数化查询接口，参数化的语句使用参数而不是将用户输入变量嵌入
到SQL语句中，即不要直接拼接SQL语句。例如使用database/sql里面的查询函数Prepare和Query，或者
Exec(query string, args ...interface{})。

5、在应用发布之前建议使用专业的SQL注入检测工具进行检测，以及时修补被发现的SQL注入漏洞。网上有很多
这方面的开源工具，例如sqlmap、SQLninja等。

6、避免网站打印出SQL错误信息，比如类型错误、字段不匹配等，把代码里的SQL语句暴露出来，以防止攻击者
```

## 六   go 操作Redis

### 6.1  安装redis

> ```go
> go get github.com/gomodule/redigo/redis
> ```

github地址：https://github.com/gomodule/redigo

### 6.2 连接redis

Conn接口是与Redis协作的主要接口，可以使用Dial, DialWithTimeout或者 NewConn函数来创建连接，当任务完成时，应用程序必须调用Close函数来完成操作。

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
)

func main(){
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("连接失败:",err)
		return
	}
	fmt.Println("conn success ....")
	defer conn.Close()
}
```

连接redis需注意两点：

1、修改redis-6.0.14/redis.conf文件 bind ip   将linux服务器的ip地址添加在127.0.0.1后面，修改完成后需重启redis-server

2、查看端口号6379是否允许远程访问，若不允许，需设置防火墙允许远程访问，设置完成后需重启防火墙。

### 6.3  命令操作

通过使用Conn接口中的do方法执行redis命令，redis命令大全参考：http://doc.redisfans.com/

go中发送与响应对应类型：

Do函数会必要时将参数转化为二进制字符串

| Go Type         | Conversion                          |
| :-------------- | :---------------------------------- |
| []byte          | Sent as is                          |
| string          | Sent as is                          |
| int, int64      | strconv.FormatInt(v)                |
| float64         | strconv.FormatFloat(v, 'g', -1, 64) |
| bool            | true -> "1", false -> "0"           |
| nil             | ""                                  |
| all other types | fmt.Print(v)                        |

Redis 命令响应会用以下Go类型表示：

| Redis type    | Go type                                    |
| :------------ | :----------------------------------------- |
| error         | redis.Error                                |
| integer       | int64                                      |
| simple string | string                                     |
| bulk string   | []byte or nil if value not present.        |
| array         | []interface{} or nil if value not present. |

可以使用GO的类型断言或者reply辅助函数将返回的interface{}转换为对应类型。

#### 6.3.1 set、get操作

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
)

func main(){
    //1、新建连接
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("连接失败:",err)
		return
	}
	fmt.Println("conn success")
	defer conn.Close()
    //2、存放数据
	_,err=conn.Do("set","name","曹操")
	if err!=nil{
		fmt.Println("redis set error:",err)
	}
    //3、获取数据
	name,err:=redis.String(conn.Do("get","name"))
	if err!=nil{
		fmt.Println("redis get error",err)
	}
	fmt.Println("redis get data is:",name)
}
```

#### 6.3.2  设置Key过期时间

> ```go
> _,err=conn.Do("expire","name",10)
> ```

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
	"time"
)

func main(){
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("连接失败:",err)
		return
	}
	fmt.Println("conn success...")
	defer conn.Close()

	_,err=conn.Do("set","name","刘备")
	_,err=conn.Do("expire","name",10)
	if err!=nil{
		fmt.Println("expire time error:",err)
		return
	}
    //测试：睡眠11秒
	time.Sleep(11*time.Second)
    
	name,err:=redis.String(conn.Do("get","name"))
	if err!=nil{
		fmt.Println("get data error:",err)
		return
	}
	fmt.Println("get data is:",name)
}
```

输出结果：

```go
conn success...
get data error: redigo: nil returned
```

#### 6.3.3  mget、mset

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
)

func main(){
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("conn  error:",err)
		return
	}
	fmt.Println("conn success...")
	defer conn.Close()
    
	_,err=conn.Do("mset","name","曹操","age",68)
	if err!=nil{
		fmt.Println("redis mset error",err)
		return
	}
    
	res,err:=redis.Strings(conn.Do("mget","name","age"))
	if err!=nil{
		fmt.Println("mget error:",err)
		return
	}
	fmt.Printf("mget data is %s\n",res)
}
```

运行结果

```go 
conn success...
mget data is [曹操 68]
```

####  6.3.4  list

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
)

func main(){
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("conn  error:",err)
		return
	}
	fmt.Println("conn success...")
	defer conn.Close()

	_,err=conn.Do("lpush","list1","曹操","刘备","孙权")
	if err!=nil{
		fmt.Println("redis lpush error",err)
		return
	}
	res,err:=redis.Strings(conn.Do("lrange","list1",0,2))
	if err!=nil{
		fmt.Println("redis pop error:",err)
		return
	}
	fmt.Printf("lrange data is: %s\n",res)
}
```

运行结果：

```go
conn success...
lrange data is: [孙权 刘备 曹操]
```

#### 6.3.5  hash

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
)

func main(){
    //新建连接
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("conn  error:",err)
		return
	}
	fmt.Println("conn success...")
	defer conn.Close()

	// user是键值   name是属性1  "曹操"是属性1的值  age是属性2  68是属性2的值
	_,err=conn.Do("hset","user","name","曹操","age",68)

	if err!=nil{
		fmt.Println("redis hset error",err)
		return
	}
	res1,err:=redis.String(conn.Do("hget","user","name"))
	res2,err:=redis.Int64(conn.Do("hget","user","age"))

	if err!=nil{
		fmt.Println("redis hget error:",err)
		return
	}
	fmt.Printf("hget key is user value : name=%s,age=%d\n",res1,res2)
}
```

输出结果：

```go
conn success...
hget key is user value : name=曹操,age=68
```

#### 6.3.6  Pipelining(管道)

管道操作可以理解为并发操作，并通过Send()，Flush()，Receive()三个方法实现。客户端可以使用send()方法一次性向服务器发送一个或多个命令，命令发送完毕时，使用flush()方法将缓冲区的命令输入一次性发送到服务器，客户端再使用Receive()方法依次按照先进先出的顺序读取所有命令操作结果。

```go
Send(commandName string, args ...interface{}) error
Flush() error
Receive() (reply interface{}, err error)
```

- Send：发送命令至缓冲区
- Flush：清空缓冲区，将命令一次性发送至服务器
- Recevie：依次读取服务器响应结果，当读取的命令未响应时，该操作会阻塞。

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
)

func main(){
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("conn  error:",err)
		return
	}
	fmt.Println("conn success...")
	defer conn.Close()

	conn.Send("hset","user","name","曹操","age",68)
	conn.Send("hset","user","sex","male")
	conn.Send("hget","user","name")
	conn.Flush()
	res1,err:=conn.Receive()
	fmt.Printf("Rece res1: %v\n",res1)
	res2,err:=conn.Receive()
	fmt.Printf("Rece res2: %v\n",res2)
	res3,err:=conn.Receive()
	fmt.Printf("Rece res3: %s\n",res3)
}
```

输出结果

```go
conn success...
Rece res1: 0
Rece res2: 0
Rece res3: 曹操
```

#### 6.3.7  pub/sub

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
	"time"
)

func main(){
	go Subs()
	go Push("曹操字曹孟德")

	time.Sleep(3*time.Second)
}

func Subs(){
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("conn  error:",err)
		return
	}
	fmt.Println("conn success...")
	defer conn.Close()

	psc:=redis.PubSubConn{conn}
	psc.Subscribe("channel1")
	for{
		switch v:=psc.Receive().(type) {
		case redis.Message:
			fmt.Printf("%s: message: %s\n",v.Channel,v.Data)
		case redis.Subscription:
			fmt.Printf("%s:%s %d\n",v.Channel,v.Kind,v.Count)
		case error:
			fmt.Println(v)
			return
		}
	}
}

func Push(message string){
	conn,_:=redis.Dial("tcp","192.168.30.161:6379")
	_,err:=conn.Do("PUBLISH","channel1",message)
	if err!=nil{
		fmt.Println("pub err:",err)
		return
	}
}
```

输出结果:

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
	"time"
)

func main(){
	go Subs()
	go Push("曹操字曹孟德")

	time.Sleep(3*time.Second)
}

func Subs(){
	conn,err:=redis.Dial("tcp","192.168.30.161:6379")
	if err!=nil{
		fmt.Println("conn  error:",err)
		return
	}
	fmt.Println("conn success...")
	defer conn.Close()

	psc:=redis.PubSubConn{conn}
	psc.Subscribe("channel1")
	for{
		switch v:=psc.Receive().(type) {
		case redis.Message:
			fmt.Printf("%s: message: %s\n",v.Channel,v.Data)
		case redis.Subscription:
			fmt.Printf("%s:%s %d\n",v.Channel,v.Kind,v.Count)
		case error:
			fmt.Println(v)
			return
		}
	}
}

func Push(message string){
	conn,_:=redis.Dial("tcp","192.168.30.161:6379")
	_,err:=conn.Do("PUBLISH","channel1",message)
	if err!=nil{
		fmt.Println("pub err:",err)
		return
	}
}
```

#### 6.3.8  事务操作

* MULTI：开启事务

* EXEC：执行事务

* DISCARD：取消事务

* WATCH：监视事务中的键变化，一旦有改变则取消事务

#### 6.3.9  连接池使用

```go
package main

import (
	"fmt"
	"github.com/gomodule/redigo/redis"
)
var Pool redis.Pool

func init(){
	Pool=redis.Pool{
		MaxIdle:16,
		MaxActive:32,
		IdleTimeout:120,
		Dial: func() (conn redis.Conn, err error) {
			return redis.Dial("tcp","192.168.30.161:6379")
		},
	}
}
func main(){
	conn:=Pool.Get()
	res,err := conn.Do("HSET","user","name","曹操")
	fmt.Println(res,err)
	res1,err := redis.String(conn.Do("HGET","user","name"))
	fmt.Printf("res:%s,error:%v",res1,err)
}
```

## 七  Cookie

go语言中使用Cookie

cookie的原理图：

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210630094212264.png" alt="image-20210630094212264" style="zoom:50%;" />

Cookie是由浏览器维持的，存储在客户端的一小段文本信息。

Go语言中通过net/http包中的SetCookie来设置:

> ```go
> http.SetCookie(w ResponseWriter, cookie *Cookie)
> ```

Cookie对象

```go
type Cookie struct {
	Name  string
	Value string

	Path       string    // optional
	Domain     string    // optional
	Expires    time.Time // optional
	RawExpires string    // for reading cookies only

	// MaxAge=0 means no 'Max-Age' attribute specified.
	// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
	// MaxAge>0 means Max-Age attribute present and given in seconds
	MaxAge   int
	Secure   bool
	HttpOnly bool
	SameSite SameSite
	Raw      string
	Unparsed []string // Raw text of unparsed attribute-value pairs
}
```

```go
package main

import (
	"fmt"
	"net/http"
)

func main(){
	http.HandleFunc("/setcookie",setCookie)
	http.HandleFunc("/getcookie",getCookie)
	http.ListenAndServe("localhost:8000",nil)
}

func setCookie(w http.ResponseWriter,r *http.Request){
	cookie:=http.Cookie{Name:"name",Value:"haiyi",Path:"/",MaxAge:120}
	http.SetCookie(w,&cookie)
	w.Write([]byte("write cookie ok"))
}

func getCookie(w http.ResponseWriter,r *http.Request){
	cookie,_:=r.Cookie("name")
	fmt.Fprint(w,cookie)
}
```

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210630090805603.png" alt="image-20210630090805603" style="zoom:50%;" />

cookie不过是浏览器端的一个环境变量而已， 仅此而已。cookie值存在于浏览器所在的本地电脑中。 而所谓的服务端设置cookie, 其实是服务端给浏览器发送对应的设置cookie信息， 由浏览器来真正执行设置cookie的操作, 存于本地电脑磁盘中。

## 八   Session

session机制是一种服务器端的机制，服务器使用一种类似于散列表的结构来保存信息，每一个网站访客都会被分配给一个唯一的标志符,即sessionID,它的存放形式有两种:1、经过url传递。2、保存在客户端的cookies里

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210630095011083.png" alt="image-20210630095011083" style="zoom:50%;" />

<font color='red'> Go标准包没有为session提供任何支持 </font>

用户访问Web应用时，服务端程序会随时需要创建session，这个过程可以概括为三个步骤：

- 生成全局唯一标识符（sessionid）；
- 开辟数据存储空间。一般会在内存中创建相应的数据结构，但这种情况下，系统一旦掉电，所有的会话数据就会丢失，如果是电子商务类网站，这将造成严重的后果。所以为了解决这类问题，你可以将会话数据写到文件里或存储在数据库中，当然这样会增加I/O开销，但是它可以实现某种程度的session持久化，也更有利于session的共享；
- 将session的全局唯一标示符发送给客户端。

Session管理涉及如下因素

- 全局session管理器
- 保证sessionid 的全局唯一性
- 为每个客户关联一个session
- session 的存储(可以存储到内存、文件、数据库等)
- session 过期处理

待补充session...？？？

## 九  文本处理

### 9.1 xml处理

案例：服务器信息xml文件处理

```xml
<?xml version="1.0" encoding="utf-8"?>
<servers version="1">
    <server>
        <serverName>Shanghai_VPN</serverName>
        <serverIP>127.0.0.1</serverIP>
    </server>
    <server>
        <serverName>Beijing_VPN</serverName>
        <serverIP>127.0.0.2</serverIP>
    </server>
</servers>
```

xml包的Unmarshal函数

> ```go
> func Unmarshal(data []byte, v interface{}) error
> ```

go 解析xml文件

```go
package main

import (
	"encoding/xml"
	"fmt"
	"io/ioutil"
	"os"
)

type Recurlyserver struct {
	XMLName  xml.Name  `xml:"servers"`
	Version string  `xml:"version,attr"`
	Svs []servers  `xml:"server"`
	Description string  `xml:",innerxml"`
}

type servers struct {
	XMLName xml.Name	`xml:"server"`
	ServerName string   `xml:"serverName"`
	ServerIP	string  `xml:"serverIP"`
}


func main(){
	file,err:=os.Open("server.xml")
	if err!=nil{
		fmt.Printf("error:%v",err)
		return
	}
	defer file.Close()

	data,err:=ioutil.ReadAll(file)

	if err!=nil{
		fmt.Printf("error:%v",err)
		return
	}

	v:=Recurlyserver{}
	err=xml.Unmarshal(data,&v)
	if err!=nil{
		fmt.Printf("error:%v\n",err)
		return
	}
	fmt.Println(v)
}
```

输出结果:

```go
{{ servers} 1 [{{ server} Shanghai_VPN 127.0.0.1} {{ server} Beijing_VPN 127.0.0.2}] 
    <server>
        <serverName>Shanghai_VPN</serverName>
        <serverIP>127.0.0.1</serverIP>
    </server>
    <server>
        <serverName>Beijing_VPN</serverName>
        <serverIP>127.0.0.2</serverIP>
    </server>
}
```

练习：将下述students.xml解析

```xml
<?xml version="1.0" encoding="utf-8"?>
<students version="1">
    <student>
        <studentName>haiyi</studentName>
        <age>24</age>
        <sex>male</sex>
        <books>
            <book>
                <bookName>红与黑</bookName>
                <price>55.8</price>
            </book>
            <book>
                <bookName>呼啸山庄</bookName>
                <price>99</price>
            </book>
        </books>
    </student>
    <student>
        <studentName>王二狗</studentName>
        <age>30</age>
        <sex>male</sex>
        <books>
            <book>
                <bookName>十万个为啥</bookName>
                <price>22.8</price>
            </book>
            <book>
                <bookName>从入门到放弃</bookName>
                <price>68</price>
            </book>
        </books>
    </student>
</students>
```

```go
package main

import (
	"encoding/xml"
	"fmt"
	"io/ioutil"
	"os"
)
//根据xml文件格式定义结构体
type RecuryStudents struct{
	XMLName xml.Name  `xml:"students"`
	Version string  `xml:"version,attr"`
	Students []student  `xml:"student"`
	Description  string  `xml:",innerxml"`
}

type  student struct {
	XMLName xml.Name  `xml:"student"`
	StudentName  string  `xml:"studentName"`
	Age  int  `xml:"age"`
	Sex  string  `xml:"sex"`
	Books RecuryBooks  `xml:"books"`
}

type RecuryBooks struct {
	XMLName xml.Name  `xml:"books"`
	Version string `xml:"version,attr"`
	Books []book  `xml:"book"`
	Description  string  `xml:",innerxml"`
}

type book struct {
	XMLName xml.Name  `xml:"book"`
	BookName  string  `xml:"bookName"`
	Price  float64  `xml:"price"`
}


func main(){
	file,err:=os.Open("students.xml")
	if err!=nil{
		fmt.Printf("file open error:%v\n",err)
		return
	}
	defer file.Close()

	data,err:=ioutil.ReadAll(file)

	if err!=nil{
		fmt.Printf("ioutil read error:%v\n",err)
		return
	}
	v:=RecuryStudents{}
	err=xml.Unmarshal(data,&v)
	if err!=nil{
		fmt.Printf("xml unmarshal error:%v\n",err)
		return
	}
	fmt.Println(v)
}
```

输出结果：

```go
{{ students} 1 [{{ student} haiyi 24 male {{ books}  [{{ book} 红与黑 55.8} {{ book} 呼啸山庄 99}] 
            <book>
                <bookName>红与黑</bookName>
                <price>55.8</price>
            </book>
            <book>
                <bookName>呼啸山庄</bookName>
                <price>99</price>
            </book>
        }} {{ student} 王二狗 30 male {{ books}  [{{ book} 十万个为啥 22.8} {{ book} 从入门到放弃 68}] 
            <book>
                <bookName>十万个为啥</bookName>
                <price>22.8</price>
            </book>
            <book>
                <bookName>从入门到放弃</bookName>
                <price>68</price>
            </book>
        }}] 
    <student>
        <studentName>haiyi</studentName>
        <age>24</age>
        <sex>male</sex>
        <books>
            <book>
                <bookName>红与黑</bookName>
                <price>55.8</price>
            </book>
            <book>
                <bookName>呼啸山庄</bookName>
                <price>99</price>
            </book>
        </books>
    </student>
    <student>
        <studentName>王二狗</studentName>
        <age>30</age>
        <sex>male</sex>
        <books>
            <book>
                <bookName>十万个为啥</bookName>
                <price>22.8</price>
            </book>
            <book>
                <bookName>从入门到放弃</bookName>
                <price>68</price>
            </book>
        </books>
    </student>
}
```

生成xml文件 以案例为例

> ```go
> func Marshal(v interface{}) ([]byte, error)
> func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
> ```

```go
package main

import (
	"encoding/xml"
	"fmt"
	"os"
)

type ServersXML struct {
	XMLName  xml.Name  `xml:"servers"`
	Version string  `xml:"version,attr"`
	Servers []server  `xml:"server"`

}

type server struct {
	ServerName string   `xml:"serverName"`
	ServerIP	string  `xml:"serverIP"`
}

func main(){
	v:=&ServersXML{Version:"1"}
	v.Servers=append(v.Servers,server{"Windows IP","192.168.30.172"})
	v.Servers=append(v.Servers,server{"Linux IP","192.168.30.161"})
	output,err:=xml.Marshal(v)
	if err!=nil{
		fmt.Println("product xml file error:",err)
		return
	}
	os.Stdout.Write([]byte(xml.Header))
	os.Stdout.Write(output)
}
```

输出结果：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<servers version="1">
    <server>
        <serverName>Windows IP</serverName>
        <serverIP>192.168.30.172</serverIP>
    </server>
    <server>
        <serverName>Linux IP</serverName>
        <serverIP>192.168.30.161</serverIP>
    </server>
</servers>
```

### 9.2 json处理

案例：

```json
{"servers":
    [{"serverName":"Shanghai_VPN","serverIP":"127.0.0.1"},		{"serverName":"Beijing_VPN","serverIP":"127.0.0.1"}]
}
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"os"
)

type Server struct{
	ServerName string
	ServerIP string
}
type ServerSlice struct{
  	Servers  []Server
}
func main(){
	file,err:=os.Open("server.json")
	if err!=nil{
		fmt.Println("file open error:",err)
		return
	}
	defer file.Close()

	v:=ServerSlice{}
	data,err:=ioutil.ReadAll(file)
	if err!=nil{
		fmt.Println("read json error:",err)
		return
	}
	err=json.Unmarshal(data,&v)
	if err!=nil{
		fmt.Println("json marshal error",err)
		return
	}
	fmt.Println(v)
}
```

interface接口

```go
package main

import (
	"encoding/json"
	"fmt"
)

func main(){
	b := []byte(`{"Name":"Wednesday","Age":6,"Parents":["Gomez","Morticia"]}`)
	var f interface{}
	err := json.Unmarshal(b, &f)
	if err!=nil{
		fmt.Println("json unmarshal error:",err)
		return
	}
	//接口变量f存储了map
	//通过类型断言获取各个值
	fmt.Println(f)
	m := f.(map[string]interface{})
	for k, v := range m {
		switch vv := v.(type) {
		case string:
			fmt.Println(k, "is string", vv)
		case int64:
			fmt.Println(k, "is int", vv)
		case []interface{}:
			fmt.Println(k, "is an array:")
			for i, u := range vv {
				fmt.Println(i, u)
			}
		default:
			fmt.Println(k, "is of a type I don't know how to handle")
		}
	}
}
```

simplejson处理未知结构体反序列化之后的数据

> ```go
> go get github.com/bitly/go-simplejson
> ```

```go
package main

import (
   "fmt"
   "github.com/bitly/go-simplejson"
)

func newJson(b []byte){
   js,err:=simplejson.NewJson(b)
   if err!=nil{
      fmt.Println("simple json err",err)
      return
   }
   name,_:=js.Get("Name").String()
   age,_:=js.Get("Age").Int()
   parents,_:=js.Get("Parents").Array()
   fmt.Println("name value is:",name)
   fmt.Println("age value is:",age)
   fmt.Println(parents)
}
func main(){

   b := []byte(`{"Name":"Wednesday","Age":6,"Parents":["Gomez","Morticia"]}`)
   newJson(b)
}
```

输出结果：

```go
name value is: Wednesday
age value is: 6
[Gomez Morticia]
```

生成json文件

```go
package main

import (
	"bufio"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"os"
)

type Server struct{
	ServerName string  `json:"serverName"`
	ServerIP string		`json:"serverIP"`
}
type ServerSlice struct{
  	Servers  []Server
}
func main(){
	jsonUnMarshal()
}
func jsonUnMarshal(){
	v:=ServerSlice{}
	v.Servers=append(v.Servers,Server{"Shanghai_VPN","127.0.0.1"})
	v.Servers=append(v.Servers,Server{"Beijing_VPN","127.0.0.1"})
	data,err:=json.Marshal(&v)
	if err!=nil{
		fmt.Println("json marshal error:",err)
		return
	}
	file,err:= os.OpenFile("server1.json",os.O_WRONLY|os.O_CREATE,0666)
	if err!=nil{
		fmt.Println("创建文件失败！",err)
		return
	}
	defer file.Close()

	writer:= bufio.NewWriter(file)
	writer.Write(data)
	writer.Flush()
}
```

### 9.3  正则表达式

正则表达式语法详见

> ```go
> https://studygolang.com/pkgdoc
> ```

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"regexp"
	"strings"
)

func main(){
	ip:="1127.0.0.333"
	fmt.Println(IsIP(ip))
	fmt.Println(IsNum("w2121"))
	//beeNet()

	a := "I  learning Go language zzz ddd"
	re, _ := regexp.Compile("[a-z]{2,4}")

	str:=re.FindAll([]byte(a),-1)
	for _,val:=range str{
		fmt.Println(string(val))
	}
	//查找符合正则的第一个
	one := re.Find([]byte(a))
	fmt.Println("Find:", string(one))
	//查找符合正则的所有slice,n小于0表示返回全部符合的字符串，不然就是返回指定的长度
	all := re.FindAll([]byte(a), -1)
	fmt.Println("FindAll", all)
	//查找符合条件的index位置,开始位置和结束位置
	index := re.FindIndex([]byte(a))
	fmt.Println("FindIndex", index)
	//查找符合条件的所有的index位置，n同上
	allindex := re.FindAllIndex([]byte(a), -1)
	fmt.Println("FindAllIndex", allindex)
	re2, _ := regexp.Compile("am(.*)lang(.*)")

	//查找Submatch,返回数组，第一个元素是匹配的全部元素，第二个元素是第一个()里面的，第三个是第二个()里面的
	//下面的输出第一个元素是"am learning Go language"
	//第二个元素是" learning Go "，注意包含空格的输出
	//第三个元素是"uage"
	submatch := re2.FindSubmatch([]byte(a))
	fmt.Println("FindSubmatch", submatch)
	for _, v := range submatch {
		fmt.Println(string(v))
	}
	//定义和上面的FindIndex一样
	submatchindex := re2.FindSubmatchIndex([]byte(a))
	fmt.Println(submatchindex)
	//FindAllSubmatch,查找所有符合条件的子匹配
	submatchall := re2.FindAllSubmatch([]byte(a), -1)
	fmt.Println(submatchall)
	//FindAllSubmatchIndex,查找所有字匹配的index
	submatchallindex := re2.FindAllSubmatchIndex([]byte(a), -1)
	fmt.Println(submatchallindex)

}
//ip地址判断(该方式仅判断了各个点的长度)
func IsIP(ip string) (b bool){
	if ok,_:=regexp.MatchString("^[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}$", ip);!ok{
		return false
	}
	return true
}

func IsNum(num string)(b bool){
	if m, _ := regexp.MatchString("^[0-9]+$", num); m {
		return true
	} else {
		return false
	}
}

func beeNet(){
	resp, err := http.Get("http://www.baidu.com")
	if err != nil {
		fmt.Println("http get error.")
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("http read error")
		return
	}
	src := string(body)

	fmt.Println(src)
	fmt.Println("--------------------------------------------------------")
	//将HTML标签全转换成小写
	re, _ := regexp.Compile("\\<[\\S\\s]+?\\>")
	src = re.ReplaceAllStringFunc(src, strings.ToLower)
	//去除STYLE
	re, _ = regexp.Compile("\\<style[\\S\\s]+?\\</style\\>")
	src = re.ReplaceAllString(src, "")
	//去除SCRIPT
	re, _ = regexp.Compile("\\<script[\\S\\s]+?\\</script\\>")
	src = re.ReplaceAllString(src, "")
	//去除所有尖括号内的HTML代码，并换成换行符
	re, _ = regexp.Compile("\\<[\\S\\s]+?\\>")
	src = re.ReplaceAllString(src, "\n")
	//去除连续的换行符
	re, _ = regexp.Compile("\\s{2,}")
	src = re.ReplaceAllString(src, "\n")
	fmt.Println(strings.TrimSpace(src))
}
```

## 十  模板处理template

将模板和数据进行渲染的输出格式化后的字符程序

1. 创建模板对象
2. 加载模板字串
3. 执行渲染模板

### 10.1  Go模板使用

#### 10.1.1 单个字段输出

Go语言的模板通过{{}}来包含需要在渲染时被替换的字段，{{.}}表示当前的对象。但是需要注意一点：这个字段必须是导出的(字段首字母必须是大写的)，否则在渲染的时候就会报错

```go
package main

import (
	"html/template"
	"os"
)

type Person struct {
	UserName string
	age int
}

func main(){
	//步骤1：创建模板对象
	t:=template.New("field name example")
	//步骤2：加载模板字段
	t,_=t.Parse("hello {{.UserName}}  {{.age}} !")
	p:=Person{"haiyi",18}
	//步骤3：执行渲染模板
	t.Execute(os.Stdout,p)
}
```

输出结果   

```go
hello haiyi     (age字段无法加载)
```

#### 10.1.2  字段嵌套输出

如果字段里面还有对象。使用{{with …}}…{{end}}和{{range …}}{{end}}来进行数据的输出

- {{range}} 这个和Go语法里面的range类似，循环操作数据
- {{with}}操作是指当前对象的值，类似上下文的概念

```go
package main

import (
	"html/template"
	"os"
)

/*
	内部嵌套字段模板输出
 */
type Person struct {
	UserName string
	Email []string
	Friends []*Friend
}

type Friend struct {
	FName string
}

func main(){
	f1:=Friend{"haiyi"}
	f2:=Friend{"panshi"}

	t:=template.New("person template")
	t,_=t.Parse("hello  {{.UserName}} \n" +
		"{{range  .Email}}" +
		"an email is  {{.}}\n" +
		"{{end}}" +
		"{{with .Friends}}" +
		"{{range .}}" +
		"my friend name is {{.FName}}\n" +
		"{{end}}" +
		"{{end}}")
	p:=Person{UserName:"张强",Email:[]string{"haiyi@163.com","21252691@qq.com"}}
	p.Friends=[]*Friend{&f1,&f2}
	t.Execute(os.Stdout,p)
}
```

输出结果：

```go
hello  张强 
an email is  haiyi@163.com
an email is  21252691@qq.com
my friend name is haiyi
my friend name is panshi
```

#### 10.1.3  条件处理

```go
{{ if arg }}
  some content
{{ end }}

{{ if arg }}
  some content
{{ else }}
  other content
{{ end }}
```

```go
package main

import (
	"html/template"
	"os"
)
func main() {
	t:=template.New("template if test")
	//Must 作用是检测模板是否正确,例如大括号是否匹配,注释是否正确关闭,变量是否正确书写
	t=template.Must(t.Parse("if test:不会输出内容：{{if ``}}不会输出{{end}}\n"))
	t.Execute(os.Stdout,nil)

	t=template.New("template if test")
	t=template.Must(t.Parse("{{if `anything`}} 输出结果 {{end}}\n"))
	t.Execute(os.Stdout,nil)

	t=template.New("template if-else test")
	t=template.Must(t.Parse("{{if `anything`}} if 输出内容 {{else}} else 输出内容 {{end}}"))
	t.Execute(os.Stdout,nil)

}
```

输出结果：

```go
if test:不会输出内容：
 输出结果 
 if 输出内容 
```

#### 10.1.4 模板变量

申明局部变量，变量的作用域是{{end}}之前

```go
{{with x := "output" | printf "%q"}}{{x}}{{end}}

{{with x := "output"}}{{printf "%q" x}}{{end}}

{{with x := "output"}}{{x | printf "%q"}}{{end}}
```

#### 10.1.5  模板函数

每个模板函数都有一个唯一值的名称,然后与go函数关联

> ```go
> type FuncMap map[string]interface{}
> ```

email函数的模板函数名是emailDeal，它关联的Go函数名称是EmailDealWith,通过如下方式注册

> ```go
> t = t.Funcs(template.FuncMap{"emailDeal": EmailDealWith})
> ```

EmailDealWith这个函数的参数和返回值定义如下：

> ```go
> func EmailDealWith(args ...interface{}) string 
> ```

```go
package main

import (
	"fmt"
	"html/template"
	"os"
	"strings"
)

type Friend struct {
	Fname string
}
type Person struct {
	UserName string
	Emails   []string
	Friends  []*Friend
}

func EmailDealWith(args ...interface{}) string {
	ok := false
	var s string
	if len(args) == 1 {
		s, ok = args[0].(string)
	}
	if !ok {
		s = fmt.Sprint(args...)
	}
	// find the @ symbol
	substrs := strings.Split(s, "@")
	if len(substrs) != 2 {
		return s
	}
	// replace the @ by " at "
	return (substrs[0] + " at " + substrs[1])
}

func main() {
	f1 := Friend{Fname: "minux.ma"}
	f2 := Friend{Fname: "xushiwei"}
	t := template.New("fieldname example")
	t = t.Funcs(template.FuncMap{"emailDeal": EmailDealWith})
	t, _ = t.Parse("hello {{.UserName}}!\n" +
		"{{range .Emails}}" +
        "an emails {{.|emailDeal}}\n" +            //{{.|emailDeal}}过滤后输出
		"{{end}}" +
		" {{with .Friends}}" +
		"{{range .}}" +
		"my friend name is {{.Fname}}\n" +
		" {{end}}" +
		" {{end}}")
	p := Person{UserName: "haiyi",
		Emails: []string{"haiyi@beego.me", "haiyi@gmail.com"},
		Friends: []*Friend{&f1, &f2}}
	t.Execute(os.Stdout, p)
}
```

输出结果

```go
hello haiyi!
an emails haiyi at beego.me
an emails haiyi at gmail.com
 my friend name is minux.ma
 my friend name is xushiwei
```

内置实现的函数

```go
var builtins = FuncMap{
        "and": and,
        "call": call,
        "html": HTMLEscaper,
        "index": index,
        "js": JSEscaper,
        "len": length,
        "not": not,
        "or": or,
        "print": fmt.Sprint,
        "printf": fmt.Sprintf,
        "println": fmt.Sprintln,
        "urlquery": URLQueryEscaper,
}
```

#### 10.1.6  Must操作

模板包里面有一个函数Must，它的作用是检测模板是否正确，例如大括号是否匹配，注释是否正确的关闭，变量是否正确的书写

```go
package main

import (
	"html/template"
	"os"
)

func main(){
	t:=template.New("must test")
	t=template.Must(t.Parse("annotation test /* must */ \n"))
	t.Execute(os.Stdout,nil)

	t=template.New("must test")
	t=template.Must(t.Parse("field test {{.UserName} "))
	t.Execute(os.Stdout,nil)
}
```

输出结果

```go
annotation test /* must */ 
panic: template: must test:1: unexpected "}" in operand
goroutine 1 [running]:
html/template.Must(...)
	D:/Program Files/JetBrains/sdk/go1.14.15/src/html/template/template.go:372
main.main()
	D:/Program Files/JetBrains/go_program/src/gocode/goweb/template/para5_must/main/main.go:14 +0x14d
```

#### 10.1.7  嵌套模板

header.tmpl  (本质上.tmpl文件也是html文件)

```html
{{define "header"}}
<html>
    <head>
        <title>演示信息</title>
    </head>
<body>
{{end}}
```

content.tmpl

```html
{{define "content"}}
{{template "header"}}
    <h1>演示嵌套</h1>
    <ul>
        <li>嵌套使用define定义子模板</li>
        <li>调用使用template</li>
    </ul>
{{template "footer"}}
{{end}}
```

footer.tmpl

```html
{{define "footer"}}
    </body>
</html>
{{end}}
```

```go
package main
import (
    "fmt"
    "os"
    "text/template"
)
func main() {
    s1, _ := template.ParseFiles("header.tmpl", "content.tmpl", "footer.tmpl")
    s1.ExecuteTemplate(os.Stdout, "header", nil)
    fmt.Println()
    s1.ExecuteTemplate(os.Stdout, "content", nil)
    fmt.Println()
    s1.ExecuteTemplate(os.Stdout, "footer", nil)
    fmt.Println()
    s1.Execute(os.Stdout, nil)
}
```

输出结果

```html

    <html>
    <head>
        <title>演示信息</title>
    </head>
    <body>


    
    <html>
    <head>
        <title>演示信息</title>
    </head>
    <body>

    <h1>演示嵌套</h1>
    <ul>
        <li>嵌套使用define定义子模板</li>
        <li>调用使用template</li>
    </ul>
    
    </body>
    </html>



    </body>
    </html>


Process finished with exit code 0
```

## 十一  beego框架

### 11.1 beego框架插件安装

安装beego插件

> ```go
> go get github.com/beego/beego/v2@latest
> ```

beego文档

> ```go
> https://beego.me/docs/intro/
> ```

安装bee工具

> ```go
> go get -u github.com/beego/bee/v2
> ```

安装完成之后bee在%GOPATH%bin里，在cmd里运行

![image-20210702155818911](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210702155818911.png)

> ```go
> bee new  文件夹   //快速部署项目
> ```

生成项目文件

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210702160326224.png" alt="image-20210702160326224" style="zoom:50%;" />

> ```go
> bee run    //项目执行
> ```

*注：在bee run执行前需对项目go.sum进行更新：执行go mod tidy操作, 否则执行会报错*

![image-20210702160622698](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210702160622698.png)



### 11.1  beego的参数配置和路由配置

beego 目前支持 INI、XML、JSON、YAML 格式的配置文件解析，默认采用了 INI 格式解析

#### 11.1.1  参数配置


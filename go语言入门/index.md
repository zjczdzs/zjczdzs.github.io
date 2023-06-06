# Go语言入门学习


<!--more-->

# 一.为什么需要go语言

**其他编程语言的弊端。**

1. 	硬件发展速度远远超过软件。
2. 	C语言等原生语言缺乏好的依赖管理(依赖头文件)。

- Java和C+等语言过于笨重。
- 系统语言对垃圾回收和并行计算等基础功能缺乏支持。
- 对多核计算机缺乏支持。
  **Go语言是一个可以编译高效，支持高并发的，面向垃圾回收的全新语言。**
- 秒级完成大型程序的单节点编译。
- 依赖管理清晰。
- 不支持继承，程序员无需花费精力定义不同类型之间的关系。
- 支持并发执行，支持多线程通讯。
- 对多核计算机支持友好。
- 自动垃圾回收
- 更丰富的内置类型
- 函数多返回值
- 错误处理
- 匿名函数和闭包
- 类型和接口
- 并发编程
- 反射
- 语言交互性

**Go语言不支持的特性**

- 不支持函数重载和操作符重载
- 为了避免在C/C++开发中的一些Bug和混乱，不支持隐式转换
- 支持接口抽象，不支持继承
- 不支持动态加载代码
- 不支持动态链接库
- 通过recover和panic来替代异常机制
- 不支持断言
- 不支持静态变量

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-y83xAJK8-1659583869067)(C:\Users\ZHAI\AppData\Roaming\Typora\typora-user-images\image-20220723112727867.png)]

# 二、安装go

官网要进外网可能进不去

https://golang.org/dl/

**国内下载地址**

[Downloads - The Go Programming Language (google.cn)](https://golang.google.cn/dl/)

下载安装包后选地方安装

**配置环境变量**

在d盘自建一个工作文件夹名字随意xxx，里面要创建三个文件夹分别名为bin,pkg,src

右键我的电脑->属性->高级系统设置->环境变量

在系统环境变量里新建一个名为GOROOT，目录为 go的安装目录

在用户变量里吧GOPATH的目录改到xxx

编辑系统环境变量的path，

加上一行%GOROOT%\bin

再加上一行%GOPATH%

检验是否成功

win+r -> 输cmd回车 ->输go env回车

![img](https://img-blog.csdnimg.cn/50944d82310a4f2086d9d3c28c102c81.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVGhyQXZpY2lp,size_19,color_FFFFFF,t_70,g_se,x_16)

**国内镜像配置**

呼出命令行（快捷键 Win + R），输入cmd，执行下方两行命令

```
先输go env -w GO111MODULE=on回车

再输go env -w GOPROXY=https://goproxy.cn,direct回车
```

检验

```
go env
```

![img](https://img-blog.csdnimg.cn/e3e07e4e0a95498cbfaec6517c888116.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVGhyQXZpY2lp,size_19,color_FFFFFF,t_70,g_se,x_16)

即安装成功

# 三、vscode配置go

打开vscode，安装一个Go插件，如下

![img](https://img-blog.csdnimg.cn/b52437a3737642b6ae68d81b7df228ab.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80ODk2MjA2MQ==,size_16,color_FFFFFF,t_70#pic_center)

按住Ctrl+Shift+P 输入Go:Install/Update Tools 

点查看更多，全选后回车

![在这里插入图片描述](https://img-blog.csdnimg.cn/ab921cb84ab047848e550601efe4ef11.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80ODk2MjA2MQ==,size_16,color_FFFFFF,t_70#pic_center)

显示安装完成。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3d0aa828217d46ed918256d8c8c9c48f.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80ODk2MjA2MQ==,size_16,color_FFFFFF,t_70#pic_center)

在hello.go文件中编写go程序

从这里可以看出go文件的结构

```go
package main // 包声明

import “fmt” // 引入包

func main() { // 函数
    		  // 变量
	fmt.Println(“hello World”) // 语句 & 表达式
              //注释
}
```

新建终端终端进入保存文件的目录后执行下面命令运行

（这里保存到桌面了）

```
go run test.go
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9e482db191f84214a1930795a2e5af20.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80ODk2MjA2MQ==,size_16,color_FFFFFF,t_70#pic_center)

相关命令

**go build** xxx.go 编译生成二进制文件

Go语言不支持动态链接，因此编译时会将所有依赖编译进同一个二进制文件。
指定输出目录。go build -o bin/mybinary .

**go fmt** 格式化go代码

**go test**单元测试 go test ./ ... -v运行测试

**go get** 把项目所需依赖下载到本地

**go install** 在容器里直接编译源文件

**go mod** 语言管理

**go tool** 性能分析 

**go vet** 代码静态检查，发现可能的bug或者可疑的构造

**注意**：

- 当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 protected ）。

- 需要注意的是 **{** 不能单独放在一行，所以以下代码在运行时会产生错误

  ```go
  package main
  
  import "fmt"
  
  func main()  
  {  // 错误，{ 不能在单独的行上
      fmt.Println("Hello, World!")
  }
  ```

- 在 Go 程序中，一行代表一个语句结束。每个语句不需要像 C 家族中的其它语言一样以分号 ; 结尾，因为这些工作都将由 Go 编译器自动完成。

  如果你打算将多个语句写在同一行，它们则必须使用 ; 人为区分，但在实际开发中我们并不鼓励这种做法。

# 四、if

常规

```
if condition1 {

} else  if condition2{

} else {

}
```



简短语句 ----支持在判断之前先声明一个变量

```
if v := x-100;v < 0{ 

	return v

}
```

# 五、switch

```
switch var1{
	case val1:
	case val2:
		fallthrough//关键字代表执行下一个case
	case val3:
	f()
	default://默认分支
	...
}
```

# 六、for

go只有for循环没有while循环

原始for循环

```
for i:=0;i<10;i++{
	sum+=1
}
```

初始化语句和后置语句可选，与while等价

```
for;sum<1000;{
	sum+=sum
}
```

无限循环

```
for{
	if true {
		break
	}
}
```

**for-range**

遍历数组，切片，字符串，map等

```
for index,char:=range muString{
...
}
for key,value:=range MyMap{
...
}
for index,value:=range MyArray{
...
}
```

**循环控制语句**

break：经常用于中断当前 for 循环或跳出 switch 语句

continue: 跳过当前循环的剩余语句，然后继续进行下一轮循环。

goto: 将控制转移到被标记的语句

# 七、变量与常量

- **常量**  const identifier type

- **变量**   var identifier type

## 特殊常量iota

可以认为是一个可以被编译器修改的常量。

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。

iota 可以被用作枚举值：

```
const (
    a = iota
    b = iota
    c = iota
)
```

第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1；所以 a=0, b=1, c=2 可以简写为如下形式：

```
const (
    a = iota
    b
    c
)
```

### iota 用法

```
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i) //0 1 2 ha ha 100 100 7 8
}
```



## 变量定义

**变量声明**

- var语句用于声明一个变量列表，跟函数的参数列表一样，类型在最后。
- 可以一次声明多个变量
- var c, python, java bool

**变量初始化**

- 变量声明可以包含初始值，每个变量对应一个。
- 如果初始化值已存在，则可以省略类型;
- **如果没有初始化，则变量默认为零值**。*bool 零值为 false*,字符串为 **""**
- 变量会从初始值中获得类型。var i, j int= 1, 2

**短变量声明**

- 在函数中，简洁赋值语句:=可在类型明确的地方代替var声明。
- 函数外的每个语句都必须以关键字开始（var, func等等)
- 因此:=结构不能在函数外使用。
- **如果变量已经使用 var 声明过了，再使用 \**:=\** 声明变量，就产生编译错误**

短变量声明时不需要声明数据类型，由go自动推导。

c, python, java := true, false, "no!"

## 类型转换与推导

隐式转换有风险出错，所以go只有强制转换

### 类型转换

表达式T(v)将值v转换为类型T。

一些关于数值的转换:

- var i int = 42

- var i int = 42

- var f float64 = float64(i)

- var u uint = uint(f)

或者，更加简单的形式:

- i :=42
- f := float64(i)
- u := uint(f)

### 类型推导

在声明一个变量而不指定其类型时（即使用不带类型的:=语法或var=表达式语法)，变量的类型由右值推导得出。

- var i int
- j = i   // j 就也是一个 int

# 八、数组

相同类型且长度固定连续内存片段，以编号访问每个元素

定义方法

- var identifier [len]type

示例

- myArray := [3]int{1,2,3}

  如果数组长度不确定，可以使用 **...** 代替数组的长度，编译器会根据元素个数自行推断数组的长度：

  ```
  var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
  或
  balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
  ```

# 九、Make和New

New 返回指针地址

Make返回第一个元素，可预设内存空间

```
mySlice1 :=new([]int)
mySlice2 := make([]int, 0)
Slice3 := make([]int, 10)
mySlice4 := make([]int, 10,20)
//第一个参数是数组，第二个参数是初始多大，第三个参数是最大长度。
//填0时后两项全交给系统管理
```

# 

# 十、切片

语言切片是对数组的抽象，切片是对数组一个连续片段的引用，数组定义中不指定长度即为切片（不定长数组或动态数组）

可以声明一个未指定大小的数组来定义切片，切片不需要说明长度。

```
var identifier []type
```

切片在未初始化之前默认为nil，长度为0

或使用 **make()** 函数来创建切片:

```
var slice1 []type = make([]type, len)
也可以简写为
slice1 := make([]type, len)
```
也可以指定容量，其中 capacity 为可选参数

```
make([]T, length, capacity)
```

默认 endIndex 时将表示一直到arr的最后一个元素。

```
s := arr[:endIndex] 
```

默认 startIndex 时将表示从 arr 的第一个元素开始。

```
s1 := s[startIndex:endIndex] 
```

将 arr 中从下标 startIndex 到 endIndex-1 下的元素创建为一个新的切片。

```
s := arr[startIndex:endIndex] 
```

通过切片 s 初始化切片 s1。

```
s :=make([]int,len,cap) 
```

切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。

```
myArray :=[]int{1,2，3，4，5}
//切片截取
myslice := myArray [1:3]
myslice1 := []int{}

//代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。

/* 向切片添加一个元素 */
myslice1 =append(myslice1,1)
/* 同时添加多个元素 */
myslice1 = append(myslice1,2,8,3,4)
/* 拷贝 myslice1 的内容到 myArray */
copy(myArray,myslice1)
//不需要管切片长度，但是没有原生的删除方法
```

切片删除

```
func main(){
	myArray := [5]int{1,2,3,4,5}
	mySlice := myArray[1:3]
	fmt.Printf("mySlice %+v\n" , mySlice)
	fullSlice := myArray[:]
	remove3rdltem := deleteltem(fullSlice, 2)
	fmt.Printf("remove3rdltem %+v\n",remove3rdltem)
)
func deleteltem(slice []int, index int)[]int {
	return append(slice[:index], slice[index+1:]...)
	}
```

注意：go语言里全部都是值传递，下面第一种改变切片的方式错误

```
func main() {
	myslice :=[]int{10,20，30，40，50}
	for _, value := range myslice {
	value = 2
	}
	fmt.Printf( "myslice %+vin", myslice)
}
```

```
//这样才是对的
func main() {
	myslice :=[]int{10,20，30，40，50}
	for index := range myslice {
		myslice[index] 2
	}
	fmt.Printf( ""myslice +v\n", myslice)
}
```

```
//range也可以用来枚举 Unicode 字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }
```

fmt作用

```
fmt.println("aaa","bbb",'cc')// 打印一行
fmt.printf("message %s%s%s\n","a","b","c")//按值打印
fmt.Sprintf()//拼接字符串
	var stockcode=123
    var enddate="2020-12-31"
    var url="Code=%d&endDate=%s"
    var target_url=fmt.Sprintf(url,stockcode,enddate)
    fmt.Println(target_url) //Code=123&endDate=2020-12-31
fmt.Errorf()
```

# 十一、MAP

key-value的组合

定义格式

```
myMap := make(map[string]string,10)
//value可以是复杂类型的
myFuncMap := map[string] func() int{
"funcA": func() int { return 1 },
} // 这里的value是一个函数类型的
```

赋值

```
myMap["a"] = "b"
```

取值

```
//按key取值
value,exists := myMap["a"]//会有两个值，exists是布尔型的，判断是否存在值
if exists {
	println(value)
}
//遍历取值，前一个是key后是value
for k, v := range myMap {
println(k,v)
}
```

delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。实例如下：

# 十二、结构体和指针

- 通过type ... struct关键字自定义结构体
- Go语言支持指针，但不支持指针运算
  - 指针变量的值为内存地址
  - 未赋值的指针为nil

```
type Human struce {
	firstName, lastName string
}
```

# 十三、结构体标签

结构体中的**字段**除了有名字和类型外，还可以有一个可选的标签(tag)相当于key - value的key
使用场景:Kubernetes APlServer对所有资源的定义都用Json tag和protoBuff tag
NodeName string 'ison:"nodeName,omitempty" protobuf:"bytes,10.opt,name=nodeName"

```
type MyType struct {
Name string 'json:"name"'
}
func main(){
	mt := MyType{Name: "test"}
	myType := reflect.TypeOf(mt)
	name := myType.Field(0)
	tag := name.Tag.Get("json")
	println(tag)
}
```

# 十四、自定义类型与类型别名

**自定义类型**

自定义类型是 **定义了一个全新的类型** 。我们可以基于内置的基本类型定义，也可以通过 struct 定义。例如：

```
//将类型MyInt定义为int类型
type MyInt int
```

此时MyInt就是一种新的类型，它具有 int 的特性。

实际上，Go语言中的结构体类型就是一种自定义类型。

**类型别名**

比自定义类型的格式多一个  =  

```
type TypeA = Type
```

TypeA只是Type的别名，本质上**TypeA与Type是同一个类型**。

**两者的区别**

```go
package main

import (
	"fmt"
)

//类型定义
type NewInt int

//类型别名
type MyInt = int

func main() {
	var a NewInt
	var b MyInt

	fmt.Printf("type of a:%T\n", a) //type of a:main.NewInt
	fmt.Printf("type of b:%T\n", b) //type of b:int
}
```

输出

```
type of a:main.NewInt
type of b:int
```

a的类型是main.NewInt，表示main包下定义的NewInt类型。这是一个新的数据类型。

b的类型是int。MyInt类型别名只会在代码中存在，编译完成时并不会有MyInt类型。

# 十五、函数

len() 函数可以接受不同类型参数并返回该类型的长度。如果我们传入的是字符串则返回字符串的长度，如果传入的是数组，则返回数组中包含的元素个数。

函数定义格式如下：

```
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

max() 函数的代码，该函数传入两个整型参数 num1 和 num2，并返回这两个参数的最大值：

```go
/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 声明局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result
}
```

Go **函数可以返回多个值**

```
package main

import "fmt"

func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("Google", "Runoob")
   fmt.Println(a, b)
}
```

调用函数，可以通过两种方式来传递参数：

- 值传递：值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
- 引用传递：值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际数。

默认情况下，Go 语言使用的是值传递，即在调用过程中不会影响到实际参数。	

# 十六、变量作用域

Go 语言程序中全局变量与局部变量名称可以相同，但是函数内的局部变量会被优先考虑。实例如下：

```go
package main

import "fmt"

/* 声明全局变量 */
var g int = 20

func main() {
   /* 声明局部变量 */
   var g int = 10

   fmt.Printf ("结果： g = %d\n",  g)
}
//结果： g = 10
```

形式参数会作为函数的局部变量来使用。

# 十七、指针

变量是一种使用方便的占位符，用于引用计算机内存地址。

Go 语言的取地址符是 &，放到一个变量前使用就会返回相应变量的内存地址。

指针声明格式

```
var var_name *var-type
```

```
var ip *int        /* 指向整型*/
var fp *float32    /* 指向浮点型 */	
```

在指针类型前面加上 * 号（前缀）来获取指针所指向的内容。

```
package main

import "fmt"

func main() {
   var a int= 20   /* 声明实际变量 */
   var ip *int        /* 声明指针变量 */

   ip = &a  /* 指针变量的存储地址 */

   fmt.Printf("a 变量的地址是: %x\n", &a  )

   /* 指针变量的存储地址 */
   fmt.Printf("ip 变量储存的指针地址: %x\n", ip )

   /* 使用指针访问值 */
   fmt.Printf("*ip 变量的值: %d\n", *ip )
}
```

空指针判断：	

```
if(ptr != nil)     /* ptr 不是空指针 */
if(ptr == nil)    /* ptr 是空指针 */
```

指向指针的指针变量声明格式如下：

```
var ptr **int;
```

以上指向指针的指针变量为整型。

访问指向指针的指针变量值需要使用两个 * 号

```
package main
import "fmt"
func main() {
   var a int
   var ptr *int
   var pptr **int
   a = 3000
   /* 指针 ptr 地址 */
   ptr = &a
   /* 指向指针 ptr 地址 */
   pptr = &ptr
   /* 获取 pptr 的值 */
   fmt.Printf("变量 a = %d\n", a )
   fmt.Printf("指针变量 *ptr = %d\n", *ptr )
   fmt.Printf("指向指针的指针变量 **pptr = %d\n", **pptr)
}
//输出
变量 a = 3000
指针变量 *ptr = 3000
指向指针的指针变量 **pptr = 3000
```

# 十八、接口

把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。

```
/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}
```

# 十九、错误处理

Go 语言通过内置的错误接口提供了非常简单的错误处理机制。

error类型是一个接口类型，这是它的定义：

```
type error interface {
    Error() string
}
```

我们可以在编码中通过实现 error 接口类型来生成错误信息。

函数通常在最后的返回值中返回错误信息。使用errors.New 可返回一个错误信息：

```
func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, errors.New("math: square root of negative number")
    }
    // 实现
}
```

# 二十、并发

通过 go 关键字来开启 goroutine 。

goroutine 是轻量级线程，goroutine 的调度是由 Golang 运行时进行管理的。

goroutine 语法格式：

```
go 函数名( 参数列表 )
```

Go 允许使用 go 语句开启一个新的运行期线程， 即 goroutine，以一个不同的、新创建的 goroutine 来执行一个函数。 同一个程序中的所有 goroutine 共享同一个地址空间。

```
package main

import (
        "fmt"
        "time"
)

func say(s string) {
        for i := 0; i < 5; i++ {
                time.Sleep(100 * time.Millisecond)
                fmt.Println(s)
        }
}

func main() {
        go say("world")
        say("hello")
}
```

执行以上代码，你会看到输出的 hello 和 world 是没有固定先后顺序。因为它们是两个 goroutine 在执行

**通道**

通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 `<-` 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。

```
ch <- v    // 把 v 发送到通道 ch
v := <-ch  // 从 ch 接收数据
           // 并把值赋给 v
```

声明一个通道很简单，我们使用chan关键字即可，通道在使用前必须先创建：

```
ch := make(chan int)
```

默认情况下，通道是不带缓冲区的。发送端发送数据，同时必须有接收端相应的接收数据。

以下实例通过两个 goroutine 来计算数字之和，在 goroutine 完成计算后，它会计算两个结果的和：

```
package main

import "fmt"

func sum(s []int, c chan int) {
        sum := 0
        for _, v := range s {
                sum += v
        }
        c <- sum // 把 sum 发送到通道 c
}

func main() {
        s := []int{7, 2, 8, -9, 4, 0}

        c := make(chan int)
        go sum(s[:len(s)/2], c)
        go sum(s[len(s)/2:], c)
        x, y := <-c, <-c // 从通道 c 中接收

        fmt.Println(x, y, x+y)
}
```

输出  -5 17 12

**通道缓冲区**

通道可以设置缓冲区，通过 make 的第二个参数指定缓冲区大小

```
ch := make(chan int, 100)
```

带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据。

不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

**遍历通道与关闭通道**

Go 通过 range 关键字来实现遍历读取到的数据，类似于与数组或切片。格式如下：

```
v, ok := <-ch
```

如果通道接收不到数据后 ok 就为 false，这时通道就可以使用 **close()** 函数来关闭
通道很简单，我们使用chan关键字即可，通道在使用前必须先创建：

```
ch := make(chan int)
```

默认情况下，通道是不带缓冲区的。发送端发送数据，同时必须有接收端相应的接收数据。

以下实例通过两个 goroutine 来计算数字之和，在 goroutine 完成计算后，它会计算两个结果的和：

```
package main

import "fmt"

func sum(s []int, c chan int) {
        sum := 0
        for _, v := range s {
                sum += v
        }
        c <- sum // 把 sum 发送到通道 c
}

func main() {
        s := []int{7, 2, 8, -9, 4, 0}

        c := make(chan int)
        go sum(s[:len(s)/2], c)
        go sum(s[len(s)/2:], c)
        x, y := <-c, <-c // 从通道 c 中接收

        fmt.Println(x, y, x+y)
}
```

输出  -5 17 12

**通道缓冲区**

通道可以设置缓冲区，通过 make 的第二个参数指定缓冲区大小

```
ch := make(chan int, 100)
```

带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据。

不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

**遍历通道与关闭通道**

Go 通过 range 关键字来实现遍历读取到的数据，类似于与数组或切片。格式如下：

```
v, ok := <-ch
```

如果通道接收不到数据后 ok 就为 false，这时通道就可以使用 **close()** 函数来关闭


---
title: Julia 速览
date: 2018-08-24 16:46:38
tags:

---

[原文地址](https://learnxinyminutes.com/docs/julia/)

### 注释

```julia
# 这是单行注释
#=
	这是多行注释
=#
```


### 基本数据类型与运算符

Julia 中，一切都是表达式！

```julia
# 对于数字，有几种基本数据类型
3  # => 3 (Int64)
3.2  # => 3.2 (Float64)
2 + 1im  # => 2 + i (Complex{Int64})
2 // 3  # => 2//3 (Rational{Int64})  分数

# 数值运算符
1 + 1  # => 2
8 - 1  # => 7
10 * 2  # => 20
35 / 5  # => 7.0  除法运算结果为浮点数
5 / 2  # => 2.5
div(5, 2)  # => 2 截断除法
2^2  # => 4 次方
12 % 10  # => 2 取余
divrem(12, 10)  # => (1, 2)  商和余数
(1 + 3) * 2  # => 8 括号改变运算优先级

# 位运算符
~2  # => -3 按位否
3 & 5  # => 1 按位与
2 | 4  # => 6 按位或
xor(2, 4)  # => 6 按位异或
2 >> 1  # => 1 按位右移
2 << 1  # => 4 按位左移

# 查看数据的二进制表示
bitstring(12345)  
# => "0000000000000000000000000000000000000000000000000011000000111001"
bitstring('a')
# => "01100001000000000000000000000000"

# 布尔
true
false

# 布尔运算
!true  # => false
!false  # => true
1 == 1  # => true
1 == 1.0  # => true
2 == 1  # => false
1 != 1  # => false
1 != 2  # => true
1 <= 10  # => true
1 < 2 < 3  # => true

# 字符和字符串
'a'  # 字符
"this is a string"  # 字符串
"this is a string"[1]  # => 't' 字符串可以像数组一样被索引，索引从 1 开始

name = "jiazhuang"
println("my name is $name")  # 像 perl 一样支持字符串插入操作

"good" > "bye"
```

### 变量与容器

```julia
# 变量赋值前不需要声明
some_var = 5  # => 5
some_var  # => 5

# 访问未定义变量会报错
try
	some_other_var  # => ERROR
catch e
	println(e)
end

# 变量名以字母或下划线起始，之后可以使用字母、数字、下划线和感叹号
SomeOtherVar123! = 6  # => 6

# 也可以使用 unicode 字符，类似 Letax 语法
π  =>  # π = 3.1415926535897...  julia 自带的变量
```

Julia 变量命名习惯

- 变量名中单词分割可以用下划线，但不提倡这样做，除非变量名特别难读，不得已才用
- 类型名首字母大写，单词分割采用 CamelCase 风格
- 函数和宏名使用小写字母，并且不要使用下划线
- 若函数修改输入参数值时，函数名以 ！结束。这类函数被称为 inplace 函数

```julia
# 数组用来储存一组数，索引从 1 到 n
a = Int64[]  # => 0-element Int64 Array

# 一维数组元素用逗号或分号分割
b = [4, 5, 6]
b = [4; 5; 6]
b[1]  # => 4
b[end]  # => 6  用 end 索引数组最后一个元素

# 二维矩阵，行元素用空格分割，列元素用分号分割
matrix = [1 2; 3 4]

# 定义指定类型的数组
b = Int8[4, 5, 6]

# 向数组尾部添加元素
push!(a, 1)  # => [1]
push!(a, 2)  # => [1, 2]
push!(a, 4)  # => [1, 2, 4]
push!(a, 3)  # => [1, 2, 4, 3]
append!(a, b)  # => [1, 2, 4, 3, 4, 5, 6]

# 从尾部移除元素
pop!(b)  # => 6 and b in now [4, 5]

# 从头部删除元素
shift!(a)  # => 1 and a is now [2, 4, 3, 4, 5, 6]
# 从头部添加元素
unshift!(a, 7)  # => [7, 2, 4, 3, 4, 5, 6]

# 命名以感叹号结束的函数，表示他们会改变传入的参数
arr = [5, 4, 6]
sort(arr)  # => [4, 5, 6]; arr is still [5, 4, 6]
sort!(arr)  # => [4, 5, 6]; arr is now [4, 5, 6]

# 可以使用 range 对象初始化数组
a = [1:5;]  # => [1, 2, 3, 4, 5]  一定要加分号！

# 通过 range 索引数组
a[1:3]  # => [1, 2, 3]
a[2:end]  # => [2, 3, 4, 5]

# 数组切除
splice!(a, 2:3)  # => [2, 3]; a is now [1, 4, 5]

# 数组连接
a = [1, 2, 3]
b = [4, 5, 6]
append!(a, b)  # Now a is [1, 2, 3, 4, 5, 6]

# 检查元素是否在数组中
in(1, a)  # => true
1 in a   # => true

# 数组长度
length(a)  # => 6

# 元组不可变
tup = (1, 2, 3)
tup[1]  # => 1

# 元组可以给变量赋值
a, b, c = 1, 2, 3

(1,) == 1  # => false
1 == 1  # => true

# 使用元组可以很方便地交换变量值
a, b = b, a

# 字典
emplty_dict = Dict()  # => Dict{Any, Any}()

filled_dict = Dict("one" => 1, "two" => 2, "three" => 3)  # => Dict{ASCIIString, Int64}
filled_dict["one"]  # => 1

keys(filled_dict)  
# => KeyIterator{Dict{ASCIIString,Int64}}(["three"=>3,"one"=>1,"two"=>2])
# 字典中元素顺序是随意的

values(filled_dict)
# ValueIterator{Dict{ASCIIString,Int64}}(["three"=>3,"one"=>1,"two"=>2])

("two" => 2)  in filled_dict  # => true
in(("two" => 2), filled_dict)  # => true
haskey(filled_dict, "one")

get(filled_dict, "one", 4)  # => 1
get(filled_dict, "four", 4)  # => 4

# 集合
empty_set = Set()
filled_set = Set([1, 2, 2, 3, 4])  # => Set{Int64}(1, 2, 3, 4)

# 添加元素
push!(filled_set, 5)  # => Set{Int64}(5, 4, 3, 2, 1)

# 交集、并集、差集
other_set = Set([3, 4, 5, 6])
intersect(filled_set, other_set)  # => Set{Int64}(6, 4, 5, 3)
union(filled_set, other_set) # => Set{Int64}(1,2,3,4,5,6)
setdiff(Set([1,2,3,4]),Set([2,3,5])) # => Set{Int64}(1,4)
```


### 控制流

### 函数

### 类型

### 多重分派

### 包管理
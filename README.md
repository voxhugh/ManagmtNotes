# Python



### 概要

- 临时结果_
- 变量间的赋值是引用
- `in` 和 `not in` 用于成员检测
- `is` 和 `is not` 用于确认相同
- `a < b == c` 等价 `(a < b) && (b == c)`
- `not` ,  `and`  ,  `or` 逻辑运算，有短路效应
- pass语句不执行任何动作；match类似于switch
- 函数的形参默认值在定义时给定，只计算一次，后续调用之间共享
- 关键字参数 arg1 = 25 在函数调用时指定形参的值
- 表达式内部赋值必须显式使用 海象运算符 `:=`



### 控制流



##### if

```Python
if x<0:
    print('Negative')
elif x==0:
    print('Zero')
else:
    print('More')

# 循环后的else: 内层for与else作为一个整体，循环正常结束后执行
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, 'equals', x, '*', n//x)
            break
    else:
        print(n, 'is a prime number')
```



##### for

基于范围引用迭代

```Python
# for w in words.copy() 迭代的同时修改，可以迭代一个副本
for w in words:
    print(w,len(w))
```

- `items()`                  迭代 字典 提取 键值对
- `enumerate()`          迭代 序列 提取 索引&值
- `zip()`                       迭代多个序列一一匹配元素
- `reversed()`            逆向 序列
- `sorted()`                 排序 序列 返回一个新序列
- `set()`                       去重



##### range()

```python
# 返回一个可迭代对象
range(7)			# len
range(5,20)			# [5,20)
range(5,20,3)		# [5,20)，步长3
```



##### def

`def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2)`		函数定义

​            仅限位置          /    位置或关键字   *     仅限关键字

> 注意：\**dic 解包一个字典，\*tup 解包元组

```python
# 标注: arg, ret 的类型说明
def f(arg1: str, arg2: int = 2) -> list[str]:
    return [arg1] * arg2
```



##### lambda

`lambda arg: ret`		匿名函数



### 数据结构



#### list

`[a, b, c]`

```python
# 列表推导式: [expr for if/for...]
[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
```

`append(x)`                # 末尾追加

`extend(it)`              # 末尾追加

`insert(pos, x)`      # 插入元素

`remove(x)`                # 删除元素

`pop([pos])`              # 删除位置

`clear()`                    # 清空

`index(x)`                  # 首个元素的索引

`count(x)`                  # 统计元素个数

`sort()`                      # 排序

`reverse()`                # 翻转元素

`copy()`                      # 浅拷贝



#### tuple

`(x, y, z)`

```python
# 打包
t = 12345, 54321, 'hello!'
# 解包
x, y, z = t

# 0 个元素
empty = ()
# 1 个元素
tup = 'one',
```



#### set

`{'apple', 'orange', 'pear', 'banana'}`



#### dict

`{'jack': 4098, 'sape': 4139}`

```python
# 字典推导式: {key: val for}
{x: x**2 for x in (2, 4, 6)}

# 构造函数创建
dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
# key为字符串时
dict(sape=4139, guido=4127, jack=4098)
```

> 注意：`get()`提取值不会引发异常



### 其他



#### del

- 可以从 list 删除切片
- 可以从 dict 删除键值对



#### 切片

- `[:]` 默认首尾，越界自动优化，左闭右开
- 作左值是引用，作右值是值拷贝



#### 编码风格

- 缩进：四个空格
- 换行：一行不超 79 个字符
- 分割：空行
- 注释：单独一行
- 命名：`ClsName` ,  `func_with_add()`


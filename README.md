# Python



#### 总结

- 临时结果_
- 变量间的赋值是引用
- 切片 `[:]` 默认首尾，越界自动优化，左闭右开
- 列表的切片作左值是引用，作右值是值拷贝
- for循环基于范围引用迭代，迭代字典用 `items()` 提键值对，迭代序列用 `enumerate()` 提索引&值
- pass语句不执行任何动作；match类似于switch
- 函数的形参默认值在定义时给定，只计算一次，后续调用之间共享
- 关键字参数 arg1 = 25 在函数调用时指定形参的值

#### 控制流语句

##### if

```Python
if x<0:
    print('Negative')
elif x==0:
    print('Zero')
else:
    print('More')
```

- **循环后的else**

  ```Python
  # 内层for与else作为一个整体，循环正常结束后执行
  for n in range(2, 10):
      for x in range(2, n):
          if n % x == 0:
              print(n, 'equals', x, '*', n//x)
              break
      else:
          print(n, 'is a prime number')
  ```

  

##### for

```Python
for w in words:			# for w in words.copy() 迭代的同时修改，可以迭代一个副本
    print(w,len(w))
```

##### range()

```python
# 返回一个可迭代对象
range(7)			# len
range(5,20)			# [5,20)
range(5,20,3)		# [5,20)，步长3
```

##### def

`def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2)`

​            仅限位置          /    位置或关键字   *     仅限关键字

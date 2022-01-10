---
title: python与js用法对比
date: '2022-01-08 15:43:55'
spoiler: 省略.
cta: 'python'
---

## 1. 数组 集合 字典
#### 1.1. 判断是否是xx的成员
python2
```jsx
data = {'name': 'aa', 'age': 22}
data.has_key('name')
输出：True
```
python3
```jsx
data = {'name': 'aa', 'age': 22}
'name' in data.keys()
输出True
```
#### 1.2. 去重：
python
```jsx{2,4}
data = [1,3,3,4,5,2,3,4,5,6,7,1,8]
new_data = list(set(data))
输出：[1, 2, 3, 4, 5, 6, 7, 8]
new_data = list({}.fromkeys(data).keys())
输出：[1, 3, 4, 5, 2, 6, 7, 8]
for循环省略
```
js
```jsx
data = [1,3,3,4,5,2,3,4,5,6,7,1,8]
Array.from(new Set(data))
输出：[1, 2, 3, 4, 5, 6, 7, 8]
...还有别的后面再补
```
#### 1.3. 往数组中添加新的值
python:
```jsx{2,4,6,10,12}
data1 = [1, 2, 3]
data1.append(5)
data1 输出：[1, 2, 3, 5]
data1.insert(2, 6)
data1 输出：[1, 2, 6, 3, 5]
data1.extend([7])
data1 输出：[1, 2, 6, 3, 5, 7]
data1 = [1, 2, 3, 4, 5]
data2 = [6, 7]
data1[2:0] = data2 // 将原列表中从start为开始到stop前一位的数据删去，使用填充的元素进行添加
data1 输出[1, 2, 6, 7, 4, 5]
data1[0:0] = data2
data1 输出：[6, 7, 1, 2, 3, 4, 5]
```
js:
```jsx
let data1 = [1, 2, 3]
data1.push(5) // 队尾加值
data1 输出：[1, 2, 3, 5]
unshift() // 队头加值
pop() // 删除最后一个值
shift() // 删除第一个值
```
#### 1.4. 数组相加
python
```jsx
data1 = [1, 2, 3]
data2 = [3, 4, 5]
data1.extend(data2)
data1 输出：[1, 2, 3, 3, 4, 5]
上面的切片也可以对数组进行相加
```
js
```jsx
```
#### 1.5. 集合的差值 交值
```jsx{3,5}
listA = {1, 2, 3, 4, 5}
listB = {4, 5, 6, 7, 8}
listA - listB
输出： {1, 2, 3}
listA.difference(listB)
输出： {1, 2, 3}
```
#### 1.6. list的交集 / 并集 / 差集 / 对称差集
```jsx{3,4,6,8,10,12,14}
listA = [1, 2, 3, 4, 5]
listB = [4, 5, 6, 7, 8]
listA & listB //  X 错误
list(set(listA) & set(listB)) // 交集
输出：[4, 5]
list(set(listA) ｜ set(listB)) // 并集
输出：[1, 2, 3, 4, 5, 6, 7, 8]
list(set(listA) - set(listB)) // 差集
输出：[1, 2, 3]
list(set(listB) - set(listA)) // 差集
输出：[8, 6, 7]
list(set(listA).difference(set(listB))) // 差集
输出：[1, 2, 3]
list(set(listA) ^ set(listB)) // 对称差集
输出：[1, 2, 3, 6, 7, 8]
```
#### 1.7. find / findIndex
python: 检测字符串中是否包含子字符串 
```jsx
data = '1, 2, 3'
data.find('1')
输出：0
```
js: 针对数组
```jsx
let aaa = [{name: 1, age: 1}, {name: 2, age: 2}]
aaa.find((item) => item.name === 1)
输出: {name: 1, age: 1}
aaa.findIndex((item) => item.name === 1)
输出: 0
```
#### 1.8. join
python:
```jsx{2}
data = ['1', '2']
','.join(data)
# 输出'1,2'
```
js:
```jsx{2}
let data = [1, 2]
data.join(',')
// 输出'1,2'
```
#### 1.9. split
python：
```jsx
data = '1, 2, 3'
data.split(',')
输出：['1', ' 2', ' 3']
```
js:
```jsx
let data = '1, 2, 3'
data.split(',')
输出：['1', ' 2', ' 3']
```
#### 1.10. 长度
python:
```jsx
len(List)
```
js:
```jsx
arr.length
```
#### 1.11. 集合添加
```jsx
aa = {1, 2, 4}
aa.add(5)
aa 输出：{1, 2, 4, 5}
```
#### 1.12. dict
字典
后面再补充

## 2. 函数
#### 2.1 函数写法
python：
```jsx
def xx_xx():
    return '1'
def xx_xx(message=[]): // 函数参数 默认取值
    return '1'
```
js:
```jsx
function xx_xx() {
}
function xx_xx(message=[]) { // 函数参数 默认取值
}
```
#### 2.2. 匿名函数
```jsx
list(map(lambda x: x * x, [1, 2, 3]))
相当于
def fn(x):
    return x * x
```
#### 2.3. 过滤
```jsx
data = ['', '1', 2]
list(filter(lambda x : x, data))
# 输出：['1', 2]
``` 

## 3. if else switch
python:
```jsx
if len(list) == 0:
    pass
elif len(list) == 1:
    pass
else:
    pass
python没有switch
```
js:
```jsx
if (arr.length === 0) {
} else if (arr.length === 1) {
} else {
}
switch(xxx){
    case '1':
        break;
}
```

## 4. 循环
python:
```jsx
list = [1, 2, 3]
for i in list:
    print(i)
```
js:
```jsx
let list = [1, 2, 3]
for(let i = 0;i < 10; i++)
list.forEach(item => {})
list.map(item => {})
list.some(item => {})
...等等
```

## 5. 逻辑运算
python： 
```jsx
and or
if aa == bb and cc == dd:
    pass
if aa = bb or cc == dd:
    pass
```
js：
```jsx
&& ||
if(aa === bb && cc === dd) {}
if(aa === bb || cc === dd) {}
```

## 6. 数据类型
python
```jsx
str
int / float / complex
list / tuple / range
dict
set / frozenset
bool
bytes / bytearray / memoryview
```
js
```jsx
string / boolean / number / null / undefined / Symbol /
Array / Object / Function /
```
再写下range dict set bytes 等用法

## 7. 类型返回
python: type(xxx)
```jsx{2}
list = [1, 2, 3]
type(list) // <class 'list'>
data = {'age': 1}
type(data) // <class 'dict'>
data = 1
type(data) // <class 'int'>
data = 'hello'
type(data) // <class 'str'>
```
js：typeof / instance of
```jsx{2,5}
let data = '1'
typeof(data) // 'string'
...省略
let obj = {a: 1}
obj instanceof Object // true
obj instanceof Function //false
...省略
```

## 8. 字符串连接
python：
```jsx
msg1 = 'Dears：11111'
msg2 = 2
msg = '%s%s' % (msg1, msg2) // %s
data = "hello""world" // 直接拼接
msg3 = '2'
msg = msg1 + msg3 // +
```
js:
```jsx
let msg1 = 'Dears：11111'
let msg2 = 2
let msg = msg1 + msg2 // +
msg = `msg1${msg2}`  // ``拼接
let msg3 = msg1.concat('sun'); // concat()
...省略
```

## 9. 整除
python:
```jsx
data1 = 5
data2 = '8'
rate = format(data1 / data2, '.3f') // .3f：设置小数点后的位数
```
js:
```jsx
let rate = 5 / 8
```

## 10. python需要写utf-8
再写写字符转码
encode
decode
等
还有utf-8 和latin-1的区别

## 11. class

## 12. 定义
python中 直接赋值
```jsx
list = []
```
js需要加关键字 let const
```jsx
let arr = []
const XXX = 8
```
## 13. 注释
python:
```jsx
# 这是一条注释
```
js:
```jsx
// 这是一条注释
```

## 14. 打印
python：
```jsx
print('xxx')
```
js:
```jsx
console.log('xxx')
```

## 15. python与nodejs
关于接口，关于取值等

## 16. 未完待续

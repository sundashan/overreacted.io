---
title: python第二弹
date: '2022-01-27 14:57:22'
spoiler: 省略.
cta: 'python'
---

## 1. 装饰器
#### 1.1. wraps
```python
```
#### 1.2. 装饰器类
class
```python
```

## 2. 文件相关
#### 2.1 json
读取json
准备一个json文件，并命名为test
```python
import json

def index():
    path = 'test.json'
    f = open(path, 'r')
    data = json.load(f)
    print(data)
```
输出到json
```python
import json

def xxx(): 
    data = {}
    list = {
        'data': data
    }
    with open('./list.json', 'w') as write_f:
            json.dump(list, write_f, indent=4, ensure_ascii=False)
```
#### 2.2 excel
读取excel
```python
import xlrd
from pypinyin import lazy_pinyin
# 允许上传的文件类型
ALLOWED_EXTENSIONS = set(['xlsx', 'xls'])
# 检查文件类型是否有效
def allowed_file(filename):
    return '.' in filename and \
           filename.rsplit('.', 1)[1] in ALLOWED_EXTENSIONS
def import_xxx():
    res = result()
    log.info('导入xxx信息')
    file = request.files['file']
    if file and allowed_file(file.filename):
        filename = secure_filename(''.join(lazy_pinyin(file.filename)))
        data = xlrd.open_workbook(file_contents=file.read())
        table = data.sheets()[0]

        for i in range(1, table.nrows): 
            row = table.row_values(i)
            name = strip(row[0])
            if name != '':
                # .....
            continue
    else:
        print("请上传excel文件！")
```
输出到excel
```python
from flask import make_response
from openpyxl.workbook import Workbook
from openpyxl.writer.excel import ExcelWriter, save_virtual_workbook
def export_xx():
    wb = Workbook() # 新建一个Workbook
    ws = wb.worksheets[0] # 第一个sheet是ws
    ws.title = "xxx" # 设置ws的名称
    # 设置列名
    ws['A1'] = '姓名'
    ws['B1'] = '性别'
    ws['C1'] = '年龄'
    ws['D1'] = '备注'
    ws.column_dimensions['A'].width = 18.0 # 设置列宽度
    # 从第一行开始填充数据
    i = 1
    for item in data:
        astr = '{}'.format(i + 1)
        i += 1
        if 'name' in item:
            ws['A' + astr] = item['name']
        if 'sex' in item:
            ws['B' + astr] = item['sex']
        if 'age' in item:
            ws['C' + astr] = item['age']
        if 'remark' in item:
            ws['D' + astr] = item['remark']
    content = save_virtual_workbook(wb)
    resp = make_response(content)
    resp.headers["Content-Disposition"] = 'attachment; filename={}.xlsx'.format("export_db".encode().decode('latin-1'))
    resp.headers['Content-Type'] = 'application/x-xlsx'
    return resp
```

## 3. 线程Thread
线程与进程，等待，互斥等，start与run的区别
```python
from threading import Thread

def xxxx():
    # 1234

def xxx():
    res = result()
    Thread(target=xxxx).start()
    res.set(message=["xx任务创建成功"])
    return res.data()
```

## 4. 定时任务
讲解下定时任务，包括job等，最后是用法
```python
# -*- coding: utf-8 -*
from flask_apscheduler import APScheduler

# 实例APScheduler定时任务
scheduler = APScheduler()
ONE_HOUR = "09"
ONE_MINUTE = "00"
def some_function():
    #1234

def run_task():
    scheduler.add_job(id='some_function', func=some_function, args=None, trigger='cron', hour=ONE_HOUR, minute=ONE_MINUTE)
```

## 5. mysql相关

## 6. es相关


## 7. 其他
#### 7.1. 函数传参
##### 7.1.1. *args：函数传参个数不定
```python

```
#### 7.1.2 **kwargs: 函数传参为键值对，并且个数不定
```python
```

#### 7.2. 方法
##### 7.2.1. Map
这个前面也提过，省略

##### 7.2.2. filter
前面也提过，省略

##### 7.2.3. reduce
```python
data = reduce((lambda x, y: x * y), [1, 2, 3])
# 输出: 6
```

#### 7.3. __slots__ 

#### 7.4.枚举 enumerate

#### 7.5. dir()

#### 7.6. type和id
type 前面提过
id 需要补充

#### 7.7. 二维数组转一维数组
二维数组转一维数组（可能描述不准确，反正是这么个意思）
```python
import itertools

def index():
    listA = [[1, 2], [3], [5, 6]]
    print(list(itertools.chain(*listA)))
    # 输出：[1, 2, 3, 5, 6]
    print(list(itertools.chain.from_iterable(listA)))
    # 输出：[1, 2, 3, 5, 6]

    listB = [[1, 2], [3, 4], [[5, 6], [7, 8, 9, 1]]]
    print(list(itertools.chain.from_iterable(listA)))
    # 输出：[1, 2, 3, 4, [5, 6], [7, 8, 9, 1]]
```

多维数组转一维数组
```python
```

#### 7.7. inspect 代码里面无，可以探索下


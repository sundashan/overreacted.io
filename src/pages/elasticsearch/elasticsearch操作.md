---
title: elasticsearch操作
date: 2021-12-19 14:11:40
spoiler: 省略.
cta: 'elasticsearch'
---

## 1. 索引 Index
#### 1.1. 创建索引
PUT 请求 http://127.0.0.1:9200/role
其中，put就是新创建，role就是我们创建索引的名字 Index的名字

#### 1.2. 删除索引模板：
DELECT  _template/template_1

#### 1.3. 查看全部索引，详细展示
GET http://127.0.0.1:9200/_cat/indices?v
查看全部索引，详细展示

#### 1.4. 查看
GET _template/template_1

#### 1.5. 查看多个
GET _template/tem*

#### 1.6 创建索引模版
PUT   _template/template_1
![](./images/1.png)

## 2. 数据
### 2.1. 添加数据
http://127.0.0.1:9200/role/_doc 索引中添加文档数据 POST可，PUT报错
http://127.0.0.1:9200/role/_doc/1001   自定义id，索引中添加文档数据 POST，PUT均可

#### 2.2. 查询
http://127.0.0.1:9200/role/_doc/1001 GET 查询这一条的信息

### 2.2. 查询
GET http://127.0.0.1:9200/role/_search
#### 2.2.1. 查询
match query 知道分词器，会对field进行分词操作
```jsx
{
    "query": {
        "match": {
            "name": "孙山"
        }
    }
}
输出：
```
term query 会去倒排索引中寻找确切的term，没有分词器，很适合keyword，numeric、date等明确值
查询某个字段里含有某个关键词的文档
```jsx
{
    "query": {
        "bool": {
            "must": {
                "term": {
                    "name": "孙山"
                }
            }
        }
    }
}
输出：
```
#### 2.2.2. 全量查询
match_all 查询所有文档
```jsx{3}
{
    "query": {
        "match_all": {
        }
    }
}
```
#### 2.2.3. 分页查询
```jsx{3,4}
{
    "query": {},
    "from": 0,
    "size": 2
}
```
#### 2.2.4. 对想要的数据进行指定查询：
_source: 结果只返回一部分字段时，可加上_source
```jsx{3}
{
    "query": {},
    "_source": ["title"]
}
```
#### 2.2.5. 排序：
sort 排序: desc降序， asc升序
```jsx{3,5}
{
    "query": {},
    "sort": {
        "price": {
            "order": "desc"
        }
    }
}
```
#### 2.2.6.多条件同时成立 
```jsx{5,8}
{
    "query": {
        "bool": {
            "must": [{
                "match": {
                    "role": "admin"
                }
            }, {
                "match": {
                    "desc": "test"
                }
            }]
        }
    }
}
```
#### 2.2.7. 或者：should
```jsx{4}
{
    "query": {
        "bool": {
            "should": [{
                "match": {
                    "role": "admin"
                }
            }, {
                "match": {
                    "desc": "test"
                }
            }]
        }
    }
}
```
#### 2.2.8. 范围查询
```jsx{3,4,6}
{
    "query": {},
    "filter": {
        "range": {
            "price": {
                "gt": 5000
            }
        }
    }
}
```
#### 2.2.9. 完全匹配：
```jsx{2}
{
    "query": {
        "match_phrase": {
        }
    },
}
```
#### 2.2.10. exists
```jsx{5, 6}
{
    "query": {
        "bool": {
            "must": {
                "exists": {
                    "field": "name" // 筛选name字段存在的数据
                }
            }
        }
    }
}
```
#### 2.2.11. 聚合查询：
```jsx{3}
body:
{
    "query": {},
    "aggs": { // 聚合操作
        "price_group": { // 名称，随意起的
            "terms": { // 分组
                "field": "price" // 分组字段
            },
            "buckets": [
                {
                    "key": 9,
                    "doc_count": 2
                }, {
                    "key": 8,
                    "doc_count": 1
                }
            ]
        }
    }
}
输出：
```
#### 2.2.12. 查询某个字段里含有多个关键词的文档terms
```jsx{5}
{
    "query": {
        "bool": {
            "must": {
                "terms": {
                    "age": ["15", "13", "18"]
                }
            }
        }
    }
}
```
#### 2.2.13. 必须不：must_not
```jsx{4}
{
    "query": {
        "bool": {
            "must_not": { // 匹配所有age不等于15的文档
                "term": {
                    "age": "15"
                }
            }
        }
    }
}
```
### 2.3. 非全量更新：
POST 请求 http://127.0.0.1:9200/role/_update/1001
入参：
```jsx
{
    "doc": {
        "title": "111"
    }
}
```

## 3. 映射关系
PUT http://127.0.0.1:9200/role/_mapping
```jsx
{
    "properties": {
        "name": {
            "type": "text",
            "index": true
        },
        "sex": {
            "type": "keyword",
            "index": true
        },
        "tel": {
            "type": "keyword",
            "index": false
        },
    }
}
```

## 4. 常用方法:
### 4.1. fuzzy 模糊查询
### 4.2. refresh
默认为每秒执行一次。
当我们对数据进行操作，如增删改时，需要手动刷新，才能被搜索到
### 4.3. flush
### 4.4. delete_by_query
顾名思义，根据query对数据进行删除， 会删除所有query语句匹配上的文档
### 4.5. update_by_query
### 4.6. update:
### 4.7. index
### 4.8. bulk
批量操作
```jsx
result = []
es = Elasticsearch('地址')
for i in data: // 预设data是某list
    _id = i["_id"]
    result["age"] = "15" // 
    action = {"op_type": "update", "_index": "index_name", "_type": "_doc", "doc": result, "_id": _id} // 操作类型是update；索引名称；类型；更新的值；id
    update_actions.append(action)
helpers.bulk(client=es, actions=update_actions) // 批量操作
```
以下是不建议的方式：
```jsx
result = []
es = Elasticsearch('地址')
for i in data: // 预设data是某list
    _id = i["_id"]
    result["age"] = "15" 
```
### 4.9. count
### 4.10. indices.exists()

## 5. 其他
multi_match 可指定多个字段
match_phrase 短语匹配查询
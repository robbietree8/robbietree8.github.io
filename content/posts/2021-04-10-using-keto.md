---
title: using keto
date: 2021-04-10
draft: false
---

# Using Keto

## 解决什么问题

分布式的权限校验系统


## 上手

```
$ brew tap ory/keto
$ brew install ory/keto/keto
$ keto help
```

## 回顾

![class diagram](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/robbietree8/robbietree8.github.io/main/assets/2021-04-10/snippet-1.puml)


权限描述的是: `谁`对`资源`拥有什么`操作`

假设有这么一个权限描述

```
alice can create articles
anyone can view any articles
only owner can edit his articles
```

__青铜实现__

```
permission: {
    permissionId,
    permissionName,
    action,
    resource
}

subject_permission_rel: {
    subjectId,
    permissionId
}
```

```json
# permission
[{
    "permissionId": 1,
    "permissionName": "write",
    "action": "POST",
    "resource": "/articles/1.md"
},
{
    "permissionId": 2,
    "permissionName": "read",
    "action": "GET",
    "resource": "/articles/1.md"
},
{
    "permissionId": 3,
    "permissionName": "write",
    "action": "POST",
    "resource": "/articles"
},
{
    "permissionId": 4,
    "permissionName": "read",
    "action": "GET",
    "resource": "/articles"
}]

# subject_permission_rel
[{
    "subjectId": "alice",
    "permissionId": 1
},
{
    "subjectId": "alice",
    "permissionId": 3
},
{
    "subjectId": "*",
    "permissionId": 4
}]
```

Q1: is alice able to create articles

```
let pis = select permissionId from subject_permission_rel where subjectId =~ 'alice|*'

(permissions in pis).anyMatch(p -> p.action = 'POST' && p.resource =~ '/articles')
```

Q2: is alice able to edit article 1.md

```
let pis = select permissionId from subject_permission_rel where subjectId =~ 'alice|*'

(permissions in pis).anyMatch(p -> p.action = 'POST' && p.resource =~ '/articles/1.md')
```

Q3: is bob able to view articles

```
let pis = select permissionId from subject_permission_rel where subjectId =~ 'bob|*'

(permissions in pis).anyMatch(p -> p.action = 'GET' && p.resource =~ '/articles')
```

__王者实现__

权限声明

```json
{
  "namespace": "articles",
  "object": "/articles",
  "relation": "writer",
  "subject": "alice"
}
{
  "namespace": "articles",
  "object": "/articles/1.md",
  "relation": "owner",
  "subject": "alice"
}

{
  "namespace": "articles",
  "object": "/articles",
  "relation": "viewer",
  "subject": "bob"
}
```

```
export KETO_READ_REMOTE="127.0.0.1:4466"
export KETO_WRITE_REMOTE="127.0.0.1:4467"

keto relation-tuple create ./create_rt -f json-pretty
```

Q1: is alice able to create articles

```
keto check alice owner articles /articles/1.md
```

Q2: is alice able to edit article 1.md

```
keto check alice writer articles /articles
```

Q3: is bob able to view articles

```
keto check bob viewer articles /articles
```




## 基本概念解释

### Relation Tuples

Keto里的一条关系元组记录的格式为

```
object#relation@subject
```

解释为

```
Subject has relation on object
```

特别的有一条语法是

```
<subject> ::= subject_id | <subject_set>
<subject_set> ::= <object>'#'relation
```

利用这个特性，可以定义一些嵌套的规则，比如某篇文章的拥有者同时也是这篇文章的编辑者，这样的话，就不需要对每种权限各自定义一堆规则(HRBAC)。

```
articles#owner@jack
articles#editor@articles:owner
```

解释如下

```
jack是articles的owner，同时作为articles的owner同时也拥有了editor权限
```

### Objects & Subjects

keto 建议以 uuid 作为 Objects, Subjects 的载体，应用程序需要将应用内的数据映射到 uuid 上。



## 参考

- [官方文档](https://www.ory.sh/keto/docs/)
- [Zanzibar](https://www.usenix.org/conference/atc19/presentation/pang)
- [Zanzibar: Google’s Consistent, Global Authorization System](https://research.google/pubs/pub48190/)



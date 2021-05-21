---
title: plantuml guide
date: 2019-09-12
draft: false
tags: ["craftsman"]
---

# 文本绘图利器plantuml

## 应用场景

做系统设计免不了需要画图，从设计流程上来说，*用例分析* -> *组件交互* -> *领域对象分析*，往往不是一蹴而就的，需要一个反复迭代的过程，在这个过程中，如何更好的追溯你的设计变更很重要，不管后续维护的是你自己还是其他人，都可以从这个`设计变更`里得到重要的信息。

## 一个需求

这里以一个假想的需求为例，演示一下上述所述的画图场景

以顾客到餐馆用餐为例，需要经历顾客来餐厅吃饭，点菜，服务员下单，厨师出菜，服务员上菜，顾客结账的过程，下面我们来看怎么画这些图。


### 用例分析

```
left to right direction

actor Customer as C
actor Waiter as W
actor Chef as H

rectangle {
    C -- (点菜)
    C -- (结帐)
    (出菜) -- H
    (下单) -- W
    (上菜) -- W
}
```

![CaseAnalysis](https://www.plantuml.com/plantuml/svg/oqbDAr4eoLSeoapFA558oInAJIx9pC_ZuafCBialKd0kBIx9pqqjKaWiLd26YeKdPfP0HC9XgZ9Iqq1y3oukaFx4lFISL8LgBWKWS5RGrLNGUDwqyqN_74raaTsJd-wO017HUDg-2oGDal20Y3pPqVsqTofO91mcqWLJ4yvLomK0)


### 组件交互设计

```
participant Customer as C
participant Waiter as W
participant Chef as H

C -> W : 点菜
W -> H : 下单
H -> W : 出菜
W -> C : 上菜
C -> W : 结帐
```

![ComponentAnalysis](https://www.plantuml.com/plantuml/svg/AqWiAibCpYn8p2jHS2ujBidFJIrII2nMSEOgG989JymiWOY7euWxPwIcWKGzkBYS5NJj5C8Lh1IUD-ryqJ-7Anp4zm3od-peVjexbSi39l-qVHTStXaitmNY8_JldlnqnmGk0000)

### 领域对象分析与设计

```
class Customer {
    name: String
    Menu fillMenu()
    checkout(order: Order)
}

class Waiter {
    Order makeOrder(menu: Menu)
    void serve(food: Food)
}

class Chef {
    Food cook(order: Order)
}

class Menu {
    dish: String
    quantity: int
}

class Order {
    menu: Menu
    orderTime: Date
}

class Food
```

![DomainAnalysis](https://www.plantuml.com/plantuml/svg/TP313i8W38RlF4MFx1MupdWp7ZJnJA1pP0CYb2N6-EwEOfmUv02bxV_r1pFhdA4lcQB710y1wmhQeu8J9HUkd3XWA32uUQw1x3XdHZHJB2HZifWK7ElHYQSGXfaNxUX3v29uFI57qgySnTW6MwApa34jA8SOhOBzkd_1-X67DwfMmCGu_HlCPbklTNdyUSYjw42ExWfSe4tIx3NDPeslFEJiweViDE6cgJx42m00)


## 参考资料

- [语法](http://plantuml.com/zh/)
- 工具
    - [MWeb](https://zh.mweb.im)
    - [VScode](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml)
---
title: system design principles by example
date: 2021-04-02
draft: false
---

# 从todo-cli谈谈一些系统设计的原则

## 介绍

以实际的需求为切入点，谈谈在系统设计以及实现上哪些原则可以帮助我们更好的实现设计。

需求来自郑晔老师的[代码之丑](https://time.geekbang.org/column/article/325594), 简单讲，就是一个简易的命令行todo应用。


## 原则解读

### 1. 分层设计

__类设计图，具体代码可以下拉到参考链接__

![class diagram](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/robbietree8/robbietree8.github.io/main/assets/2021-04-02/classDiagram.puml)


这是目前版本的类设计图，可以看到`domain`包作为核心功能的承载者，不依赖外部的类，这样做的好处是，外围的一些需求变化，不会导致核心功能的变更。

但是从具体的实现上来看，还是有一点问题，比如`AddCommand`的实现。

```java
@Command(name = "add", description = "Add todo item.", mixinStandardHelpOptions = true)
public class AddCommand implements Runnable {
    @Parameters(description = "Item content.")
    String content = "";

    @Inject
    ItemRepository itemRepository;

    @Inject
    SessionRepository sessionRepository;

    @Override
    public void run() {
        String currentUser = sessionRepository.getCurrentUser();

        Long nextIndex = itemRepository.nextIndex(currentUser);

        Item item = Item.create(nextIndex, currentUser, content);
        itemRepository.save(item);

        System.out.printf("\n%d. %s\n", nextIndex, content);
        System.out.printf("\nItem %d added\n", nextIndex);
    }
}
```

实现上，在`command`里直接处理了业务逻辑，如果这时候来了一个需求，需要提供rest api供网页端使用，怎么办？显然目前的这种实现是需要加以改造的。

为此，我引入了`domain service`，将`command`里处理的业务逻辑挪到此。

```java
@Singleton
public class AddService {
    @Inject
    ItemRepository itemRepository;

    public Long add(final String username, final String content) {
        Long nextIndex = itemRepository.nextIndex(username);

        Item item = Item.create(nextIndex, username, content);
        itemRepository.save(item);

        return nextIndex;
    }
}

@Command(name = "add", description = "Add todo item.", mixinStandardHelpOptions = true)
public class AddCommand implements Runnable {
    @Parameters(description = "Item content.")
    String content = "";

    @Inject
    SessionRepository sessionRepository;

    @Inject
    AddService addService;

    @Override
    public void run() {
        String currentUser = sessionRepository.getCurrentUser();

        addService.add(currentUser, content);

        Long nextIndex = addService.add(currentUser, content);

        System.out.printf("\n%d. %s\n", nextIndex, content);
        System.out.printf("\nItem %d added\n", nextIndex);
    }
}
```

如此一来，如果要支持rest api的接口，只需要引入controller包，调用下service即可，不需要改动核心业务逻辑处理的代码。

详见[pull!3](https://github.com/robbietree8/todo-cli/pull/3)

⚡ **每一行代码要找到属于它的位置**


### 2. 依赖反转

![ioc-1](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/robbietree8/robbietree8.github.io/main/assets/2021-04-02/ioc-1.puml)


上图的箭头表示的是依赖关系，也就是`AddService`依赖`FileItemRepository`，这里存在的问题是，`FileItemRepository`实际上是对应文件存储的实现方式而已，如果后续换了存储方式，比如换成了数据库，那么就需要修改`AddService`，也就是需要修改核心的处理业务逻辑的代码。


![ioc-1](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/robbietree8/robbietree8.github.io/main/assets/2021-04-02/ioc-2.puml)


为此，引入了`ItemRepository`接口，`AddService`依赖这个接口，包含具体实现的`FileItemRepository`也依赖于这个接口，从依赖方向上看，对比前图，方向是反转了。

这么做的好处是，假设后续要支持数据库存储，只需要实现`ItemRepository`接口，在编译时或运行时决定`ItemRepository`的具体实现是哪个类即可，核心业务处理逻辑不需要改动。

⚡ **高层策略不应该依赖低层细节**

## 参考

- [实现todo cli的代码仓库](https://github.com/robbietree8/todo-cli)
- [dipInTheWild](https://martinfowler.com/articles/dipInTheWild.html)


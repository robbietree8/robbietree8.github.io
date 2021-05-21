---
title: using spock by example
date: 2021-04-04
draft: false
tags: ["craftsman"]
---

# Using Spock

## 引言

Java 程序员最熟悉的单元测试框架莫过于 Junit，大多数人用的应该是4.x的版本，最新的5.x的版本相比4.x的版本增加了不少新的特性，写起单元测试来，也更加的得心应手，不过这篇文章不是来介绍 Junit 的，而是简单引荐下 Spock，号称是 jvm 下最好用的测试框架。


## 三段式

一般在写单元测试的时候，大致会有这样的结构

```java
class Adder {
    public int add(int a, int b) {
        return a + b;
    }
}

class AdderTest {
    @Test
    public void test_add() {
        //set up
        Adder adder = new Adder();
        
        //do something
        int rtn = adder.add(1, 2);
        
        //assert
        assertEquals(3, rtn);
    }
}
```

1. 构建测试对象
2. 调用被测试的方法
3. 断言测试结果

Spock 强化了该测试结构，将上述的三段逻辑，分成了三个 block, 即`given`, `when`和`then`

```groovy
class AdderSpec extends Specification {
    def "should add two numbers"() {
        given:
            Adder adder = new Adder()
        when:
            int rtn = adder.add(1, 2)
        then:
            3 == rtn
    }
}
```


## 断言

为上篇[博客](https://robbietree8.github.io/posts/2021-04-02-system-design-principles/)的代码加了几个单元测试，看看断言是怎么写的，相关的 pr 可以访问[todo-cli-pr!4](https://github.com/robbietree8/todo-cli/pull/4)

```groovy
class AddServiceSpec extends Specification {
    def itemRepository = Mock(ItemRepository)

    def "should add item to repository"() {
        given:
            AddService addService = new AddService(itemRepository)
        when:
            addService.add("username", "content")
        then:
            1 * itemRepository.nextIndex("username")
            1 * itemRepository.save(_)
    }
}
```

在 then 段里，可以感受下 groovy 语法的简洁性。


## mock

与 junit 需要使用第三方的 mock 框架不同，spock 内置了 mock 机制。

```groovy
class AddCommandSpec extends Specification {
    SessionRepository sessionRepository = Mock(SessionRepository)

    def "should add item successfully"() {
        given:
            sessionRepository.getCurrentUser() >> "mockUser"
            AddCommand addCommand = new AddCommand(sessionRepository, new AddService(new FileItemRepository()))
            CommandLine command = new CommandLine(addCommand)
            StringWriter sw = new StringWriter()
            command.setOut(new PrintWriter(sw))
        when:
            int exitCode = command.execute("just do it")
        then:
            exitCode == 0
            sw.toString().contains("just do it")
    }
}
```


## 参考

- [Interaction Based Testing](https://spockframework.org/spock/docs/1.3/interaction_based_testing.html)
- [JUnit5 使用者：为何 Spock 值得你看它一眼](https://blog.dteam.top/posts/2020-04/spock-is-better.html)
- [picocli-testing](https://picocli.info/#_testing_your_application)

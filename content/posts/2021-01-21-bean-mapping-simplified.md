---
title: bean mapping simplified
date: 2021-01-21
draft: false
tags: ["craftsman"]
---

# Bean mapping simplified

## 背景

对于分层应用程序来说，`Java Bean`之间的转换非常常见，比如从`DTO -> BO`，从 `BO -> DO`，那么如何选择一个既好用性能又好的转换工具呢？

在开源社区里，做这个方面事情的工具非常多，这里挑选了三个相对较活跃的工具，做一下对比。

## 横向比较

|   |  star数 | compile or runtime  | 是否使用反射 | 注解 or XML | inception year |
|---|---|---|---|---|---|
| mapstruct  | 2.3k  | compile  | N | 注解 | 2012 |
| model mapper  | 1.4k  | runtime  | Y | N/A | 2011 |
| dozer  | 1.6k  | runtime  | Y | XML & 注解 | 2005 |


## 性能对比

这里测试了简单对象(simpleTest)和复杂对象(complexTest)，从吞吐率和平均时间两个维度来考量。
无论是哪个维度，`mapstruct`都横扫另外两个工具。

__吞吐量__
```
Benchmark                                                                  Mode     Cnt       Score    Error   Units
BeanMappingPerfTest.complexTest                                           thrpt           38368.001           ops/ms
BeanMappingPerfTest.complexTest:dozerMapperComplexBenchmark               thrpt             102.886           ops/ms
BeanMappingPerfTest.complexTest:mapstructMapperComplexBenchmark           thrpt           38172.549           ops/ms
BeanMappingPerfTest.complexTest:modelMapperComplexBenchmark               thrpt              92.567           ops/ms
BeanMappingPerfTest.simpleTest                                            thrpt          148404.768           ops/ms
BeanMappingPerfTest.simpleTest:dozerMapperSimpleBenchmark                 thrpt             564.281           ops/ms
BeanMappingPerfTest.simpleTest:mapstructMapperSimpleBenchmark             thrpt          146767.287           ops/ms
BeanMappingPerfTest.simpleTest:modelMapperSimpleBenchmark                 thrpt            1073.199           ops/ms
```

__平均运行时间__
```
Benchmark                                                                  Mode     Cnt       Score    Error   Units
BeanMappingPerfTest.complexTest                                            avgt               0.008            ms/op
BeanMappingPerfTest.complexTest:dozerMapperComplexBenchmark                avgt               0.012            ms/op
BeanMappingPerfTest.complexTest:mapstructMapperComplexBenchmark            avgt              ≈ 10⁻⁵            ms/op
BeanMappingPerfTest.complexTest:modelMapperComplexBenchmark                avgt               0.014            ms/op
BeanMappingPerfTest.simpleTest                                             avgt               0.001            ms/op
BeanMappingPerfTest.simpleTest:dozerMapperSimpleBenchmark                  avgt               0.002            ms/op
BeanMappingPerfTest.simpleTest:mapstructMapperSimpleBenchmark              avgt              ≈ 10⁻⁵            ms/op
BeanMappingPerfTest.simpleTest:modelMapperSimpleBenchmark                  avgt               0.001            ms/op
```



## 优势对比

### 个性化映射

有些时候，我们希望对目标对象能做一些个性化的映射，而不是直接映射。
毕竟我们定义了那么多相似的对象，肯定是想有点额外的收益嘛 : )

> With mapstruct

__定义原始类__
```java
public class PersonalizedSourceObject {
    private Address address;

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }
}
```

__定义目标类__
```java
public class PersonalizedDestinationObject {
    private String fullAddress;

    public String getFullAddress() {
        return fullAddress;
    }

    public void setFullAddress(String fullAddress) {
        this.fullAddress = fullAddress;
    }

    @Override
    public String toString() {
        return "PersonalizedDestinationObject{" +
                "fullAddress='" + fullAddress + '\'' +
                '}';
    }
}
```

__定义转换接口__
```java
@Mapper
public interface MapstructModelConverter extends ModelConverter {
    MapstructModelConverter CONVERTER = Mappers.getMapper(MapstructModelConverter.class);

    @Override
    @Mappings({
            @Mapping(target = "fullAddress", expression = "java(personalizedSourceObject.getAddress().getProvince() "+ " + \"-\" " +
                    "+ personalizedSourceObject.getAddress().getCity())")
    })
    PersonalizedDestinationObject convert(PersonalizedSourceObject personalizedSourceObject);
}
```

### 与Spring集成

我们的应用框架基本上是以`Spring`为主，如果能与`Spring`集成，无疑带来更便捷的接入方式，而`mapstruct`就提供了这个能力。

接入方式也很简单，如下所示

```java
@Mapper(componentModel = "spring")
public interface SimpleMapper {}
```

dozer也可以直接与spring集成，不过集成的方式没有那么简明，这里就不贴了。

而对于modelmapper来说，只需要定义一个`ModelMapper`的bean即可。

## 总结

虽然这里列举了Mapstruct的各种好处，并且也推荐大家用这个工具，但是萝卜青菜各有所爱，各位开心就行。


## 参考资料

* [modelmapper](https://github.com/modelmapper/modelmapper)
* [mapstruct](https://github.com/mapstruct/mapstruct)
* [dozer](https://github.com/DozerMapper/dozer)
* [java-microbenchmark-harness](https://www.baeldung.com/java-microbenchmark-harness)
* [java-performance-mapping-frameworks](https://www.baeldung.com/java-performance-mapping-frameworks)

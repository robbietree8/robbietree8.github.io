# java-dns-cache-manipulator源码阅读笔记

阅读开源代码[java-dns-cache-manipulator](https://github.com/alibaba/java-dns-cache-manipulator)的一些收益



__脑图__

![](https://i.imgur.com/rQSjgL5.jpg)



以下源码来自[InetAddressCacheUtil](https://github.com/alibaba/java-dns-cache-manipulator/blob/master/library/src/main/java/com/alibaba/dcm/internal/InetAddressCacheUtil.java)

__典型的双检锁实现/反射的应用__

```java
static volatile Object[] ADDRESS_CACHE_AND_NEGATIVE_CACHE = null;

/**
 * @return {@link InetAddress#addressCache} and {@link InetAddress#negativeCache}
 */
static Object[] getAddressCacheFieldsOfInetAddress0()
        throws NoSuchFieldException, IllegalAccessException {
    if (ADDRESS_CACHE_AND_NEGATIVE_CACHE == null) {
        synchronized (InetAddressCacheUtil.class) {
            if (ADDRESS_CACHE_AND_NEGATIVE_CACHE == null) {  // double check
                final Field cacheField = InetAddress.class.getDeclaredField("addressCache");
                cacheField.setAccessible(true);

                final Field negativeCacheField = InetAddress.class.getDeclaredField("negativeCache");
                negativeCacheField.setAccessible(true);

                ADDRESS_CACHE_AND_NEGATIVE_CACHE = new Object[]{
                        cacheField.get(InetAddress.class),
                        negativeCacheField.get(InetAddress.class)
                };
            }
        }
    }
    return ADDRESS_CACHE_AND_NEGATIVE_CACHE;
}
```

> 参考资料

[Java内存模型中的happen-before是什么？](https://time.geekbang.org/column/article/10772)

__同步块__

```java
public static void setInetAddressCache(String host, String[] ips, long expiration)
        throws NoSuchMethodException, UnknownHostException,
        IllegalAccessException, InstantiationException, InvocationTargetException,
        ClassNotFoundException, NoSuchFieldException {
    host = host.toLowerCase();
    Object entry = newCacheEntry(host, ips, expiration);

    synchronized (getAddressCacheFieldOfInetAddress()) {
        getCacheFiledOfAddressCacheFiledOfInetAddress().put(host, entry);
        getCacheFiledOfNegativeCacheFiledOfInetAddress().remove(host);
    }
}
```

> 参考资料

[synchronized底层如何实现？什么是锁的升级、降级？](https://time.geekbang.org/column/article/9042)


以下源码来自[DnsCacheManipulator](https://github.com/alibaba/java-dns-cache-manipulator/blob/master/library/src/main/java/com/alibaba/dcm/internal/DnsCacheManipulator.java)

__异常转换__

```java
public static void setDnsCache(String host, String... ips) {
    try {
        InetAddressCacheUtil.setInetAddressCache(host, ips, NEVER_EXPIRATION);
    } catch (Exception e) {
        final String message = String.format("Fail to setDnsCache for host %s ip %s, cause: %s",
                host, Arrays.toString(ips), e.toString());
        throw new DnsCacheManipulatorException(message, e);
    }
}
```



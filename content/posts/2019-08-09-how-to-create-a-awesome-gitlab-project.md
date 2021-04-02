# 如何创建一个高质量高逼格的Gitlab工程项目

## 目录
1. [高质量](#高质量)
2. [高逼格](#高逼格)
3. [对自己好一点](#对自己好一点)
4. [参考](#参考)

## 高质量

首先我们来定义一下什么是**高质量**，不同于美的定义（一千个读者可能有一千个哈姆雷特），对于高质量，我们可以达成一个共识。

1. 有效的单元测试
    - 相信对于单元测试的作用，大家都不会陌生，对于能提高效率的事情，何乐而不为呢？
    - 对于Java项目来说，*junit*是公认的测试框架，熟练使用就可以了，另外，你一定会用到*mockito*这个工具，具体见文末的参考链接
2. 有可观的测试覆盖率
    - 光光有单元测试是不够的，只有达到一定阈值的覆盖率才是合格的，不必追求百分百的覆盖率，按照2/8原则，80%是一个可以*追求*的阈值
    - 这里推荐下*jacoco*这个测试覆盖率工具，和*Intellij IDEA*也有很好的集成
3. 集成sonar
    - 通过sonar扫描代码库，来*帮助*我们找出容易忽略的问题，对于你认为sonar检出的不是一个问题，大可以忽略，不用太纠结
4. CI/CD
    - 开发完成之后，还需要手动跑测试用例，手动发布到开发环境？
    ![](https://i.loli.net/2019/08/10/ldesfpaEoPk1g7t.jpg)
    - 正确的姿势见下图


![](https://www.plantuml.com/plantuml/svg/YzQALT2rKt3EJqbroSzBvO8n57HJyilpW3AXUPab8Qd59LOAB_RFVBAZuTdQfK_dxtesPysJ7LrFvwnukgVXQV_4PtDsWRWRDZxjN_zazzAdwtgUTKpWWca5MH1GMfmHak-UMPAJc6IbyBHtwjFMvcTRkr-id_goOTQB_UrF9_IztzDJ05JrPFVYvmiQdtPiUB9xyjDTaxcGTS9TvPbNaffUb5YIQgK0KV-iVyAJNKk0kjB12Y76lMXVzRHhnOlj4r_ERWCw268V5rmlt3INr8AS_4eh1MHboCw2cFEoUSNplPk0La0KFEzR_tHHCnS0)

## 高逼格

然后我们来定义一下什么是**高逼格**

### 一定要有图

![](http://files.blogcn.com/wp06/M00/03/F8/wKgKDVGKXNIAAAAAAAUg2nA4CdE500.jpg)

正所谓一图胜千言，相对于文字，图片能传递更多的信息，也更容易让看官理解。
对于我们的项目来说，需要什么样的图呢？

> 流程图

比如


![](https://www.plantuml.com/plantuml/svg/NP3DIiD058Ntzodc1Ve2Bdm3Low3YqadgI7DZqbBTAVGeek41cfK2WR4dxfA5Q58Q_1bCZFDobTmaac2k1dkFNE-UowNu15MiQ-XWxF3ao72Fh90CR5kOjHtR5lhZnplg99DqJO_iWU52FQVomN5iFNX-9IeTM-0Cl2mZcq93Heem9vsx8nzhLP908gboJrgmL81BDJRbw7LWth42UnA0REhNMbomUqa0-sXyZwbMSfkrxlb8qjZTXNpxPob4nhUiEIDHTdFLqZpX4wVsrN1w0OCn81cu_8oBASHpxFM9ccsYUd5Whud_6dp9va4WgIprUuXv8i9dRqKd-Ty6Gah-qZOtKh28bGKnNOEmj-YbYhG8l_crw_j6IfC0ZBd5m00)


> 类图

比如


![](https://www.plantuml.com/plantuml/svg/ZLJBRfn04Bpp5NCaKlOBREt5aQKajcpPwmzec0QFCmyqtQnjHVdtD0C6Xjt3Sc5KNTKr_O3Q0f6w2PvwXr7zBVuLXV6CiO4QrNFzYWuU8LAAANb6I7K3LZvpDBvLHx0zVhiQj7NAjzOzOMk8u-UaFmQZLKmOnZ8pI3cZv5byfaYBF9vAkPke6zj_73wxpFCcE1Ty9ZEki-ZGCsqhcLMt5fZHexvSkBH7skQnvX3NjNKOIOgRIbEKNkkB_lJ3zNMrg5TswvIpuZ46X_oAFLsk-GtD7xYSVe3AelZKIBIfPmJBxNnrRr7_2bKsrzuBab6VuFq7CDmYY-OhyPqqw3fPxbKpH2R9qjg7fY7aU_3GpYeRPDTaNaWyXijfLv8tmH4NUF7NLPvhrOcjoMP_lpP-N78jLLH0UBK67GBrCQKRyNlehiVOzucpLmmjdKOVgZUPWjHlqEefVZTKoCPD9WSvlNZ3CWEi3PdWKs6RMUtwRhS_-ycTC2qslE6US7OwlFkTiEOzAURVWw0vlSWsOPJkyxwUf-HhSPwpiVeF)

> 上下游依赖图

比如


![](https://www.plantuml.com/plantuml/svg/VSin2e0m343HlQS8EFS2KWgjgvUeDCQ2f9B6-wiK5mVNp_jSCq80vzeXXSakjQhtCCo5DetxYOOV4R-Yv-blD2Q0zRSPGILnMr4W9qqznpiBKccAOk8B2RMR2m00)

画图的工具，每个人都有各自的喜好，在这里，推荐使用*plantuml*，原因是它可以使用文本的形式来描述，好处是通过源码版本工具比如*git*等，可以追溯每次的变更，进而了解设计的变化


### 徽章

在"面基网站"**Github**上我们经常能看到一些很酷的徽章，比如
![-w459](https://i.loli.net/2019/08/09/vDlzS2s4XrbqiU9.jpg)

在Gitlab里能不能也做到呢？答案是可以的
比如我们要显示测试覆盖率，代码质量等，就可以这样做
![-w396](https://i.loli.net/2019/08/09/qYjIzKiOdQ2Nca9.jpg)

只要在markdown里插入以下文字就可了，是不是很酷？

```
[![Coverage](http://{sonar_host}/api/project_badges/measure?project={projectId}&metric=coverage)](http://{sonar_host}/component_measures?id={projectId}&metric=Coverage) [![code quality](http://{sonar_host}/api/project_badges/measure?project={projectId}&metric=alert_status)](http://{sonar_host}/dashboard?id={projectId}) [![Reliability Rating](http://{sonar_host}/api/project_badges/measure?project={projectId}&metric=reliability_rating)](http://{sonar_host}/dashboard?id={projectId})
```
*注意替换里面的变量*

![](https://i.loli.net/2019/08/10/7jUJ8PgkQdSvYl1.jpg)

## 对自己好一点

最后我们要做到对**开发者友好**，记住这个开发者可能也是你哦。

首先我们得有README，读`read me`，一个看官打开你的项目，首先看到的就是项目的README，在这个文件里，你可以用markdown语言来描述。

在README里，可以包含以下内容

1. 标题，简要描述这个项目是干嘛的
2. 副标题，可以加点详细的描述
3. 如果README里的内容很多的话，加个目录
4. 描述你的设计方案，记得多用图的形式
5. 描述你的项目包含哪些功能
6. 最后给那些跃跃一试的同学一次快速上手的机会，比如提供一个可以直接运行的docker包，或者提供docker-compose.yml文件
7. 描述下如何对这个项目做贡献(PR)

具体范例可以参考*influxdb*等

## 参考

- [mockito](https://site.mockito.org/)
- [jacoco](https://www.eclemma.org/jacoco/)
- [plantuml](http://plantuml.com/zh/)
- [influxdb](https://github.com/influxdata/influxdb)
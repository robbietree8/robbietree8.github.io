---
title: hubot with rocketchat
date: 2018-02-09
draft: false
tags: ["playground"]
---

# hubot with rocketchat


## 缘起

无意间看到[这篇文章](https://medium.com/@wanquribao/%E6%B9%BE%E5%8C%BA%E6%97%A5%E6%8A%A5%E6%98%AF%E5%A6%82%E4%BD%95%E8%BF%90%E4%BD%9C%E7%9A%84-5f2482e21be2)，里面提到文章的作者使用了`slack`和`hubot`自动化了一些事情，恰巧我在公司的gitlab里，发现了一位同事使用了Rocket.Chat(可以认为是slack的开源版)，好奇心驱使下，就倒腾了些这些玩意 -> hubot & rocket.chat，just for fun.

## 可以干吗

rest api & shell, 你可以在web界面（也提供了手机app）上输入一些指令，hubot会根据这些指令做相应的事情，看以下的gif:

__rest api__

我写了一个js脚本，这个脚本可以根据输入的城市去一个rest api提供方查询这个城市的天气预报

![weathe](https://i.loli.net/2018/02/09/5a7d98279f248.gif)

__shell command__

下载了李鼎的bash脚本，通过web端来调起执行

![shel](https://i.loli.net/2018/02/09/5a7d982c6a12c.gif)


## 怎么弄？

### hubot

参考: https://hubot.github.com/docs/

```
npm install -g yo generator-hubot
mkdir webot & cd webot
yo hubot --adapter="rocketchat@1"
```

### rocket.chat server


参考 https://rocket.chat/docs/installation/docker-containers/docker-compose/

1. wget https://raw.githubusercontent.com/RocketChat/Rocket.Chat/develop/docker-compose.yml
2. update port accordingly
3. docker-compose up -d mongo
4. docker-compose up -d mongo-init-replica
5. docker-compose up -d rocketchat
6. docker-compose up -d hubot


### 两者集成

参考 https://github.com/RocketChat/hubot-rocketchat

```
npm install hubot-rocketchat@1
export ROCKETCHAT_ROOM=''
export LISTEN_ON_ALL_PUBLIC=true
export ROCKETCHAT_USER=bot
export ROCKETCHAT_PASSWORD=123456
export ROCKETCHAT_AUTH=ldap
export ROCKETCHAT_URL=localhost:4000
start with `bin/hubot -a rocketchat`
```


## 社区贡献的脚本

1. update npm
2. npm search hubot-scripts <whatever you want>



## 如何写自己的脚本


在hubot的scripts目录里新增脚本，比如我们想在`Rocket.Chat`里查询天气，我们可以这么做

`webot weather 杭州`

效果如下

![](https://i.loli.net/2018/02/09/5a7d97ff031b1.jpg)

那么这个脚本怎么写呢?

```js
const axios = require('axios')

const baseURL = 'http://www.sojson.com/open/api/weather'
const weatherapi = axios.create({
  baseURL,
  headers: {
    'Accept-Encoding': 'application/json',
    'Content-Type': 'application/json'
  }
})

module.exports = robot => {

  robot.respond(/weather (.+)/i, res => {
    const query = res.match[1]
    weatherapi.get(`/json.shtml?city=`+encodeURIComponent(`${query}`))
      .then(resp => {
        const payload = resp.data.data.forecast.
          map(x => `${x.date} - ${x.high} - ${x.low}`)
            .join('\n')

        res.send(`${payload}`)
      })
      .catch(err => res.send(`天气查询失败: ${query}`))
  })

}
```

仿造这个例子，可以写任何提供rest api接口的脚本





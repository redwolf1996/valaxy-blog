---
title: '随心瑜-燃脂训训练营和2021春节活动页总结'
date: 2021-02-03 10:53:02
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
> **以下是来新公司最新做的两个微信H5活动页面前端部分一些技术点的总结**
- **微信公众号授权思路**
  1.创建一个auth授权页面，并创建一个路由指向它，在这个页面发起一个腾讯提供的get请求，带上redirect_url参数，此url指向该页面本身，发起请求后腾讯回调该链接的时候会返回一个code的参数（单页应用，返回时获取code需做特殊处理才能从返回的链接中解析出code，正常的获取参数可以不必自己写方法获取链接参数直接用框架路由自带的方法，比如vue的this.$router.query.code，如下所示）
```
export function getQueryArg(attr, splitChar) {
  var query = window.location.search,
    splitChar = splitChar || "&",
    attrArr = [],
    attrObj = {};

  query = query.replace(/^\?/, "");
  attrArr = query.split(splitChar);
  for (var i = attrArr.length - 1; i >= 0; i--) {
    attrObj[attrArr[i].split("=")[0]] = attrArr[i].split("=")[1];
  }

  return attrObj[attr] || "";
}
```
  2.获取code之后接着就拿着这个code去请求后端接口获取token(这一步是真正的登录以获取用户讯息并将它保存在本地以保持状态)
```
    this.$request
        .post("/yogastar/api/wxmpcloginbycode", {
          code: getQueryArg("code")
        })
        .then(re => {
          localStorage.setItem("access_token", re.data.access_token);
          localStorage.setItem("subscribe", re.data.subscribe);
          if (localStorage.getItem("beforeUrl"))
            location.href = localStorage.getItem("beforeUrl");
        });
```
3. 本地如何调试公众号授权避免无法授权的烦恼？
1）首先是要在微信开发者工具种操作
2）方法一是去测试环境登录，然后在本地环境种植一个一模一样的token，此方法的问题是每次token变了都要手动去操作
3）方法二是做内网穿透、本地绑定host到一个真实的域名，并且本地要搭建服务器的环境，才能完全模拟测试环境，缺点是复杂繁琐，还要重新申请一个域名防止跟测试、线上环境有冲突
4）方法三是有个小技巧，建立一个auth页面2，本地调试的时候先直接访问这个测试环境的页面，获取code后再跳转的本地的localhost页面，优点是省去了手动操作的麻烦，也不会出现git老是提交无用代码到gitlab上，避免了一些不必要的错误且不会影响测试和线上环境，缺点是多了一次跳转，速度优点慢
```
// 中介的auth页面
  created() {
    if (location.href.indexOf('code=') != -1) {
      let code = getQueryArg('code')
      location.href = `http://localhost:7002/#/oauth?code=${code}`
    } else {
      getAuthLink({ redirect_url: location.href }).then((re) => {
        location.replace(re.data.url)
      })
    }
  },
```
```
// 实际的auth页面
  created() {
    if (location.href.indexOf('code=') != -1) {
      let code = getQueryArg('code') || this.$route.query.code
      getTokenByCode({ code }).then((re) => {
        localStorage.setItem('access_token', re.data.access_token)
        localStorage.setItem('subscribe', re.data.subscribe)
        localStorage.setItem('user_info', JSON.stringify(re.data))
        if (!!localStorage.getItem('beforeUrl')) {
          location.replace(localStorage.getItem('beforeUrl'))
        } else {
          this.$router.push({ path: `/` })
        }
      })
    } else {
      getAuthLink({ redirect_url: location.href }).then((re) => {
        location.replace(re.data.url)
      })
    }
  },
```

- **img元素底部留白的问题**
  原因是img元素是行内元素，设置它的vertical-align: middle/top之后，底部的空白会消失

- **环境变量问题**
  不必手动更改环境变量，在package.json中设置好命令，利用node的process.env结合vue-cli实现在不用环境下应用不用的环境变量

- **图片压缩问题**
在线的可以使用tinypng，mac本地可以使用图压批量进行处理

- **垂直方向滚动的气泡**
 本质上就是横向的swiper变成纵向，但是swiper默认的是块级元素是一行不能在里面加两行数据，需要每两行去滚动的话可以用行内元素加上transform变相解决(目前能想到的方法)

- **春节活动首页的弹幕**
  使用vue-baberage，然后setTimeout每隔几十秒去请求弹幕，但是要弹幕长时间的不卡需要及时的清除定时器和老弹幕
```
    getBarrageList() {
      this.$request
        .post('/yogastar/api/wxmpcbarrage', {
          scene: 'newyear',
          number: 20,
        })
        .then((re) => {
          const that = this
          for (let i = 0; i < re.data.length; i++) {
            let t = setTimeout(function() {
              that.addToBarrageList(
                re.data[i].id,
                re.data[i].headimgurl,
                re.data[i].info
              )
            }, 1000 * i + Math.floor(Math.random() * 10000 + 1))
            this.timers.push(t)
          }
        })
    }
```
- animate.css的源代码可以从中直接摘取来用，有的时候没有必要完整引入

- **翻卡片的组件代码如下**
```
<template>
  <div v-bind:class="flipped ? 'flip-container flipped' : 'flip-container'">
    <div class="flipper">
      <div class="front">
        <slot name="front"></slot>
      </div>
      <div class="back">
        <slot name="back"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FlipCard',
  data() {
    return {
      flipped: false,
    }
  },
  methods: {
    reset() {
      this.flipped = false
    },
    reverse() {
      this.flipped = true
    },
  },
}
</script>

<style type="text/css" scoped>
i.frontFlipBtn,
i.backFlipBtn {
  position: absolute;
  right: 20px;
  top: 20px;
  color: #ffffff;
}
i.backFlipBtn {
  -webkit-transform: rotateY(-180deg);
  -moz-transform: rotateY(-180deg);
  -o-transform: rotateY(-180deg);
  -ms-transform: rotateY(-180deg);
  transform: rotateY(-180deg);
}
.flip-container {
  -webkit-perspective: 1000;
  -moz-perspective: 1000;
  -o-perspective: 1000;
  perspective: 1000;
}
.flip-container {
  min-height: 120px;
}
.flipper {
  -moz-transform: perspective(1000px);
  -moz-transform-style: preserve-3d;
  position: relative;
}
.front,
.back {
  -webkit-backface-visibility: hidden;
  -moz-backface-visibility: hidden;
  -o-backface-visibility: hidden;
  backface-visibility: hidden;
  -webkit-transition: 0.6s;
  -webkit-transform-style: preserve-3d;
  -moz-transition: 0.6s;
  -moz-transform-style: preserve-3d;
  -o-transition: 0.6s;
  -o-transform-style: preserve-3d;
  -ms-transition: 0.6s;
  -ms-transform-style: preserve-3d;
  transition: 0.6s;
  transform-style: preserve-3d;
  top: 0;
  left: 0;
  width: 100%;
}
.back {
  -webkit-transform: rotateY(-180deg);
  -moz-transform: rotateY(-180deg);
  -o-transform: rotateY(-180deg);
  -ms-transform: rotateY(-180deg);
  transform: rotateY(-180deg);
  position: absolute;
}
.flip-container.flipped .back {
  -webkit-transform: rotateY(0deg);
  -moz-transform: rotateY(0deg);
  -o-transform: rotateY(0deg);
  -ms-transform: rotateY(0deg);
  transform: rotateY(0deg);
}
.flip-container.flipped .front {
  -webkit-transform: rotateY(180deg);
  -moz-transform: rotateY(180deg);
  -o-transform: rotateY(180deg);
  -ms-transform: rotateY(180deg);
  transform: rotateY(180deg);
}
.front {
  z-index: 2;
}
</style>

```

- **复杂的、花里胡俏的但是又没有交互的动画**
可以用AE制作，然后导出json再去[github下载lottie-web](https://github.com/airbnb/lottie-web)这个库去解析，如果不会制作动画，也可以去这个网站搜索[做好的json](https://lottiefiles.com/)或者在线制作下载




<!-- more -->

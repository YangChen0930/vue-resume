<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor
      ref="resumeEditor"
      :markdown="currentMarkdown"
      :enableHtml="enableHtml"
    ></ResumeEditor>
  </div>
</template>

<script>
import StyleEditor from './components/StyleEditor'
import ResumeEditor from './components/ResumeEditor'
import './assets/reset.css'

export default {
  name: 'app',
  components: {
    StyleEditor,
    ResumeEditor
  },
  data() {
    return {
      interval: 40,
      currentStyle: '',
      enableHtml: false,
      fullStyle: [
        `/*
  * Inspired by http://strml.net/
  * 各位面试官，大家好，我是杨晨
  * 下面，就由我就来做个自我介绍！
  */

  /* 首先给所有元素加上过渡效果 */
  * {
    -webkit-transition: all .3s;
    transition: all .3s;
  }
  /* 白色背景太单调了，更换一下背景颜色 */
  html {
    color: rgb(222,222,222);
    background: rgb(0,43,54);
  }
  /* 文字离边框太近了 */
  .styleEditor {
    padding: .5em;
    border: 1px solid;
    margin: .5em;
    overflow: auto;
    width: 45vw;
    height: 95vh;
  }
  /* 代码高亮 */
  .token.selector{ color: rgb(133,153,0); }
  .token.property{ color: rgb(187,137,0); }
  .token.punctuation{ color: yellow; }
  .token.function{ color: rgb(42,161,152); }

  /* 加点 3D 效果呗 */
  html{
    -webkit-perspective: 1000px;
            perspective: 1000px;
  }
  .styleEditor {
    position: fixed; left: 0; top: 0;
    -webkit-transition: none;
    transition: none;
  }

  /* 接下来准备好一个编辑器 */
  .resumeEditor{
    position: fixed; right: 0; top: 0;
    padding: .5em;  margin: .5em;
    width: 48vw;
    height: 95vh;
    border: 1px solid;
    background: white; color: #222;
    overflow: auto;
  }
  /* 好了，我开始写简历了 */
          `,
        `
  /* 这个简历好像差点什么
  * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
  * 简单，用开源工具翻译成 HTML 就行了
  */
  `,
        `
  /* 再对 HTML 加点样式 */
  .resumeEditor{
    padding: 2em;
  }
  .resumeEditor h2{
    display: inline-block;
    border-bottom: 1px solid;
    margin: 1em 0 .5em;
  }
  .resumeEditor ul,.resumeEditor ol{
    list-style: none;
  }
  .resumeEditor ul> li::before{
    content: '•';
    margin-right: .5em;
  }
  .resumeEditor ol {
    counter-reset: section;
  }
  .resumeEditor ol li::before {
    counter-increment: section;
    content: counters(section, ".") " ";
    margin-right: .5em;
    float: left;
  }
  .resumeEditor blockquote {
    margin: 1em;
    padding: .5em;
    background: #ddd;
  }
`
      ],
      currentMarkdown: '',
      fullMarkdown: `# 杨晨

>   一个有洁癖的喜欢技术的前端小学生
>

## 个人优势

  * 开发过项目类型：PC端 + 移动端、混合app

  * 使用过前端技术 ： HTML5、CSS3、Flex、原生JS、jquery、Vue、小程序、NodeJS

  * 使用过的技术栈：Vue + Vuex +Vue-Router +Axios +Sass +ES6 +Webpack + Babel、

    Vue3+Vite+Pinia+Vue Router+Axios+ES6

  * 前端框架(Vue/React)，实现前端单页面应用状态管理及路由机制

  * 实现前后端分离开发

  * 前端工程化开发，深入理解Git/SVN版本管理工具


## 工作经历

### 2022.02 —— 至今

#### 扬州极链科技技术有限公司（前端负责人）

工作职责：

1. 负责项目前端团队建设
2. 负责前端开发规范制定
3. 负责前端团队技术选型
4. 负责前端项目架构和开发
5. 负责前端工作进度跟踪和技术攻关

### 2019.11 —— 2022.02

#### 中软国际科技服务有限公司  前端开发工程师

工作职责：

1. 负责开发维护 HiAnalyticsJSSDK  埋点工具
2. 参与花瓣搜索端测 render的前端开发

### 2017.02 —— 2019.10

#### 江苏智途科技过分有限公司 前端开发工程师

工作职责：

1. 负责 公司官网前端开发
2. 参与混合 APP h5前端开发及打包发布
3. 参与前端基础组件搭建

### 2016.08——2016.12

#### 江苏舜天国际集团江都工具有限公司

工作职责：

1. 负责公司公众号h5前端开发


## 项目经历

### 天穹情报分析台：2022.05——2024.01



### 成都大数据项目：2023.08——2024.01



### 长沙图书馆项目要：2023.01——2023.08



### 花瓣搜索：2019.12——2022.01

华为对标谷歌搜索的搜索项目，分为APP版本和PC版，基本功能是将搜索结果里不同类型数据渲染成对应卡片形式一一展示。
我主要参与APP版本的h5和PC版本的前端开发以及快卡的接入，主要包括打车出行同时支持打开接入的打车APP或下载APP、智能问答模块可以跟APP进行原生交互、通过iframe实现基础的广告接入功能以及原生JS实现的网页翻译功能

**技术栈**：Vue SSR + Egg / python+jinja（低版本）

**项目业绩**：最美天气广告接入，开始为搜索项目盈利。导航打车也是搜索为数不多的可盈利功能

**项目链接**：[花瓣搜索](https://www.petalsearch.com)



### HiAnalyticsJSSDK：2019.11——2022.02

一个内部基础埋点SDK，支持统计PV、UV、点击事件以及自定义事件。
我主要负责SDK开发和维护

**技术栈**：Javascript 、Typescript

**项目业绩**：为华为视频，华为钱包，玩机技巧，鸿蒙等项目组提供打点服务，提高了相关业务的用户行为分析功能



### 如皋党建app

负责h5前端开发，打包成android和ios应用提供用户使用

**技术栈**：Vue全家桶 + Cordova



### 安福宏观经济库

负责大数据产品应用层数据可视化前端开发，主要包括前台数据可视化和后端数据管理系统。

**技术棧**：Html+CSS+Jquery+Echarts+Raphel+Datatable

## 教育经历

  2012.09 -- 2016.06
  徐州工程学院 | 信息与计算科学（统计与精算）| 本科

## 链接

  * [GitHub](https://github.com/YangChen0930)
  * [博客](https://www.yzyeleven.xyz/)

  > 如果我符合贵公司的要求，可以邮件或手机联系我。

    邮箱：17315205682@163.com

    手机：17315205682`
    }
  },
  created() {
    this.makeResume()
  },

  methods: {
    makeResume: async function () {
      await this.progressivelyShowStyle(0)
      await this.progressivelyShowResume()
      await this.progressivelyShowStyle(1)
      await this.showHtml()
      await this.progressivelyShowStyle(2)
    },
    showHtml: function () {
      return new Promise((resolve, reject) => {
        this.enableHtml = true
        resolve()
      })
    },
    progressivelyShowStyle(n) {
      return new Promise((resolve, reject) => {
        let interval = this.interval
        let showStyle = async function () {
          let style = this.fullStyle[n]
          if (!style) {
            return
          }
          // 计算前 n 个 style 的字符总数
          let length = this.fullStyle
            .filter((_, index) => index <= n)
            .map((item) => item.length)
            .reduce((p, c) => p + c, 0)
          let prefixLength = length - style.length
          if (this.currentStyle.length < length) {
            let l = this.currentStyle.length - prefixLength
            let char = style.substring(l, l + 1) || ' '
            this.currentStyle += char
            if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
              this.$nextTick(() => {
                this.$refs.styleEditor.goBottom()
              })
            }
            setTimeout(showStyle, interval)
          } else {
            resolve()
          }
        }.bind(this)
        showStyle()
      })
    },
    progressivelyShowResume() {
      return new Promise((resolve, reject) => {
        let length = this.fullMarkdown.length
        let interval = this.interval
        let showResume = () => {
          if (this.currentMarkdown.length < length) {
            this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
            // let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
            let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
            if (prevChar === '\n' && this.$refs.resumeEditor) {
              this.$nextTick(() => this.$refs.resumeEditor.goBottom())
            }
            setTimeout(showResume, interval)
          } else {
            resolve()
          }
        }
        showResume()
      })
    }
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

html {
  min-height: 100vh;
}

* {
  box-sizing: border-box;
}
</style>

<template>
  <div id="app">
    <StyleEditor ref="styleEditor"
                 :code="currentStyle"></StyleEditor>
    <ResumeEditor ref="resumeEditor"
                  :markdown="currentMarkdown"
                  :enableHtml="enableHtml"></ResumeEditor>
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
  data () {
    return {
      interval: 40,
      currentStyle: '',
      enableHtml: false,
      fullStyle: [
        `/*
  * Inspired by http://strml.net/
  * 各位考官，大家好，我是杨晨
  * 下面，就由我就来写一份简历！
  */

  /* 首先给所有元素加上过渡效果 */
  * {
    -webkit-transition: all .3s;
    transition: all .3s;
  }
  /* 白色背景太单调了，更换一下背景颜色 */
  html {
    color: rgb(222,222,222); background: rgb(0,43,54);
  }
  /* 文字离边框太近了 */
  .styleEditor {
    padding: .5em;
    border: 1px solid;
    margin: .5em;
    overflow: auto;
    width: 45vw; height: 90vh;
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
    -webkit-transform: rotateY(10deg) translateZ(-100px) ;
            transform: rotateY(10deg) translateZ(-100px) ;
  }

  /* 接下来准备好一个编辑器 */
  .resumeEditor{
    position: fixed; right: 0; top: 0;
    padding: .5em;  margin: .5em;
    width: 48vw; height: 90vh;
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
`],
      currentMarkdown: '',
      fullMarkdown: `杨晨
----

  一个致力于全栈的前端攻城狮

  技能
  ----
  * 开发过项目类型：PC端 + 移动端、混合app
  * 使用过前端技术 ： HTML5、CSS3、Flex、原生JS、jquery、Vue、小程序、NodeJS
  * 使用过的技术栈：Vue + Vuex +Vue-Router +Axios + Stylus +ES6 +Webpack + Babel
  * 前后端分离
  * 响应式开发布局
  * 前端工程化开发，深入理解git/SVN版本管理工具
  * 前端框架(vue,react)，实现前端单页面应用状态管理及路由机制

  工作经历
  ----
  1. 江苏舜天国际集团江都工具有限公司
  2. 江苏智途科技过分有限公司

  项目经历
  ----

  教育经历
  ----
  2012.09 -- 2016.06
  徐州工程学院 | 信息与计算科学（统计与精算）| 本科

  链接
  ----

  * [GitHub](https://github.com/YangChen0930)
  * [码云](https://gitee.com/YangChengg)

  > 如果我符合贵公司的要求，可以邮件或手机联系我。
    --联系方式：邮箱：18205145325@163.com/手机：17315205682
`
    }
  },
  created () {
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
    progressivelyShowStyle (n) {
      return new Promise((resolve, reject) => {
        let interval = this.interval
        let showStyle = async function () {
          let style = this.fullStyle[n]
          if (!style) { return }
          // 计算前 n 个 style 的字符总数
          let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
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
    progressivelyShowResume () {
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
  font-family: "Avenir", Helvetica, Arial, sans-serif;
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

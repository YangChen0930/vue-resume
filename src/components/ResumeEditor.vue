<template>
  <div class="resumeEditor" :class="{ htmlMode: enableHtml }" ref="container">
    <div v-if="enableHtml" v-html="result"></div>
    <pre v-else>{{ result }}</pre>
  </div>
</template>

<script>
import MarkdownIt from 'markdown-it'
export default {
  props: ['markdown', 'enableHtml'],
  name: 'ResumeEditor',
  computed: {
    result: function () {
      return this.enableHtml ? this.md.render(this.markdown) : this.markdown
    }
  },
  created() {
    this.md = new MarkdownIt({
      html: true,
      linkify: true
    })
  },
  methods: {
    goBottom: function () {
      this.$refs.container.scrollTop = 100000
    },
    goTop: function () {
      this.$refs.container.scrollTop = 0
    }
  }
}
</script>

<style scoped>
.htmlMode {
  animation: flip 2s;
}

@keyframes flip {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>

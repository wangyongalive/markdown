<template>
  <div id="previewPane" v-html="notePreview">

  </div>
</template>

<script>
  import marked from 'marked';

  export default {
    name: 'previewPane',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    },
    props: ['selectedNotechild'],
    computed: {
      notePreview() {
        return this.selectedNotechild ? marked(this.selectedNotechild.content) : ''
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

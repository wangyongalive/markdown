<template>
  <div v-html="notePreview">
  </div>
</template>

<script>
  import marked from 'marked';

  export default {
    name: 'previewPane',
    props: ['selectedNote'],
    computed: {
      notePreview() {
        return this.selectedNote ? marked(this.selectedNote.content) : ''
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

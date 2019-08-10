<template>
  <div id="sideBar">
    <!--工具栏-->
    <div class="toolbar">
      <!--添加笔记案例-->
      <button @click="addNote" class="btn-toolbar" :title="`${titlechild} note(s) already`">
        <i class="material-icons">add</i>
        Add note
      </button>
    </div>
    <div class="notes">
      <div class="note" v-for="note of sortedNoteschild" :class="{selected: note === selectedNotechild}"
           @click="selectNote(note)">
        <i class="icon material-icons" v-if="note.favorite">star</i>{{note.title}}
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'sideBar',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    },
    // 父向子 传递值
    props: ['titlechild', 'sortedNoteschild', 'selectedNotechild'],
    methods: {
      addNote() {
        // 触发父组件的事件
        this.$emit('addNotechild')
      },
      selectNote(note) {
        this.$emit('selectNotechild', note)
      }
    }
  }
</script>

<style scoped>
  .material-icons {
    font-size: 24px;
    line-height: 1;
    vertical-align: middle;
  }
  .icon {
    font-size: 20px;
    float: right;
  }

  .btn-toolbar {
    border-radius: 3px;
    border: none;
    display: inline-block;
    width: 100%;
    padding: 8px 12px;
    cursor: pointer;
    background: #40b883;
    color: white;
    margin: 2px;
    box-sizing: border-box; /*宽度包括  内容 边宽 内边距*/
  }
  .btn-toolbar:hover {
    background: #63c89b;
  }

  .note.selected {
    background: #40b883;
    color: white;
  }
</style>

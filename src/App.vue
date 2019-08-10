<template>
  <div id="app">
    <silder-bar :titlechild="notes.length"
                @addNotechild="addNote"
                :sortedNoteschild="sortedNotes"
                :selectedNotechild="selectedNote"
                @selectNotechild="selectNote"
    ></silder-bar>
    <template v-if="notes.length">
      <main-pane
        :selectedNotechild="selectedNote"
        @favoriteNotechild="favoriteNote"
        @removeNotechild="removeNote"
      ></main-pane>
      <preview-pane
        :selectedNotechild="selectedNote"
      ></preview-pane>
    </template>
  </div>
</template>

<script>
  import silderBar from './components/sideBar'
  import mainPane from './components/mainPane'
  import previewPane from './components/previewPane'

  export default {
    name: 'App',
    components: {
      silderBar,
      mainPane,
      previewPane
    },
    data() {
      return {
        notes: JSON.parse(localStorage.getItem('notes')) || [],
        selectedId: localStorage.getItem('selected-id') || null,  // 选中的id  来自localstore
      }
    },
    methods: {
      addNote() {
        const time = Date.now();
        const note = {
          id: String(time),
          title: '新建笔记本' + (this.notes.length + 1),
          content: '**欢迎使用markdown** ',
          created: time,
          favorite: false,
        };
        this.notes.push(note); // 添加到列表中
        this.selectNote(note);
      },
      selectNote(note) {
        this.selectedId = note.id; // 获取被选中对象的id
      },
      favoriteNote() { // 取反
        this.selectedNote.favorite ^= true
      },
      removeNote() {
        // 判断是否要删除
        if (this.selectedNote && confirm('Delete the note?')) {
          const index = this.notes.indexOf(this.selectedNote);
          if (index !== -1) {
            this.notes.splice(index, 1)
          }
        }
      },
      saveNotes() {
        localStorage.setItem('notes', JSON.stringify(this.notes));
        console.log('Notes saved!', new Date());
      },
    },
    computed: {
      selectedNote() { //  运回与selectedId匹配的笔记 获取被选中的对象
        console.log('改变了选中对象');
        return this.notes.find(note => note.id === this.selectedId)
      },
      sortedNotes() { // 对笔记进行排序  slice取副本
        return this.notes.slice().sort((a, b) => a.created - b.created)
          .sort((a, b) => (a.favorite === b.favorite) ? 0 : a.favorite ? -1 : 1)
      },
    },
    watch: {
      notes: {
        handler: 'saveNotes',
        deep: true
      },
      selectedId(val) {
        localStorage.setItem('selected-id', val)
      }
    }
  }
</script>

<style>
  /*这里高度要设置100% 不然后面继承的值就不是屏幕的高度*/
  html, body {
    height: 100%;
    margin: 0;
    padding: 0;
  }

  #app {
    display: flex;
    height: 100%;
    width: 100%;
  }

  #sideBar {
    flex: 2;
    height: 100%;
    background-color: #f8f8f8;
  }

  #mainPane {
    flex: 4;
    height: 100%;
    border-right: solid 3px #808080;
    border-left: solid 3px #808080;
    display: flex;
    flex-direction: column;
  }

  #previewPane {
    flex: 4;
    height: 100%;
    padding-left: 4px;
  }

</style>

<template>
  <div >
    <div class="toolbar">
      <input class="inputTitle" v-model="selectedNotechild.title" placeholder="输入笔记名称..."/>
      <button class="btnFavorite" @click="favoriteNotechild" title="Favorite note">
        <i class="material-icons">
          {{ selectedNotechild.favorite ? 'star' : 'star_border' }}
        </i>
      </button>
      <button class="btnRemove" @click="removeNotechild" title="Remove note">
        <i class="material-icons">delete</i>
      </button>
    </div>
    <textarea v-model="selectedNotechild.content"></textarea>
    <!--状态栏-->
    <div class="bottombar">
           <span class="date">
               <span class="label">Created</span>
               <span class="value">{{ selectedNotechild.created | date }}</span>
          </span>
      <span class="lines">
              <span class="label">Lines</span>
              <span class="value">{{ linesCount }}</span>
          </span>
      <span class="words">
              <span class="label">Words</span>
              <span class="value">{{ wordsCount }}</span>
         </span>
      <span class="characters">
            <span class="label">Characters</span>
            <span class="value">{{ charactersCount }}</span>
        </span>
    </div>
  </div>
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'mainPane',
    data() {
      return {}
    },
    props: ['selectedNotechild'],
    methods: {
      favoriteNotechild() {
        this.$emit('favoriteNotechild')
      },
      removeNotechild() {
        this.$emit('removeNotechild')
      }
    },
    computed: {
      linesCount() {
        if (this.selectedNotechild) {
          // 计算有多少行
          return this.selectedNotechild.content.split(/\r\n|\r|\n/).length
        }
      },
      wordsCount() {
        // 计算有多少个单词
        if (this.selectedNotechild) {
          let s = this.selectedNotechild.content;
          // 将换行符转换为空格
          s = s.replace(/\n/g, ' ');
          // 排除开始和结束空格
          s = s.replace(/(^\s*)|(\s*$)/gi, '');
          // 将2个或更多重复的空格转换为1
          s = s.replace(/[ ]{2,}/gi, ' ');
          // 返回空格数
          return s.split(' ').length;
        }
      },
      charactersCount() {
        // 字母个数
        if (this.selectedNotechild) {
          return this.selectedNotechild.content.split('').length
        }
      },
    },
    filters: {
      date: function (value) {
        return moment(value).format('DD/MM/YY, HH:mm')
      }
    }
  }
</script>


<style scoped>

  .inputTitle {
    height: 34px;
    border: solid 2px #ade2ca;
    border-radius: 3px;
    padding: 6px 10px;
    background: #f0f9f5;
    color: #666;
    margin: 2px;
    box-sizing: border-box;
  }

  .btnFavorite {
    height: 34px;
    box-sizing: border-box;
    background: #40b883;
    color: white;
    border: 0px;
    padding-top: 1px;
    padding-bottom: 1px;
  }

  .btnRemove {
    height: 34px;
    box-sizing: border-box;
    background: #40b883;
    color: white;
    border: 0px;
    padding-top: 1px;
    padding-bottom: 1px;
  }

  .material-icons {
    font-size: 24px;
    line-height: 1; /*居中对齐*/
    vertical-align: middle;
  }

  textarea {
    resize: none;
    border: none;
    box-sizing: border-box;
    margin: 3px;
    outline: none !important; /*外面的边框消失*/
    flex: 1; /*弹性盒子*/
  }

  .bottombar {
    color: #777;
    padding: 4px;
    box-sizing: border-box;
  }

  .bottombar .label {
    color: #bbb;
  }

  .bottombar > span {
    margin-right: 10px;
  }
</style>

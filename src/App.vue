<template>
  <div class="app">
    <h1>🚩收藏網址小工具🚩</h1>

    <!-- 使用者輸入網址與標題 -->
    <input v-model="url" placeholder="請輸入網址" />
    <input v-model="title" placeholder="請輸入標題（選填）" />
    <button @click="addBookmark">新增</button>

    <!-- 顯示清單 -->
    <ul>
      <li v-for="(item, index) in bookmarks" :key="index">
        <a :href="item.url" target="_blank">{{ item.title || item.url }}</a>
        <button @click="deleteBookmark(index)">刪除</button>
      </li>
    </ul>

    <!-- 清空全部 -->
    <button @click="clearAll" v-if="bookmarks.length">清空全部</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      url: '',        // 使用者輸入的網址
      title: '',      // 使用者輸入的標題（可選）
      bookmarks: []   // 收藏清單陣列
    }
  },
  created() {
    // 一進來時讀取 localStorage 的資料
    const saved = localStorage.getItem('bookmarks')
    if (saved) {
      this.bookmarks = JSON.parse(saved)
    }
  },
  watch: {
    // 當 bookmarks 有變化，就存到 localStorage
    bookmarks: {
      handler(newVal) {
        localStorage.setItem('bookmarks', JSON.stringify(newVal))
      },
      deep: true
    }
  },
  methods: {
    // 新增收藏
    addBookmark() {
      if (!this.url) {
        alert('請輸入網址')
        return
      }
      this.bookmarks.push({
        url: this.url,
        title: this.title
      })
      this.url = ''
      this.title = ''
    },
    // 刪除單一項目
    deleteBookmark(index) {
      this.bookmarks.splice(index, 1)
    },
    // 清空全部
    clearAll() {
      if (confirm('是否要清空所有收藏？')) {
        this.bookmarks = []
      }
    }
  }
}
</script>

<style>
.app {
  max-width: 600px;
  margin: auto;
  padding: 2em;
  font-family: sans-serif;
}
input {
  margin: 0.5em;
}
button {
  margin: 0.5em;
}
</style>
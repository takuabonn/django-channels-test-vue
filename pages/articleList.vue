<template>
  <div>
    <v-btn @click="dialog = true">投稿</v-btn>
    <v-data-table
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      class="elevation-1"
    >
    </v-data-table>
    <v-dialog v-model="dialog" width="600">
      <ArticlePostCardVue :create-article="createArticle" />
    </v-dialog>
  </div>
</template>
<script>
import ArticlePostCardVue from '../components/ArticlePostCard.vue'
export default {
  components: {
    ArticlePostCardVue,
  },
  data: () => ({
    headers: [
      {
        text: 'タイトル',
        align: 'start',
        sortable: false,
        value: 'title',
      },
      { text: '本文', value: 'body' },
      { text: 'ステータス', value: 'status' },
    ],
    desserts: [],
    dialog: false,
    ws: new WebSocket(`ws://localhost:8000/ws/article/`),
  }),
  created() {
    this.ws.onmessage = (e) => {
      console.log(e.data)
      const messageOb = JSON.parse(e.data)
      this.dialog = false
      alert(messageOb.status)
    }
  },
  mounted() {
    this.listArticle()
  },
  methods: {
    async createArticle(article) {
      const url = `http://localhost:8000/api/articles/request/`
      await fetch(url, {
        method: 'POST', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
        headers: {
          Accept: 'application/json',
          'Content-Type': 'application/json',
        },

        body: JSON.stringify({ title: article.title, body: article.body }),
      })
        .then((res) => {
          this.listArticle()
        })
        .catch((err) => {
          this.dialog = false
          console.log(err)
        })
    },
    async listArticle() {
      const url = `http://localhost:8000/api/articles/`
      await fetch(url, {
        method: 'GET', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
        headers: {
          Accept: 'application/json',
          'Content-Type': 'application/json',
        },
      })
        .then((res) => res.json())
        .then((data) => {
          this.desserts = data
        })
        .catch((err) => {
          console.log(err)
        })
    },
  },
}
</script>
<style scoped></style>

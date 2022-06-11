<template>
  <v-card class="mx-auto">
    <v-card-text>
      <v-text-field v-model="title" label="タイトル"> </v-text-field>
    </v-card-text>
    <v-card-text>
      <v-text-field v-model="body" label="本文"> </v-text-field>
    </v-card-text>
    <v-card-actions>
      <v-btn text color="teal accent-4" @click="save"> 投稿 </v-btn>
    </v-card-actions>
  </v-card>
</template>
<script>
export default {
  props: {
    createArticle: {
      type: Function,
      required: true,
    },
  },
  data: () => ({
    title: '',
    body: '',
  }),
  methods: {
    save() {
      this.createArticle({
        title: this.title,
        body: this.body,
      })
    },
  },
}
</script>
<style scoped></style>

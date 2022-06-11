<template>
  <v-card class="mx-auto">
    <v-card-text>
      <v-text-field v-model="name" label="アカウント名"> </v-text-field>
    </v-card-text>
    <v-card-text>
      <v-text-field v-model="password" label="パスワード"> </v-text-field>
    </v-card-text>
    <v-card-actions>
      <v-btn text color="teal accent-4" @click="login">ログイン</v-btn>
    </v-card-actions>
  </v-card>
</template>
<script>
export default {
  data: () => ({
    name: '',
    password: '',
  }),
  methods: {
    async login() {
      const url = `http://localhost:8000/api-token-auth/`
      await fetch(url, {
        method: 'POST', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
        headers: {
          'Content-Type': 'application/json',
          // 'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: JSON.stringify({ username: this.name, password: this.password }),
      })
        .then(async (res) => {
          const jwt = await res.json()
          localStorage.setItem('jwt', jwt)
        })
        .catch((err) => {
          console.log(err)
        })
    },
  },
}
</script>
<style scoped></style>

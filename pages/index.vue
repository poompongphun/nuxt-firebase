<template>
  <div>
    <h1>Hello World</h1>
    <p>This is a simple example of a Vue.js component.</p>
    <div v-for="(item, i) in count" :key="i">
      {{ item.name }}
    </div>
    <v-btn color="success" @click="addItem">text</v-btn>
    <v-btn color="success" @click="sub">sub</v-btn>
    <v-btn color="error" @click="unsub">unsub</v-btn>
    <v-text-field
      v-model="username"
      name="name"
      label="label"
      id="id"
    ></v-text-field>
    <v-text-field
      v-model="password"
      name="name"
      label="label"
      id="id"
    ></v-text-field>
    <v-btn color="success" @click="login">login</v-btn>
    <v-btn color="error" @click="logout">logout</v-btn>
  </div>
</template>

<script>
// import contentComponent from '@/components/contentComponent'
import { mapGetters } from 'vuex'
export default {
  // components: {
  //   contentComponent,
  // },
  /**  Bind Vuexfire on client-side: */
  name: 'IndexPage',
  data: () => ({
    username: 'hello@gmail.com',
    password: '123456',
  }),
  computed: {
    ...mapGetters(['count']),
  },
  async mounted() {
    try {
      await this.$store.dispatch('bindCountDocument')
    } catch (e) {
      console.error(e)
    }
  },
  methods: {
    addItem() {
      // const increment = this.$fireModule.firestore.FieldValue.increment(amount)
      this.$fire.firestore.collection('items').add({
        detail: '',
        img: '',
        name: String(new Date(Date.now())),
        price: 0,
      })
    },
    async unsub() {
      try {
        await this.$store.dispatch('unbindCountDocument')
      } catch (e) {
        console.error(e)
      }
    },
    async sub() {
      try {
        await this.$store.dispatch('bindCountDocument')
      } catch (e) {
        console.error(e)
      }
    },
    async login() {
      try {
        await this.$fire.auth.signInWithEmailAndPassword(
          this.username,
          this.password
        )
      } catch (e) {
        console.error(e)
      }
    },
    async logout() {
      try {
        await this.$fire.auth.signOut()
      } catch (e) {
        console.error(e)
      }
    },
  },
}
</script>
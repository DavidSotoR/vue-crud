<template>
  <q-card style="width: 350px" v-if="ready">
        <q-card-section>
          <div class="text-h6">Usuario:</div>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-input label="Usuario"  v-model="usuario" autofocus />
          <q-input label="Password" v-model="password" />
          <q-input label="Email"  v-model="email" />
        </q-card-section>
        <q-card-actions align="right" class="text-primary">
          <q-btn @click="closeDlg" flat label="Cancel" />
          <q-btn @click="updateUser" flat label="Crear" :disable="disableBtn" />
        </q-card-actions>
    </q-card>
    <q-card style="width: 350px; height: 350px; padding: 140px 0 0 0" v-else>
      <q-card-actions align="center">
        <q-spinner
        color="primary"
        size="5.5em"
      />
      </q-card-actions>
    </q-card>
</template>

<script>
import axios from 'axios'
export default {
  name: 'DlgUpdtUsuario',
  props: ['id_user'],
  data () {
    return {
      api: '/api',
      disableBtn: false,
      usuario: '',
      password: '',
      email: '',
      user: {},
      ready: false
    }
  },
  methods: {
    async getDataUser () {
      const resp = await axios.get(`${this.api}/usuarios/${this.id_user}`)
      this.user = resp.data
      this.ready = true
      console.log(resp);
      this.usuario = this.user.username
      this.email = this.user.email
      this.password = this.user.password
    },
    updateUser(){
      this.disableBtn = true
      const reqData = {
        // _id: this.id_user,
        username: this.usuario,
        password: this.password,
        email: this.email
      }
      console.log(reqData);
      axios.put(`${this.api}/usuarios/${this.id_user}`, reqData)
        .then(resp=>{
          console.log(resp);
          this.$emit('closeDlgUpdtUser')
        }).catch(err=>{
          this.disableBtn = false
          console.log(err);
        })
    },
    closeDlg () {
      this.$emit('closeDlgUpdtUser')
    }
  },
  mounted () {
    this.getDataUser()
  }
}
</script>

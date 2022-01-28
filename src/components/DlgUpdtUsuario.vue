<template>
  <q-card style="width: 350px" v-if="ready">
        <q-card-section>
          <div class="text-h6">Usuario:</div>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-input label="Usuario"  v-model="usuario" autofocus :rules="[val => !!val || 'Campo es requerido', val => val.length >= 3 || 'Use minimo 3 characters']"/>
          <q-input label="Password" v-model="password" :rules="[val => !!val || 'Campo es requerido', val => val.length >= 3 || 'Use minimo 3 characters']"/>
          <q-input label="Email"  v-model="email" :rules="[val => !!val || 'Campo es requerido', val => reEmail.test(val) || 'Ingrese un email valido']"/>
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
      reEmail: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
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
    },
    validateData() {
      let error = false
      var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      if(this.usuario.length < 3) {
        error = true
      }
      if(this.password.length < 3) {
        error = true
      }
      if(this.email.length < 3 || !re.test(this.email)) {
        error = true
      }
      this.disableBtn = error
    }
  },
  mounted () {
    this.getDataUser()
  },
  watch: {
    usuario: function (val) {
      this.validateData()
    },
    password: function (val) {
      this.validateData()
    },
    email: function (val) {
      this.validateData()
    }
  }
}
</script>

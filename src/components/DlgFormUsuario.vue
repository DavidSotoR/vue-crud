<template>
<q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Crear Usuario</div>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-input label="Usuario"  v-model="usuario" autofocus :rules="[val => !!val || 'Campo es requerido', val => val.length >= 3 || 'Use minimo 3 characters']"/>
          <q-input label="Password" v-model="password" :rules="[val => !!val || 'Campo es requerido', val => val.length >= 3 || 'Use minimo 3 characters']"/>
          <q-input label="Email"  v-model="email" :rules="[val => !!val || 'Campo es requerido', val => reEmail.test(val) || 'Ingrese un email valido']"/>
        </q-card-section>
        <q-card-actions align="right" class="text-primary">
          <q-btn @click="closeDlg" flat label="Cancel" />
          <q-btn @click="createUser" flat label="Crear" :disable="disableBtn" />
        </q-card-actions>
      </q-card>
</template>

<script>
import axios from 'axios';
export default {
  name: 'DlgFormUsuario',
  data () {
    return {
      reEmail: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
      disableBtn: true,
      api: '/api',
      usuario: '',
      password: '',
      email: '',
    }
  },
  methods: {
    createUser () {
      this.disableBtn = true
      const reqData = {
        username: this.usuario,
        password: this.password,
        email: this.email
      }
      console.log(reqData);
      axios.post(this.api + '/usuarios', reqData).then(
        resp => {
          console.log(resp);
          this.$emit('closeDlgNewUser')
        }).catch(err=>{
          this.disableBtn = false
          console.log(err);
        })

    },
    closeDlg() {
      this.$emit('closeDlgNewUser')
      console.log('cerrar dlg');
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
  watch: {
    username: function (val) {
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

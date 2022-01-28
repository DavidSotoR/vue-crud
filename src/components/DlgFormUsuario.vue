<template>
<q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Crear Usuario</div>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-input label="Usuario"  v-model="usuario" autofocus />
          <q-input label="Password" v-model="password" />
          <q-input label="Email"  v-model="email" />
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
      disableBtn: false,
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
    }
  }
}
</script>

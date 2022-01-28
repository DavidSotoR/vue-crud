<template>
  <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Se eliminara el siguiente usuario:</div>
          <div class="text-h6">Usuario > {{user.username}}</div>
        </q-card-section>
        <q-card-actions align="right" class="text-primary">
          <q-btn @click="closeDlg" flat label="Cancel" />
          <q-btn :disable="disableBtn" @click="deleteUsuario" flat label="Eliminar"/>
        </q-card-actions>
      </q-card>
</template>

<script>
import axios from 'axios'
export default {
  name: 'DlgDeleteUsuario',
  props: ['user'],
  data () {
    return {
      api: '/api',
      disableBtn: false,
    }
  },
  methods: {
    closeDlg(){
      this.$emit('closeDlgDeleteUser')
    },
    deleteUsuario() {
      this.disableBtn = true
      axios.delete(`${this.api}/usuarios/${this.user._id}`).then(resp=>{
        console.log(resp);
        this.$emit('closeDlgDeleteUser')
      }).catch(err=>{
        this.disableBtn = false
        console.log(err);
      })
    }
  }
}
</script>

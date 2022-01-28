<template>
  <q-page class="flex">
    <div style="width: 100vw; padding: 10px">
      <q-table
      :rows="dataTable"
      :filter="filter"
      :columns="dataColumns"
      row-key="name">
      <template v-slot:top>
        <h5>Usuarios</h5>
        <q-input style="margin: 0 10px" filled borderless dense debounce="300" v-model="filter" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search" />
          </template>
        </q-input>
        <q-space />
        <q-btn @click="openDlgNewUser" color="primary" label="Nuevo"/>
      </template>

      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="username" :props="props">
            {{ props.row.username }}
          </q-td>
          <q-td key="password" :props="props">
            {{ props.row.password }}
          </q-td>
          <q-td key="email" :props="props">
            {{ props.row.email }}
          </q-td>
          <q-td key="edit" :props="props">
            <q-btn outline rounded icon="edit" color="orange" @click="openEditUser(props.row._id)" />
          </q-td>
          <q-td key="delete" :props="props">
            <q-btn outline rounded icon="delete" color="red" @click="openDeleteUser(props.row)" />
          </q-td>
        </q-tr>
      </template>
      
      </q-table>
    </div>
    <q-dialog v-model="showDlgNewUser" persistent>
      <DlgFormUsuario @closeDlgNewUser="closeDlgNU" :id_user="idSelected"></DlgFormUsuario>
    </q-dialog>
    <q-dialog v-model="showDlgUpdtUser" persistent>
      <DlgUpdtUsuario @closeDlgUpdtUser="closeDlgUU" :id_user="idSelected"></DlgUpdtUsuario>
    </q-dialog>
    <q-dialog v-model="showDlgDeleteUser" persistent>
      <DlgDeleteUsuario @closeDlgDeleteUser="closeDlgDU" :user="userSelected"></DlgDeleteUsuario>
    </q-dialog>
  </q-page>
</template>

<script>
import axios from 'axios';
import DlgFormUsuario from 'src/components/DlgFormUsuario.vue';
import DlgDeleteUsuario from 'src/components/DlgDeleteUsuario.vue'
import { defineComponent } from 'vue';
import DlgUpdtUsuario from 'src/components/DlgUpdtUsuario.vue';

export default defineComponent({
  components: { DlgFormUsuario, DlgDeleteUsuario, DlgUpdtUsuario },
  name: 'PageIndex',
  data () {
    return {
      filter: '',
      idSelected: null,
      dataTable: [],
      dataColumns: [
        { name: 'username', label: 'Usuario', field: 'username', sortable: true },
        { name: 'password', label: 'Contrasena', field: 'password', sortable: true },
        { name: 'email', label: 'Email', field: 'email', sortable: true },
        { name: 'edit', label: 'Editar', field: 'edit' },
        { name: 'delete', label: 'Borrar', field: 'delete' }
      ],
      api: '/api',
      showDlgNewUser: false,
      showDlgUpdtUser: false,
      showDlgDeleteUser: false,
      userSelected: {},
    }
  },
  methods: {
    async getDataTable () {
      const resp = await axios.get(this.api + '/usuarios')
      this.dataTable = resp.data
    },
    openDlgNewUser () {
      this.showDlgNewUser = !this.showDlgNewUser
    },
    closeDlgNU () {
      this.showDlgNewUser = !this.showDlgNewUser
      this.getDataTable()
    },
    openEditUser(id) {
      this.idSelected = id;
      this.showDlgUpdtUser = !this.showDlgUpdtUser
      console.log(id);
    },
    closeDlgUU(){
      this.idSelected = null
      this.showDlgUpdtUser = !this.showDlgUpdtUser
      this.getDataTable()
    },
    openDeleteUser(user){
      this.userSelected = user
      this.showDlgDeleteUser = !this.showDlgDeleteUser
    },
    closeDlgDU(){
      this.idSelected = null
      this.showDlgDeleteUser = !this.showDlgDeleteUser
      this.getDataTable()
    },
  },
  async mounted () {
    await this.getDataTable()
  }
})
</script>

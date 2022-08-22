<template>
  <v-container>
    <v-card>
      <v-card-text>
        <div align="center">
          Word of the Lorem in sit amet consectetur ipsum dolor in! Dolorum, doloribus! Nulla in sit
          amet consectetur adipisicing elit. Libero, in! the Lorem ipsum Dolorum, doloribus! in sit amet consectetur
          Nulla inventore quae impedit suscipit pariatur, vitae voluptatibus, dicta et eos autem a, nisi ullam sit minus
          mollitia!
        </div>
      </v-card-text>

      <v-text-field type="search" outlined label="Searching..." placeholder="Type something..." single-line
        class="mb-4 px-4 pb-4" hide-details dense append-icon="mdi-account-search" @click:append="show4">
      </v-text-field>
    </v-card>
    <v-card class="pa-4">
      <h1>{{ msg }}</h1>
      <v-data-table :headers="headers" :items="patients" sort-by="gender" class="elevation-1">
        <template v-slot:top>
          <!--Dialogo visualizar pacientes-->
          <v-dialog v-model="dialog" max-width="400px">
            <v-card>
              <div class="py-4 mb-2">
                <v-img style="border-radius: 150px" class="mx-auto"
                  src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg" height="100px" width="100px"></v-img>
              </div>

              <v-card-text>
                <!-- informações pacientes visualizando -->
                <v-row>
                  <v-text-field v-model="patientList.name" label="Nome completo" outlined dense hide-details
                    class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.gender" label="Email" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.gender" label="Gender" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.birth" label="Birth" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.birth" label="Telefone" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.birth" label="Nacionalidade" outlined dense hide-details
                    class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.birth" label="Endereço" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.birth" label="ID (Número de identificação)" outlined dense
                    hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patientList.birth" label="URL para compartilhamento" outlined dense
                    hide-details class="mb-2">
                  </v-text-field>
                </v-row>
              </v-card-text>

              <v-card-actions>
                <v-btn color="error" text @click="close" class="mb-2">
                  Close
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn color="primary" class="mb-2">
                  <v-icon small class="mr-2">mdi-share-variant</v-icon>
                  Share
                </v-btn>

              </v-card-actions>
            </v-card>
          </v-dialog>
        </template>

        <template v-slot:[`item.actions`]="{ item }">
          <v-btn @click="viewItem(item)" class="primary">
            <v-icon small class="mr-2">
              mdi-card-account-details
            </v-icon> view
          </v-btn>
        </template>
      </v-data-table>
    </v-card>
    {{ patients }}
  </v-container>

</template>

<script>
import axios from 'axios'
export default {
  name: 'patientTable',
  props: ['msg'],
  data: () => ({
    show4: true,
    dialog: false,
    headers: [
      { text: 'Name', align: 'left', value: 'name' },
      { text: 'Gender', value: 'gender', align: 'center' },
      { text: 'Birth', value: 'birth', align: 'center' },
      { text: 'Actions', value: 'actions', align: 'center', sortable: false },
    ],
    patients: [],
    listedIndex: -1,
    patientList: {
      name: '',
      gender: 0,
      birth: 0,
    },
    patientDefault: {
      name: '',
      gender: 0,
      birth: 0,
    },
  }),
  watch: {
    dialog(val) {
      val || this.close()
    }
  },
  methods: {
    listPatients() {
      axios
        .get('https://randomuser.me/api/?results=1', {})
        .then((response) => {
          console.log(response.data.results)
          // this.patients = response.data
        })
        .catch((error) => {
          console.log(error)
        })
        .finally(() => (this.isLoading = false));
    },
    initialize() {
      this.listPatients()
      this.patients = [
        {
          name: 'Frozen Yogurt',
          gender: 159,
          birth: 6.0,
        },
        {
          name: 'Ice cream sandwich',
          gender: 237,
          birth: 9.0,
        },
        {
          name: 'Eclair',
          gender: 262,
          birth: 16.0,
        },
        {
          name: 'Cupcake',
          gender: 305,
          birth: 3.7,
        },
        {
          name: 'Gingerbread',
          gender: 356,
          birth: 16.0,
        },
        {
          name: 'Jelly bean',
          gender: 375,
          birth: 0.0,
        },
        {
          name: 'Lollipop',
          gender: 392,
          birth: 0.2,
        },
        {
          name: 'Honeycomb',
          gender: 408,
          birth: 3.2,
        },
        {
          name: 'Donut',
          gender: 452,
          birth: 25.0,
        },
        {
          name: 'KitKat',
          gender: 518,
          birth: 26.0,
        },
      ]
    },
    viewItem(item) {
      this.listedIndex = this.patients.indexOf(item)
      this.patientList = Object.assign({}, item)
      this.dialog = true
    },
    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.patientList = Object.assign({}, this.patientDefault)
        this.listedIndex = -1
      })
    }
  },
  created() {
    this.initialize()
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>

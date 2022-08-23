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
        class="mb-4 px-4 pb-4" hide-details dense append-icon="mdi-account-search" @click:append="loadPatients()">
      </v-text-field>
    </v-card>
    <v-card class="pa-4">
      <h1>{{ msg }}</h1>
      <v-data-table :headers="headers" :items="patients" hide-default-footer :items-per-page="linesTable"
 sort-by="name" :loading="isLoading" class="elevation-1">
        <!--Dialogo visualizar pacientes-->
        <template v-slot:top>
          <v-dialog v-model="dialogDetail" max-width="400px">
            <v-card>
             <div class="py-4 mb-2">
                <v-img style="border-radius: 150px" class="mx-auto" alt="Imagem" height="100px" width="100px" :src="patients[0].picture.medium"></v-img>

              </div>
              <v-card-text>
                <v-row>
                  <v-text-field v-model="patients[0].name.first" label="Name" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patients[0].email" label="Email" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patients[0].gender" label="Gender" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patients[0].dob.date" label="Birth" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patients[0].phone" label="Phone" outlined dense hide-details class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patients[0].location.country" label="Nationality" outlined dense hide-details
                    class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patients[0].location.street.name" label="Location" outlined dense hide-details
                    class="mb-2">
                  </v-text-field>
                  <v-text-field v-model="patients[0].id.value" label="ID" outlined dense hide-details class="mb-2">
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

        <!--Name Column-->
        <template v-slot:[`item.name`]="{ item }">
          <span class="justify-center">
            {{ item.name.first }}
            {{ item.name.last }}
          </span>
        </template>

        <!--Gender Column -->
        <template v-slot:[`item.gender`]="{ item }">
          <span class="justify-center">
            {{ item.gender[0].toUpperCase() + item.gender.substring(1) }}
          </span>
        </template>

        <!--Birth Column -->
        <template v-slot:[`item.birth`]="{ item }">
          <span class="justify-center">
            {{ new Date(item.dob.date).toLocaleDateString() }}
          </span>
        </template>

        <!--Botão para visualizar modal -->
        <template v-slot:[`item.actions`]="{ item }">
          <v-btn @click="viewItem(item)" class="primary">
            <v-icon small class="mr-2">
              mdi-card-account-details
            </v-icon> view
          </v-btn>
        </template>
      </v-data-table>
      <v-card-actions>
        <v-btn :loading="isLoading" class="primary mx-auto px-4" @click="loadPatients()"><v-icon small class="mr-2">mdi-layers-plus</v-icon>Load more</v-btn>
      </v-card-actions>
    </v-card>
  </v-container>

</template>

<script>
const URL_PATIENTS = 'https://randomuser.me/api/?results='
import axios from 'axios'
export default {
  name: 'patientTable',
  props: ['msg'],
  data: () => ({
    linesTable: 1,
    link: null,
    searhPatients: true,
    isLoading: false,
    headers: [
      { text: 'Name', value: 'name', align: 'left' },
      { text: 'Gender', value: 'gender', align: 'center' },
      { text: 'Birth', value: 'birth', align: 'center' },
      { text: 'Actions', value: 'actions', align: 'center', sortable: false },
    ],
    patients: [{
      name: {
        first: null,
        last: null,
        title: null
      },
      email: null,
      phone: null,
      gender: null,
      dob: {
        date: null,
      },
      picture: {
        medium: null,
      },
      location: {
        country: null,
        street: {
          name: null,
          number: null,
        }
      }
    }],
    detailsedit: {},
    dialogDetail: false
  }),
  // watch: {
  //   dialog(val) {
  //     val || this.close()
  //   }
  // },
  methods: {
    listPatients() {
      this.isLoading = true
      axios
        .get(URL_PATIENTS + this.linesTable, {})
        .then((response) => {
          console.log(response.data.results)
          this.patients = response.data.results
        })
        .catch((error) => {
          console.log(error)
        })
        .finally(() => (this.isLoading = false));
    },
    initialize() {
      this.listPatients()
    },
    viewItem(item) {
      this.detailsedit = item
      this.dialogDetail = true
    },
    close() {
      this.dialogDetail = false
    },
    loadPatients() {
      this.linesTable += 5
      this.listPatients()
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

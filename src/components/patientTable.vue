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
      <!-- Patients search -->
      <v-text-field v-model="search" outlined label="Searching..." placeholder="Type something..." single-line
        class="mb-4 px-4 pb-4" hide-details dense append-icon="mdi-account-search">
      </v-text-field>
    </v-card>
    <v-card class="pa-4">
      <h1>{{ msg }}</h1>
      <v-data-table :headers="headers" :search="search" :items="patients" hide-default-footer
        :items-per-page="linesTable" sort-by="name" :loading="isLoading" class="elevation-0">
        <!-- Detail patients -->
        <template v-slot:top>
          <v-dialog v-model="dialogDetail" max-width="480px">
            <v-card>
              <div class="py-4 mb-2">
                <v-img style="border-radius: 150px" class="mx-auto" alt="Imagem" height="100px" width="100px"
                  :src="detailsedit.imageMedium"></v-img>
              </div>
              <v-card-text>
                  <v-row>
                    <v-col cols="12" md="8">
                      <v-text-field cols="12" col="4" v-model="detailsedit.fullName" readonly label="Name" outlined dense
                      hide-details class="mb-2" />
                    </v-col>
                    <v-col cols="12" md="4">
                      <v-text-field cols="12" col="4" v-model="detailsedit.gender" readonly label="Gender" outlined dense
                      hide-details class="mb-2" />
                    </v-col>
                  </v-row>

                <v-text-field v-model="detailsedit.email" readonly label="Email" outlined dense hide-details
                  class="mb-2" />
                
                <v-row>
                  <v-col cols="12" md="6" class="ma-0">
                    <v-text-field v-model="detailsedit.phone" readonly label="Phone" outlined dense hide-details
                      class="mb-2" />
                  </v-col>
                  <v-col cols="12" md="6" class="ma-0">
                    <v-text-field v-model="detailsedit.cell" readonly label="Cell" outlined dense hide-details
                      class="mb-2" />
                  </v-col>
                </v-row>
                
                <v-row>
                  <v-col cols="12" md="6" class="ma-0 pt-0">
                    <v-text-field v-model="detailsedit.birth" readonly label="Birth" outlined dense hide-details
                      class="mb-2" />
                  </v-col>
                  <v-col cols="12" md="6" class="ma-0 pt-0">
                    <v-text-field v-model="detailsedit.country" readonly label="Nationality" outlined dense hide-details
                      class="mb-2" />
                  </v-col>
                </v-row>
                <div class="person__map" v-if="false">
                  <iframe class="rounded" width="100%" height="150" frameborder="0" borde scrolling="no"
                    marginheight="0" marginwidth="0"
                    :src="'https://maps.google.com/maps?q=' + detailsedit.latitude + ',' + detailsedit.longitude + '&z=7&amp;output=embed'">
                  </iframe>
                </div>
                <v-text-field v-model="detailsedit.fullAddress" label="Location" outlined dense hide-details
                  class="mb-2" />
                <v-text-field v-model="detailsedit.id" label="ID" outlined dense hide-details class="mb-2" />
              </v-card-text>
              <v-card-actions class="pa-6">
                <v-btn color="error" text @click="close">
                  Close
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn color="primary">
                  <v-icon small class="mr-2">mdi-content-copy</v-icon>
                  share link
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </template>
        <!-- Details patients button -->
        <template v-slot:[`item.actions`]="{ item }">
          <v-btn @click="viewItem(item)" class="primary">
            <v-icon small class="mr-2">
              mdi-card-account-details
            </v-icon> view
          </v-btn>
        </template>
      </v-data-table>
      <v-card-actions>
        <v-btn :loading="isLoading" class="primary mx-auto mt-4 px-4" @click="loadPatients()">
          <v-icon small class="mr-2">mdi-layers-plus</v-icon>Load more
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
const URL_PATIENTS = 'https://randomuser.me/api/'
const SEED = '2f10116f1799d353'
const VERSION = '1.3' 
import axios from 'axios'
import { mapState } from 'vuex'
export default {
  props: ['msg'],
  name: 'patientTable',
  data: () => ({
    search: '',
    linesTable: 50,
    pageTable: 1,
    link: null,
    searhPatients: true,
    isLoading: false,
    headers: [
      { text: 'Name', value: 'fullName', align: 'left' },
      { text: 'Gender', value: 'gender', align: 'center' },
      { text: 'Birth', value: 'birth', align: 'center', filterable: false, sortable: false },
      { text: 'Actions', value: 'actions', align: 'center', filterable: false, sortable: false },
    ],
    patients: [],
    detailsedit: {},
    dialogDetail: false
  }),
  methods: {
    listPatients() {
      this.isLoading = true
      axios
        .get(
        `${URL_PATIENTS}?page=${this.pageTable}&results=${this.linesTable}&seed=${SEED}&version=${VERSION}`, {})
        .then((response) => {
          this.$store.commit('SET_LIST', response.data.results)
          // console.log(response.data.results)
          this.patients = response.data.results.map(user => ({
            firstName: user.name.first,
            lastName: user.name.last,
            fullName: user.name.first + ' ' + user.name.last,
            gender: user.gender[0]?.toUpperCase() + user.gender.substring(1) || '',
            country: user.location.country + ' - ' + user.nat,
            postcode: user.location.postcode,
            fullAddress: user.location.street.name + ' ' + user.location.street.number + ', ' + user.location.city + ' - ' + user.location.state + ' - ' + user.location.postcode,
            latitude: user.location.coordinates.latitude,
            longitude: user.location.coordinates.longitude,
            birth: new Date(user.dob.date).toLocaleDateString(),
            phone: user.phone,
            cell: user.cell,
            email: user.email,
            imageMedium: user.picture.medium,
            id: user.login.uuid
          }));
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
      this.linesTable += 50
      //this.pageTable += 1
      this.listPatients()
    }
  },
  created() {
    this.initialize()
  },
  computed: mapState({
    patients: state => state.patients
  }),
  watch: {
    patients: {
      handler() {
        this.link = this.patients.length > 0 ? this.patients[0].imageMedium : null
      },
      deep: true
    }
  }
}
</script>
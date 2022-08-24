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
      <v-text-field type="search" outlined label="Searching..." placeholder="Type something..." single-line
        class="mb-4 px-4 pb-4" hide-details dense append-icon="mdi-account-search" @click:append="loadPatients()">
      </v-text-field>
    </v-card>
    <v-card class="pa-4">
      <h1>{{ msg }}</h1>
      <v-data-table :headers="headers" :items="patients" hide-default-footer :items-per-page="linesTable" sort-by="name"
        :loading="isLoading" class="elevation-1">
        <!-- Detail patients -->
        <template v-slot:top>
          <v-dialog v-model="dialogDetail" max-width="460px">
            <v-card>
              <div class="py-4 mb-2">
                <v-img style="border-radius: 150px" class="mx-auto" alt="Imagem" height="100px" width="100px"
                  :src="detailsedit.imageMedium"></v-img>
              </div>
              <v-card-text>
                <v-text-field v-model="detailsedit.fullName" readonly label="Name" outlined dense hide-details class="mb-2" />
                <v-text-field v-model="detailsedit.email" readonly label="Email" outlined dense hide-details class="mb-2" />
                <v-text-field v-model="detailsedit.gender" readonly label="Gender" outlined dense hide-details class="mb-2" />
                <v-text-field v-model="detailsedit.birth" readonly label="Birth" outlined dense hide-details class="mb-2" />
                <v-text-field v-model="detailsedit.phone" readonly label="Phone" outlined dense hide-details class="mb-2" />
                <v-text-field v-model="detailsedit.country" readonly label="Nationality" outlined dense hide-details
                  class="mb-2" />
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
                  <v-icon small class="mr-2">mdi-share-variant</v-icon>
                  Share
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </template>

        <!--Name column-->
        <template v-slot:[`item.name`]="{ item }">
          <span class="justify-center">
            {{ item.firstName }}
            {{ item.lastName }}
          </span>
        </template>

        <!--Gender column -->
        <template v-slot:[`item.gender`]="{ item }">
          <span class="justify-center">
            {{ item.gender[0]?.toUpperCase() + item.gender.substring(1) || '' }}
          </span>
        </template>

        <!-- Birth column -->
        <template v-slot:[`item.birth`]="{ item }">
          <span class="justify-center">
            {{ item.birth }}
          </span>
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
        <v-btn :loading="isLoading" class="primary mx-auto px-4" @click="loadPatients()">
          <v-icon small class="mr-2">mdi-layers-plus</v-icon>Load more
        </v-btn>
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
    linesTable: 5,
    link: null,
    searhPatients: true,
    isLoading: false,
    headers: [
      { text: 'Name', value: 'name', align: 'left' },
      { text: 'Gender', value: 'gender', align: 'center' },
      { text: 'Birth', value: 'birth', align: 'center' },
      { text: 'Actions', value: 'actions', align: 'center', sortable: false },
    ],
    patients: [],
    detailsedit: {},
    dialogDetail: false
  }),
  methods: {
    listPatients() {
      this.isLoading = true
      axios
        .get(URL_PATIENTS + this.linesTable, {})
        .then((response) => {
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
      this.linesTable += 5
      this.listPatients()
    }
  },
  created() {
    this.initialize()
  }
}
</script>
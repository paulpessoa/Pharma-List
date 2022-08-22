<template>
<v-container>
  <patientTable/>
</v-container>
</template>

<script lang="ts">
  import Vue from 'vue'
  import patientTable from '../components/patientTable.vue'

  export default Vue.extend({
    name: 'Home',

    components: {
      patientTable,
    },
  })
</script>

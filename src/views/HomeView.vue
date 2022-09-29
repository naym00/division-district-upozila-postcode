<template>
  <v-container fluid>

    <v-row>
      <v-col class="d-flex" lg="3" md="3" sm="6">
        <v-select clearable v-model="selectedDivisionID" item-value="id" :items="display.divisions" item-text="bn_name" filled label="Select Division"></v-select>
      </v-col>
      <v-col class="d-flex" lg="3" md="3" sm="6">
        <v-select clearable v-model="selectedDistrictID" item-value="id" :items="display.districts" item-text="bn_name" label="Select District"></v-select>
      </v-col>
      <v-col class="d-flex" lg="3" md="3" sm="6">
        <v-select clearable v-model="selectedUpazilaName" item-value="name" :items="display.upazilas" item-text="bn_name" label="Select Upazila"></v-select>
      </v-col>
      <v-col class="d-flex" lg="3" md="3" sm="6">
        <v-select clearable v-model="selectedPostcodes" item-value="postCode" :items="display.postcodes" item-text="postCode" label="Select Postcode"></v-select>
      </v-col>
    </v-row>

    <v-row>
      <v-col lg="6" md="6" sm="12" v-if="display.districts.length">
        <data-table :headers="[{text: 'District(Bangla)', align: 'start', value: 'bn_name'}, {text: 'District(English)', value: 'name'}]" :items="display.districts.map(district => ({bn_name: district.bn_name, name: district.name}))"></data-table>
      </v-col>
      <v-col lg="6" md="6" sm="12" v-if="display.districts.length && display.upazilas.length">
        <data-table :headers="[{text: 'Upazila(Bangla)', align: 'start', value: 'bn_name'}, {text: 'Upazila(English)', value: 'name'}]" :items="display.upazilas.map(upazila => ({bn_name: upazila.bn_name, name: upazila.name}))"></data-table>
      </v-col>
      <v-col lg="12" md="12" sm="12" v-if="display.districts.length && display.upazilas.length && display.postcodes.length">
        <data-table :headers="[{text: 'Upazila', align: 'start', value: 'upazila'}, {text: 'Post Office', value: 'postOffice'}, {text: 'Post Code', value: 'postCode'}]" :items="display.postcodes.map(postcode => ({upazila: postcode.upazila, postOffice: postcode.postOffice, postCode: postcode.postCode}))"></data-table>
      </v-col>
    </v-row>

  </v-container>
</template>


<script>
  import TableComponent from '@/components/TableComponent.vue'

  import bdDivisions from '@/assets/json/bangladesh-geojson-master/bd-divisions.json'
  import bdDistricts from '@/assets/json/bangladesh-geojson-master/bd-districts.json'
  import bdUpazilas from '@/assets/json/bangladesh-geojson-master/bd-upazilas.json'
  import bdPostcodes from '@/assets/json/bangladesh-geojson-master/bd-postcodes.json'

  export default {
    name: 'HomeView',
    components: {
      'data-table': TableComponent
    },
    data: () => ({
      bdDivisions: JSON.parse(JSON.stringify(bdDivisions)),
      selectedDivisionID: '',
      bdDistricts: JSON.parse(JSON.stringify(bdDistricts)),
      selectedDistrictID: '',
      bdUpazilas: JSON.parse(JSON.stringify(bdUpazilas)),
      selectedUpazilaName: '',
      bdPostcodes: JSON.parse(JSON.stringify(bdPostcodes)),
      selectedPostcodes: '',
      display: {divisions: [], districts: [], upazilas: [], postcodes: []}
    }),
    watch: {
      selectedDivisionID: function(){
        if(this.selectedDivisionID == null){
          this.display.districts = []
          this.selectedDistrictID = null
        }
        else{this.prepareDataToDisplay(this.display, 'districts', this.bdDistricts, 'districts', 'division_id', this.selectedDivisionID)}
      },
      selectedDistrictID: function(){
        if(this.selectedDistrictID == null){
          this.display.upazilas = []
          this.selectedUpazilaName = null
        }
        else{this.prepareDataToDisplay(this.display, 'upazilas', this.bdUpazilas, 'upazilas', 'district_id', this.selectedDistrictID)}
      },
      selectedUpazilaName: function(){
        if(this.selectedUpazilaName == null){
          this.display.postcodes = []
        }
        else{this.prepareDataToDisplay(this.display, 'postcodes', this.bdPostcodes, 'postcodes', 'upazila', this.selectedUpazilaName)}
      }
    },
    mounted(){
      this.prepareDataToDisplay(this.display, 'divisions', this.bdDivisions, 'divisions')
    },
    methods: {
      prepareDataToDisplay(receiver, receiverKey, filterObject, filterObjectKey, comparewith = '', comparevalue = ''){
        if(!(comparewith && comparevalue)){receiver[receiverKey] =  filterObject[filterObjectKey]}
        else{receiver[receiverKey] =  filterObject[filterObjectKey].filter(data => data[comparewith] == comparevalue)}
      }
    }
  }
</script>

<style lang="scss">
</style>

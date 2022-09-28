<template>
  <v-container fluid>
    <v-row align="center">
      <v-col class="d-flex" cols="12" sm="6">
        <v-select v-model="selectedDivisionID" item-value="id" :items="divisionsToDisplay" item-text="bn_name" filled label="Select Division">
          <template v-slot:prepend>
            <v-btn @click="cleardata(1)">
              <v-icon>mdi-window-close</v-icon>
            </v-btn>
          </template>
        </v-select>
      </v-col>

      <v-col class="d-flex" cols="12" sm="6">
        <v-select v-model="selectedDistrictID" item-value="id" :items="districtsToDisplay" item-text="bn_name" label="Select District">
          <template v-slot:prepend>
            <v-btn @click="cleardata(2)">
              <v-icon>mdi-window-close</v-icon>
            </v-btn>
          </template>
        </v-select>
      </v-col>

      <v-col class="d-flex" cols="12" sm="6">
        <v-select v-model="selectedUpazilaName" item-value="name" :items="UpazilasToDisplay" item-text="bn_name" label="Select Upazila">
          <template v-slot:prepend>
            <v-btn @click="cleardata(3)">
              <v-icon>mdi-window-close</v-icon>
            </v-btn>
          </template>
        </v-select>
      </v-col>

      <v-col class="d-flex" cols="12" sm="6">
        <v-select :items="PostcodesToDisplay" item-text="postCode" label="Select Postcode" solo>
          <template v-slot:prepend>
            <v-btn @click="cleardata(4)">
              <v-icon>mdi-window-close</v-icon>
            </v-btn>
          </template>
        </v-select>
      </v-col>
    </v-row>

    <v-row v-if="districtsToDisplay.length">
      <template>
        <v-data-table
          :headers="[{text: 'District(Bangla)', align: 'start', value: 'bn_name'}, {text: 'District(English)', value: 'name'}]"
          :items="districtsToDisplay.map(district => ({bn_name: district.bn_name, name: district.name}))"
          :items-per-page="5"
          class="elevation-1"
        ></v-data-table>
      </template>
    </v-row>

    <v-row v-if="districtsToDisplay.length && UpazilasToDisplay.length">
      <template>
        <v-data-table
          :headers="[{text: 'Upazila(Bangla)', align: 'start', value: 'bn_name'}, {text: 'Upazila(English)', value: 'name'}]"
          :items="UpazilasToDisplay.map(upazila => ({bn_name: upazila.bn_name, name: upazila.name}))"
          :items-per-page="5"
          class="elevation-1"
        ></v-data-table>
      </template>
    </v-row>

    <v-row v-if="districtsToDisplay.length && UpazilasToDisplay.length && PostcodesToDisplay.length">
      <template>
        <v-data-table
          :headers="[{text: 'Upazila', align: 'start', value: 'upazila'}, {text: 'Post Office', value: 'postOffice'}, {text: 'Post Code', value: 'postCode'}]"
          :items="PostcodesToDisplay.map(postcode => ({upazila: postcode.upazila, postOffice: postcode.postOffice, postCode: postcode.postCode}))"
          :items-per-page="5"
          class="elevation-1"
        ></v-data-table>
      </template>
    </v-row>

  </v-container>
</template>


<script>
  import bdDivisions from '@/assets/json/bangladesh-geojson-master/bd-divisions.json'
  import bdDistricts from '@/assets/json/bangladesh-geojson-master/bd-districts.json'
  import bdUpazilas from '@/assets/json/bangladesh-geojson-master/bd-upazilas.json'
  import bdPostcodes from '@/assets/json/bangladesh-geojson-master/bd-postcodes.json'

  export default {
    name: 'HomeView',
    data: () => ({
      bdDivisions: JSON.parse(JSON.stringify(bdDivisions)),
      selectedDivisionID: '',
      divisionsToDisplay: [],

      bdDistricts: JSON.parse(JSON.stringify(bdDistricts)),
      selectedDistrictID: '',
      districtsToDisplay: [],

      bdUpazilas: JSON.parse(JSON.stringify(bdUpazilas)),
      selectedUpazilaName: '',
      UpazilasToDisplay: [],

      bdPostcodes: JSON.parse(JSON.stringify(bdPostcodes)),
      PostcodesToDisplay: [],
    }),
    watch: {
      selectedDivisionID: function(){
        this.districtsToDisplay = this.getData(this.bdDistricts, 'districts', 'division_id', this.selectedDivisionID)
      },
      selectedDistrictID: function(){
        this.UpazilasToDisplay = this.getData(this.bdUpazilas, 'upazilas', 'district_id', this.selectedDistrictID)
      },
      selectedUpazilaName: function(){
        this.PostcodesToDisplay = this.getData(this.bdPostcodes, 'postcodes', 'upazila', this.selectedUpazilaName)
      }
    },
    mounted(){
      this.getDivisions()
    },
    methods: {

      getDivisions(){
        this.divisionsToDisplay = this.bdDivisions.divisions
      },

      getData(object, nextStpe, comparewith, comparevalue){
        return object[nextStpe].filter(data => data[comparewith] == comparevalue)
      },
      cleardata(numericalvalue){
        if(numericalvalue == 1){
          // this.divisionsToDisplay = []
          this.districtsToDisplay = []
          this.UpazilasToDisplay = []
          this.PostcodesToDisplay = []
        }
        else if(numericalvalue == 2){
          // this.districtsToDisplay = []
          this.UpazilasToDisplay = []
          this.PostcodesToDisplay = []
        }
        else if(numericalvalue == 3){
          // this.UpazilasToDisplay = []
          this.PostcodesToDisplay = []
        }
        // else{
        //   this.PostcodesToDisplay = []
        // }

      }
    }
  }
</script>

<style lang="scss">
</style>

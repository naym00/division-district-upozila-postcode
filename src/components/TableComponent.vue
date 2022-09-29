<template>
    <v-data-table
      :headers="headers"
      :items="items"
      :items-per-page="3"
      class="elevation-1"
    ></v-data-table>
</template>
  
  
  <script>
  
    export default {
      name: 'TableComponent',
      props: ['headers', 'items']
    }
  </script>

  
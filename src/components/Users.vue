<template>
  <v-container class="pa-10">
     <v-data-table
        :headers="headers"
        :items="this.usersData"
        :items-per-page="10"
        class="elevation-1"
      ></v-data-table>
  </v-container>
</template>

<script>
  export default {
    name: 'Users',
    props:['usersData'],
    data(){
      return{
        headers:[
          { text: 'ID', value: 'id'},
          { text: 'First Name', value: 'firstName'},
          { text: 'Last Name', value: 'lastName'},
          { text: 'Email', value: 'email'},
          { text: 'Created at', value: 'created_at'}
        ]
      }
    },
    mounted(){
    
    },
    methods:{
    
    }
  }
</script>

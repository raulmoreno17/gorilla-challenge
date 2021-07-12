<template >
  <v-app>
    <!-- Main nav bar -->
    <v-app-bar app color="primary" dark>
      <v-btn @click="changeModule('Dashboard')" text>
        <span class="mr-2">Dashboard  <v-icon color="pink" v-if="currentModule == 'Dashboard'">mdi-arrow-left-bold-box-outline</v-icon></span>
      </v-btn>
      <v-btn @click="changeModule('Users')" text>
        <span class="mr-2">Users <v-icon color="pink" v-if="currentModule == 'Users'">mdi-arrow-left-bold-box-outline</v-icon></span>
      </v-btn>
      <v-btn @click="changeModule('ProductsAndOrders')" text>
        <span class="mr-2">Orders and products <v-icon color="pink" v-if="currentModule == 'ProductsAndOrders'">mdi-arrow-left-bold-box-outline</v-icon></span>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Dashboard v-if=" isDataLoaded && currentModule == 'Dashboard'" :usersData="usersData" :productsData="productsData" :ordersData="ordersData" :orderProductData="orderProductData" />
      <Users v-if=" isDataLoaded && currentModule == 'Users' " :usersData="usersData"  />
      <ProductsAndOrders v-if=" isDataLoaded && currentModule == 'ProductsAndOrders'" :orderProductData="orderProductData" :productsData="productsData" :ordersData="ordersData" :usersData="usersData"  />

    </v-main>
  </v-app>
</template>

<script>
import Dashboard from "./components/Dashboard.vue";
import Users from "./components/Users.vue";
import ProductsAndOrders from "./components/ProductsAndOrders.vue";
import XLSX from "xlsx";

export default {
  name: "App",

  components: {
    Dashboard,
    Users,
    ProductsAndOrders,
  },

  data() {
    return {
      currentModule: "Dashboard",
      isDataLoaded : false,
      usersData: null,
      productsData: null,
      ordersData: null,
      orderProductData: null,
    };
  },
  mounted(){
    this.getData()
    
  },
  methods: {
    changeModule(module) {
      this.currentModule = module;
    },

    async getData() {

      // Get data from the URL provided
       await fetch('https://secureassets.evolvemediallc.com/internal/RH/Data%20Sample.xlsx')
        .then((response) => response.blob())
        .then( myBlob => this.processData(myBlob));
    },

    processData(receivedData){   
      // Convert excel to JSON
      const reader = new FileReader();
      reader.readAsBinaryString(receivedData);
      reader.onload = (event) => {
        // Parse data
        const binaryString = event.target.result;
        const workbook = XLSX.read(binaryString, { type: "binary", cellText: false, cellDates: true});
        
        //  Get "Users" worksheet
        const worksheetUsers =  workbook.Sheets[workbook.SheetNames[0]];
        const datasheetOne = XLSX.utils.sheet_to_json(worksheetUsers, { header: 0, raw: false, dateNF: 'yyyy-mm-dd' });
        this.usersData = datasheetOne
        
        //  Get "Products" worksheet
        const worksheetProducts =  workbook.Sheets[workbook.SheetNames[1]];
        const datasheetTwo = XLSX.utils.sheet_to_json(worksheetProducts, { header: 0, raw: false, dateNF: 'yyyy-mm-dd' });
        this.productsData = datasheetTwo

        //  Get "Orders" worksheet
        const worksheetOrders =  workbook.Sheets[workbook.SheetNames[2]];
        const datasheetThree = XLSX.utils.sheet_to_json(worksheetOrders, { header: 0, raw: false, dateNF: 'yyyy-mm-dd' });
        this.ordersData = datasheetThree

        //  Get "OrderProduct" worksheet
        const worksheetOrderProduct =  workbook.Sheets[workbook.SheetNames[3]];
        const datasheetFour = XLSX.utils.sheet_to_json(worksheetOrderProduct, { header: 0 });
        this.orderProductData = datasheetFour
        
        // Tell to the app that now the data is ready
        this.isDataLoaded = true

      };
      
    }

  },
};
</script>

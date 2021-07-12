<template >
  <v-container>

      <v-row>

        <v-col cols="12" sm="4"  class="pa-10">
          <p class="text-h6" align="center">Latest 5 Users created</p>
          <v-simple-table >
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Name</th>
                  <th>Created at</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="users in latestUsers" :key="users.id">
                  <td>{{ users.firstName + " " + users.lastName }}</td>
                  <td>{{ users.created_at }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-col>

        <v-col  cols="12" sm="4"  class="pa-10">
          <p  class="text-h6" align="center">Latest 5 products created</p>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th>Product</th>
                  <th>Created at</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="product in latestProducts" :key="product.id">
                  <td>{{ product.name}}</td>
                  <td>{{ product.created_at }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-col>

        <v-col cols="12" sm="4" class="pa-10">
          <p  class="text-h6" align="center">Latest 5 orders created</p>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th>Order</th>
                  <th>Created at</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="order in latestOrders" :key="order.id">
                  <td>{{ order.id}}</td>
                  <td>{{ order.created_at }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-col>

      </v-row>

      <v-row >

        <v-col cols="12" sm="6" class="px-10">
          <p class="text-h6" align="center">Top 5 products</p>
          <ChartProducts v-if="monthlyOrders != null"  :mostSaleProducts="mostSaleProducts"/>
        </v-col>

        <v-col cols="12" sm="6" class="px-10" >
          <p class="text-h6" align="center">Orders on each month</p>
          <ChartOrders  v-if="monthlyOrders != null" :monthlyOrders="monthlyOrders"/>
        </v-col>

      </v-row>

  </v-container>
</template>

<script>

import ChartProducts from './ChartProducts.vue'
import ChartOrders from './ChartOrders.vue'

  export default {
    name: 'Dashboard',
    components: {
      ChartProducts,
      ChartOrders
    },

    props: ['usersData', 'productsData', 'ordersData', 'orderProductData', 'chartdata', 'options'],
    data(){
      return{
        latestUsers : [],
        latestProducts : [],
        latestOrders : [],
        mostSaleProducts : [],
        monthlyOrders : null,
        isLoaded : false
        


      }
    },
    mounted(){
      this.loadData()
    },
    created(){
      
    },
    methods:{

      loadData(){
       
         this.getLatestUsers()
         this.getLatestProducts()
         this.getLatestOrders()
         this.getMostSaleProducts()
         this.getMonthlyOrders()
         this.isLoaded = true
         
      },

      getLatestUsers(){
        
        // Sort Users
        this.latestUsers = this.usersData.sort((a,b) => (a.created_at < b.created_at) ? 1 : ((b.created_at < a.created_at) ? -1 : 0))
        //  Filter latest users
        this.latestUsers = this.latestUsers.slice(0, 5);
        
      },
      getLatestProducts(){
        // Sort Products
        this.latestProducts = this.productsData.sort((a,b) => (a.created_at < b.created_at) ? 1 : ((b.created_at < a.created_at) ? -1 : 0))
        //  Filter latest products
        this.latestProducts = this.latestProducts.slice(0, 5);
      },
      getLatestOrders(){
        // Sort Orders
        this.latestOrders = this.ordersData.sort((a,b) => (a.created_at < b.created_at) ? 1 : ((b.created_at < a.created_at) ? -1 : 0))
        //  Filter latest orders
        this.latestOrders = this.latestOrders.slice(0, 5);
      },
      getMostSaleProducts(){
        var products = []
        // Save each product and its total quantity
        this.orderProductData.forEach(element => {
          const found = products.some(el => el.product_id === element.product_id);
          if( !found ){
            //  Search the name of product id on the product data
            this.productsData.forEach(productsDataElement => {
              if(productsDataElement.id == element.product_id ){
                 var product = {
                  name: productsDataElement.name,
                  product_id: element.product_id,
                  qty: element.qty
                }
                products.push(product)
              }
            });
          } else{ 
            //  Add the quantity found in the registered product
            products.forEach(product => {
              if(product.product_id == element.product_id){
                product.qty += element.qty
              }
            });
          }
        });
        // Sort products
        products = products.sort((a,b) => (a.qty < b.qty) ? 1 : ((b.qty < a.qty) ? -1 : 0))
        //  Filter most sale products
        products = products.slice(0, 5);
        this.mostSaleProducts = products
        
      },
      getMonthlyOrders(){
        var monthlyOrders = [ 
          {
            month: "Jan",
            totalOrders: 0
          },
          {
            month: "Feb",
            totalOrders: 0
          },
          {
            month: "Mar",
            totalOrders: 0
          },
          {
            month: "Apr",
            totalOrders: 0
          },
          {
            month: "May",
            totalOrders: 0
          },
          {
            month: "Jun",
            totalOrders: 0
          },
          {
            month: "Jul",
            totalOrders: 0
          },
          {
            month: "Aug",
            totalOrders: 0
          },
          {
            month: "Sep",
            totalOrders: 0
          },
          {
            month: "Oct",
            totalOrders: 0
          },
          {
            month: "Nov",
            totalOrders: 0
          },
          {
            month: "Dec",
            totalOrders: 0
          }
        ]
        //  Register each order in its respective month
        this.ordersData.forEach(element => {
          var month = (element.created_at).slice(5,7)
          switch (month) {
            case "01":
              monthlyOrders[0].totalOrders+= 1
              break;
            case "02":
              monthlyOrders[1].totalOrders+= 1
              break;
            case "03":
              monthlyOrders[2].totalOrders+= 1
              break;  
            case "04":
              monthlyOrders[3].totalOrders+= 1
              break;
            case "05":
              monthlyOrders[4].totalOrders+= 1
              break;
            case "06":
              monthlyOrders[5].totalOrders+= 1
              break;
            case "07":
              monthlyOrders[6].totalOrders+= 1
              break;
            case "08":
              monthlyOrders[7].totalOrders+= 1
              break;
            case "09":
              monthlyOrders[8].totalOrders+= 1
              break;
            case "10":
              monthlyOrders[9].totalOrders+= 1
              break;
            case "11":
              monthlyOrders[10].totalOrders+= 1
              break;
            case "12":
              monthlyOrders[11].totalOrders+= 1
              break;
            default:
              break;
          }
        });

        this.monthlyOrders = monthlyOrders
      }

    }
  }
</script>

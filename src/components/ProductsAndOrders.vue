<template>
  <v-container class="pa-10">
    
     <v-row>

      <v-col  cols="12" sm="2">
        <v-data-table
          :headers="orderHeaders"
          :items="this.ordersData"
          :items-per-page="10"
          class="elevation-1"
          @click:row="getOrderDetails"
        ></v-data-table>
      </v-col>

      <v-col cols="12" sm="10">

        <v-card class="pa-10 full-height">
          <div v-if="!isOrderReady"   class="text-subtitle-1"> Select an order ID</div>
          <div v-if="isOrderReady" >
            
            <div>
              <v-simple-table>
                  <tbody>
                    <tr >
                      <td>User ID:</td><td>{{ userDetails.id }}</td>
                    </tr>
                    <tr >
                      <td>Name:</td><td>{{ userDetails.name }}</td>
                    </tr>
                    <tr >
                      <td>Email:</td><td>{{ userDetails.email }}</td>
                    </tr>
                  </tbody>
              </v-simple-table>
            </div>

            <div class="pt-10">
            <v-data-table
              :headers="productHeaders"
              :items="this.productsList"
              :items-per-page="5"
              class="elevation-1"
              v-if=" productsFound "
              
            ></v-data-table>
            </div>

            <div class="pt-10">
              <div v-if=" productsFound " class="text-h5">Total: {{orderTotalCost}}</div>
              <div v-if=" !productsFound "  class="text-subtitle-1">No products found for this order</div>
            </div>
 
          </div>       
        </v-card>
            
        </v-col>

      </v-row>
    
  </v-container>
</template>

<script>
  export default {
    name: 'ProductsAndOrders',
    props:['orderProductData','productsData', 'ordersData', 'usersData'],

    data(){
      return{
        isOrderReady: false,
        productsFound : false,
        orderHeaders:[
          { text: 'Order ID', value: 'id'},
        ],
        productHeaders:[
          { text: 'Product ID', value: 'product_id'},
          { text: 'Product name', value: 'product_name'},
          { text: 'Quantity', value: 'qty'},
          { text: 'Unit price', value: 'unit_price'},
        ],
        userDetails:{ id:null, name:null, email:null },
        productsList: [],
        orderTotalCost: null

      }
    },

    mounted(){
      this.getOrdersData()
    },

    methods:{
      getOrdersData(){
        this.ordersData.sort((a,b) => (parseInt(a.id) > parseInt(b.id)) ? 1 : ((parseInt(b.id) > parseInt(a.id)) ? -1 : 0))  
      },

      getOrderDetails(value) {
        this.productsList = []
        this.productsFound = false
        
        // Get user data
        this.userDetails.id = value.user_id
        this.usersData.forEach(element => {
          if (value.user_id == element.id){ 
            this.userDetails.name = element.firstName + " " + element.lastName 
            this.userDetails.email = element.email
          }
        });

        //  Get products data
        this.orderProductData.forEach(orderProductDataElement => {
          var product = {
            product_id: null,
            product_name: null,
            qty: null,
            unit_price: null
          }
          if(orderProductDataElement.order_id == value.id){
            //  Get product names
            this.productsData.forEach(productsDataElement => {
              if(orderProductDataElement.product_id == productsDataElement.id){
                product.product_name = productsDataElement.name
                product.unit_price = productsDataElement.price
              }
            });
            product.product_id = orderProductDataElement.product_id
            product.qty = orderProductDataElement.qty
            this.productsList.push(product)
          }
        });
        
        if(this.productsList.length > 0){
          this.productsFound = true
        }
        this.isOrderReady = true
        //  Calculates the total
        var total = 0
        this.productsList.forEach(element => {
          var productCost = element.qty * element.unit_price
          total+= productCost
        });

        // Create our number formatter.
        var formatter = new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD',});
        this.orderTotalCost = formatter.format(total);
       
      },


    }
  }
</script>

<style scoped>

.full-height{
  height: 100%
}

</style>

<script>
import { Bar } from "vue-chartjs";

export default {
  name: "ChartProducts",
  extends: Bar,

  props: ["mostSaleProducts"],
  data() {
    return {
      products:[],
      qtys:[]
    };
  },
  created(){
   this.showData();
  },
  mounted() {
    
    this.renderChart(
      {
        labels: this.products,
        datasets: [
          {
            label: "Sold products",
            backgroundColor: "#90CAF9",
            data: this.qtys,
          },
        ],
      },
      { responsive: true, maintainAspectRatio: false }
    );
  },

  methods: {
    showData() {
      this.mostSaleProducts.forEach((element) => {
        this.products.push(element.name);
        this.qtys.push(element.qty);
      });
    },
  },
};
</script>

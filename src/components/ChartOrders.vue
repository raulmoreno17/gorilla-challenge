<script>
import { Bar } from "vue-chartjs";

export default {
  name: "ChartOrders",
  extends: Bar,

  props: ["monthlyOrders"],
  data() {
    return {
      months:[],
      orders:[]
    };
  },
  created(){
   this.showData();
  },
  mounted() {
    
    this.renderChart(
      {
        labels: this.months,
        datasets: [
          {
            label: "Orders on this month",
            backgroundColor: "#90CAF9",
            data: this.orders,
          },
        ],
      },
      { responsive: true, maintainAspectRatio: false }
    );
  },

  methods: {
    showData() {
      this.monthlyOrders.forEach((element) => {
        this.months.push(element.month);
        this.orders.push(element.totalOrders);
      });
      // Adjust range of the chart y-axis
      this.orders.push("0");
    },
  },
};
</script>

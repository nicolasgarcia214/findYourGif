<script>
  import { afterUpdate } from "svelte";
  import { numberOfGifs, names } from "../../store/store.js";
  import Chart from "chart.js";

  let ctx;
  let myChart;
  function createChart() {
    ctx = document.getElementById("myChart");
    if (myChart) myChart.destroy();
    myChart = new Chart(ctx, {
      type: "bar",
      data: {
        labels: $names,
        datasets: [
          {
            label: "Number of results per category",
            data: $numberOfGifs,
            backgroundColor: "rgba(85, 78, 155, 0.6)",
            borderColor: "rgba(52, 48, 98, 0.7)",
            borderWidth: 2
          }
        ]
      },
      options: {
        legend: {
          labels: {
            fontColor: "white"
          }
        },
        scales: {
          yAxes: [
            {
              ticks: {
                beginAtZero: true
              }
            }
          ]
        }
      }
    });
  }

  afterUpdate(createChart);
</script>

<style>
  .Chart {
    background: rgba(255, 255, 255, 0.308);
    border: 4px solid #ffffff;
    -webkit-box-shadow: 0px 0px 20px -1px rgb(85, 78, 155);
    -moz-box-shadow: 0px 0px 20px -1px rgba(52, 48, 98, 1);
    box-shadow: 0px 0px 20px -1px rgba(52, 48, 98, 1);
    border-radius: 25px;
  }
  @media (max-width: 768px) {
    .Chart {
      display: none;
    }
  }
</style>

<div class="Chart">
  <canvas id="myChart" width="4" height="1" />
</div>

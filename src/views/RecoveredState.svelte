<script>
  export let statewise
  const Chart = require('chart.js')
  import { afterUpdate } from 'svelte'

  afterUpdate(async () => {
    let labels = []
    let data = []
    statewise.sort((a,b)=>{
      return (b.recovered/b.confirmed - a.recovered/a.confirmed)
    })
    for(let s in statewise.slice(0,5)){
      if (statewise[s].state === "Andaman and Nicobar Islands"){
        labels.push("Andaman & N")
      }else{
        labels.push(statewise[s].state)
      }
      data.push(statewise[s].recovered*100/statewise[s].confirmed)
    }
    let ctx = document.getElementById('recovereChart').getContext('2d')
    let recovereChart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: labels,
        datasets: [{
          label: 'No of Cases',
          data: data,
          backgroundColor: [
            '#2e7d32',
            '#558b2f',
            '#43a047',
            '#8bc34a',
            '#aed581',
            '#c5e1a5'
          ],
        }]
      },
      options: {
        title: {
          display: true,
          text: 'Most recovered state in India',
          fontSize: 25
        },
        legend: {
          display: false,
          labels: {
            fontColor: '#000'
          }
        },
        scales: {
          yAxes: [{
            ticks: {
              beginAtZero: true
            },
            scaleLabel: {
              display: true,
              labelString: 'No. of cases'
            }
          }],
          xAxes: [{
            scaleLabel: {
              display: true,
              labelString: 'States'
            }
          }]
        }
      }
    })
  })
</script>
{#if statewise}
<canvas id="recovereChart" class="w-100" height="500"></canvas>
{/if}

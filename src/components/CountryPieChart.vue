<template>
    <div class="card mb-4 mt-4 shadow p-3 mb-5 rounded" style="width: 18rem; height: 27rem" >
      <div class="card-body">
        <highcharts
          :options="chartOptions"
        ></highcharts>
        <span class="badge " style="font-size:1em; background-color: rgb(124, 181, 236); color: #ffffff"> <strong>Active</strong></span> <br><br>
        <span class="badge badge-success" style="font-size:1em"> <strong>Recovered</strong></span> <br><br>
        <span class="badge badge-dark" style="font-size:1em"> <strong>Death</strong></span>
      </div>
    </div>
</template>

<script>
export default {
  name: "CountryPieChart",
  props:['country'],
  created() {
    //Did that because the chart take an array like [{name: 'test', y: '10'}, {name: 'test1', y: '35'}, ............., ]
    this.newCountry.push({
      name: "Active",
      y:
        this.country["value"] -
        this.country["TotalDeaths"] -
        this.country["TotalRecovered"]
    });
    this.newCountry.push({ name: "Deaths", y: this.country["TotalDeaths"] });
    this.newCountry.push({
      name: "Recovered",
      y: this.country["TotalRecovered"]
    });
  },
  computed: {
    chartOptions() {
      return {
        colors: ['#7cb5e8', '#343a40', '#28a745'],
        chart: {
          map: "countryPieChart",
          type: "pie",
          height: 100+ '%',
          marginLeft: 0,
          marginRight: 0,
          plotBorderWidth: 0,
          plotShadow: false,
          backgroundColor: "#0f4c75",
          plotBackgroundColor: '#0f4c75',
          borderWidth:0
        },
        title: {
          text: "All Cases Diagram"
        },
        tooltip: {
          pointFormat: "{series.name}: <b>{point.percentage:.1f}%</b>"
        },
        accessibility: {
          point: {
            valueSuffix: "%"
          }
        },
        credits: {
          enabled: false
        },
        plotOptions: {
          pie: {
            allowPointSelect: true,
            cursor: "pointer",
            dataLabels: {
              enabled: false,
              format: "<b>{point.name}</b>: {point.percentage:.1f} %"
            }
          }
        },
        series: [
          {
            name: "percentage",
            colorByPoint: true,
            data: this.newCountry
          }
        ]
      };
    }
  },
  data() {
    return {
      newCountry: []
    };
  }
};
</script>

<style scoped lang="scss">
  @import "../style";
  .card{
    background-color: $background-color-secondary;
    transition: transform 0.2s;
  }
  .card:hover {
    transform: scale(1.05);
  }
</style>

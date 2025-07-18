<template>
  <ApexChart
    type="bar"
    :height="height"
    :options="chartOptions"
    :series="dataSource.series"
  />
</template>

<script>
import ApexChart from 'vue-apexcharts';

export default {
  name: 'PositiveNegativeChart',
  components: {
    ApexChart,
  },
  props: {
    height: {
      type: String,
      default: '100%',
    },
    dataSource: {
      type: Object,
      required: true,
      validator: (data) => 'categories' in data && 'series' in data,
    },
  },
  computed: {
    chartOptions() {
      return {
        chart: {
          type: 'bar',
          height: this.height,
          fontFamily: 'Montserrat-Regular',
          toolbar: {
            show: false,
          },
        },
        plotOptions: {
          bar: {
            colors: {
              ranges: [
                {
                  from: -100,
                  to: 0,
                  color: '#f44336',
                },
              ],
            },
            columnWidth: '80%',
            dataLabels: {
              position: 'top',
            },
          },
        },
        dataLabels: {
          enabled: true,
          offsetY: -15,
          formatter: (val) => val.toFixed(1),
          style: {
            fontSize: '10px',
            colors: ['#ffffff'],
          },
        },
        grid: {
          show: false,
        },
        yaxis: {
          axisBorder: {
            show: true,
            color: '#4b3f3f',
          },
          axisTicks: {
            show: true,
            color: '#4b3f3f',
          },
          labels: {
            formatter: (y) => y.toFixed(1),
            style: {
              colors: '#ffffff',
              fontSize: '11px',
            },
          },
          title: {
            text: this.dataSource.unit,
            style: {
              color: '#ffffff',
              fontSize: '12px',
            },
          },
        },
        xaxis: {
          categories: this.dataSource.categories,
          labels: {
            formatter: (val) => val,
            style: {
              colors: '#ffffff',
              fontSize: '10px',
            },
          },
          axisBorder: {
            show: true,
            color: '#4b3f3f',
          },
          axisTicks: {
            show: true,
            color: '#4b3f3f',
          },
        },
        tooltip: {
          shared: false,
          intersect: true,
          style: {
            fontSize: '10px',
          },
          custom: function ({ series, seriesIndex, dataPointIndex, w }) {
            const category = w.config.xaxis.categories[dataPointIndex];
            const value = series[seriesIndex][dataPointIndex];
            let color;
            if (value < 0) {
              color = '#f44336'; 
            } else {
              color = '#43a047'; 
            }
            return `
            <div style="padding: 5px; background: ${color}; color: #fff; border-radius: 0; font-size: 10px; border: none;">
            <strong>${category}:</strong> ${value}
            </div>
            `;
          }
        },
        colors: ['#43a047', '#f44336'],
      };
    },
  },
};
</script>
<template>
  <div>
    <canvas ref="canvasRef" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import { Chart } from 'chart.js'
// import chartjsPluginAnnotation from 'chartjs-plugin-annotation'

export default defineComponent({
  props: ['calculationResult', 'target'],
  setup () {
    const canvasRef = ref<HTMLCanvasElement | null>(null)
    // ラベル生成器
    function getlabels () {
      const arr: string[] = []
      for (let i = 0; i < 50; i++) {
        arr.push(i + '年目')
      }
      return arr
    }
    function createCharts () {
      if (canvasRef.value === null) return
      const canvas = canvasRef.value.getContext('2d')
      if (canvas === null) return
      const c = new Chart(canvas, {
        // plugins: [chartjsPluginAnnotation],
        type: 'bar',
        data: {
          labels: getlabels(),
          datasets: [{
            label: 'シミュレーション結果',
            data: this.calculationResult
          }]
        },
        options: {
          scales: {
          yAxes: [{
              ticks: {
                beginAtZero: true,
                min: 0,
              },
              id: "y-axis-1",
              type: "linear",
            }
          ]},
        //   plugins: {
        //   annotation: {
        //     annotations: [
        //       {
        //         type: "line",
        //         scaleID: "y-axis-1",
        //         mode: "horizontal",
        //         value: 2000,// ここで更新してほしい。
        //         borderColor: "black",
        //         borderWidth: 2,
        //         label: {
        //           enabled: true,
        //           content: "目標",
        //         },
        //       },
        //     ],
        //   },
        // }
        }
      })
    }
    return {
      canvasRef,
      createCharts
    }
  },
})
</script>

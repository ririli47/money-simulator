<template>
  <div>
    <v-form
      ref="form"
    >
      <v-label>今現在の情報</v-label>
      <v-text-field
        v-model="totalMoney"
        label="総資産額"
        required
        type="number"
        min="0"
      ></v-text-field>

      <v-text-field
        v-model="defanseMoney"
        label="生活防衛資金"
        required
        type="number"
        min="0"
      ></v-text-field>

      <v-label>投資計画の情報</v-label>
      <v-text-field
        v-model="investmentInterestRate"
        label="想定投資利率/年"
        required
        type="number"
        min="0"
      ></v-text-field>

      <v-text-field
        v-model="additionalMoney"
        label="追加入金/月"
        required
        type="number"
        min="0"
      ></v-text-field>
      <p>年換算：{{ additionalMoneyPerYear == undefined ? '0' : additionalMoneyPerYear}} 円</p>

      <v-label>FIREプランの情報</v-label>
      <v-text-field
        v-model="costOfLiving"
        label="基準年間生活費"
        required
        type="number"
        min="0"
      ></v-text-field>

      <v-text-field
        v-model="useMoneyRate"
        label="年切り崩し（または配当）率"
        required
        type="number"
        min="0"
      ></v-text-field>
      <p>FIRE達成目標金額：{{ targetCompleteFire == undefined ? '0' : targetCompleteFire}} 円</p>


      <v-btn
        color="error"
        class="mr-4"
        @click="reset"
      >
        Reset Form
      </v-btn>
      <v-btn
        color="info"
        class="mr-4"
        @click="createCharts()"
      >
        グラフ描画
      </v-btn>
    </v-form>
    <BarChart ref="barChartRef" :calculationResult="calculationResult" :target="targetCompleteFire"/>
  </div>
</template>

<script setup lang="ts">
  import BarChart from '@/components/BarChart.vue'
  const barChartRef = ref();

  // data
  let totalMoney = ref<number>(0);
  let defanseMoney = ref<number>(0);
  let costOfLiving = ref<number>(0);
  let useMoneyRate = ref<number>(0);
  let investmentInterestRate = ref<number>(0);
  let additionalMoney = ref<number>(0);
  const additionalMoneyPerYear = computed<number>(() => additionalMoney.value * 12)

  // main logic
  let calculationResult = ref();
  calculationResult = computed(() => {
    let result = [0];
      result[0] = totalMoney.value; // 0年目
      let nextVal = totalMoney.value - defanseMoney.value; // 0年目      
      for (let i=1; i<50; i++) {
        // 次年度額 = (総資産額-生活防衛資金) *(1+利率) + 追加投資額
        nextVal = nextVal * ((100 + investmentInterestRate.value) / 100) + additionalMoneyPerYear.value
        result.push(parseInt((defanseMoney.value + nextVal).toFixed()))
      }
    return result;
  })
  let targetCompleteFire = computed(() => {
    return useMoneyRate.value == 0 ? '-' : (costOfLiving.value / ((useMoneyRate.value)/100))
  })
  
  // methods
  const reset = () => {
    totalMoney.value = 0
    defanseMoney.value = 0
    costOfLiving.value = 0
    investmentInterestRate.value = 0
    additionalMoney.value = 0
  }
  const createCharts = () => {
    barChartRef.value.createCharts();
  }
</script>
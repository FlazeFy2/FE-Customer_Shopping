<script setup>
  import WelcomeItem from './WelcomeItem.vue'
  import DocumentationIcon from './icons/IconDocumentation.vue'
  import { ref, onMounted } from "vue"
  import { readCsv } from "@/utils/data_prepare"
  import { countWordFrequencies } from "@/utils/analyze"
  import O_PieChartComponent from "@/components/organisms/O_PieChartComponent.vue"

  const labels_gender_comparison = ref([])
  const series_gender_comparison = ref([])
  const labels_most_item_purchased = ref([])
  const series_most_item_purchased = ref([])

  onMounted(async () => {
    const filePath = "/src/assets/shopping_trends.csv" 
    try {
      const data_gender_raw = await readCsv(filePath,['Gender'])
      const data_item_purchased = await readCsv(filePath,['Item Purchased'])

      // Exploratory Data Analysis (EDA) - Pie Chart Gender Comparison
      const gender_comparison = countWordFrequencies(data_gender_raw['Gender'])
      labels_gender_comparison.value = Object.keys(gender_comparison)
      series_gender_comparison.value = Object.values(gender_comparison)

      // Exploratory Data Analysis (EDA) - Pie Chart 7 Most Item Purchased
      const most_item_purchased = countWordFrequencies(data_item_purchased['Item Purchased'],7)
      labels_most_item_purchased.value = Object.keys(most_item_purchased)
      series_most_item_purchased.value = Object.values(most_item_purchased)

    } catch (error) {
      console.error("Failed to load CSV:", error)
    }
  })
</script>

<template>
  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>

    <!-- Exploratory Data Analysis (EDA) - Pie Chart Gender Comparison -->
    <O_PieChartComponent 
      :series="series_gender_comparison" 
      :labels="labels_gender_comparison" 
      second_title="Gender Comparison" 
      content="This compare total customer purchase by its gender"
    /><br><hr><br>

    <!-- Exploratory Data Analysis (EDA) - Pie Chart Most 7 Item Purchased -->
    <O_PieChartComponent 
      :series="series_most_item_purchased" 
      :labels="labels_most_item_purchased" 
      second_title="7 Most Item Purchased" 
      content="This show 7 most purchased item on the market"
    />

  </WelcomeItem>
</template>



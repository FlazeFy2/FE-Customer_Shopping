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
  const labels_most_category = ref([])
  const series_most_category = ref([])
  const labels_most_color = ref([])
  const series_most_color = ref([])
  const labels_most_location = ref([])
  const series_most_location = ref([])
  const labels_season_comparison = ref([])
  const series_season_comparison = ref([])

  onMounted(async () => {
    const filePath = "/src/assets/shopping_trends.csv" 
    try {
      const data_gender_raw = await readCsv(filePath,['Gender'])
      const data_item_purchased = await readCsv(filePath,['Item Purchased'])
      const data_category = await readCsv(filePath,['Category'])
      const data_location = await readCsv(filePath,['Location'])
      const data_color = await readCsv(filePath,['Color'])
      const data_season = await readCsv(filePath,['Season'])

      // Exploratory Data Analysis (EDA) - Pie Chart Gender Comparison
      const gender_comparison = countWordFrequencies(data_gender_raw['Gender'])
      labels_gender_comparison.value = Object.keys(gender_comparison)
      series_gender_comparison.value = Object.values(gender_comparison)

      // Exploratory Data Analysis (EDA) - Pie Chart 7 Most Item Purchased
      const most_item_purchased = countWordFrequencies(data_item_purchased['Item Purchased'],7)
      labels_most_item_purchased.value = Object.keys(most_item_purchased)
      series_most_item_purchased.value = Object.values(most_item_purchased)

      // Exploratory Data Analysis (EDA) - Pie Chart 5 Most Category
      const most_category = countWordFrequencies(data_category['Category'],5)
      labels_most_category.value = Object.keys(most_category)
      series_most_category.value = Object.values(most_category)

      // Exploratory Data Analysis (EDA) - Pie Chart 7 Most Location
      const most_location = countWordFrequencies(data_location['Location'],7)
      labels_most_location.value = Object.keys(most_location)
      series_most_location.value = Object.values(most_location)

      // Exploratory Data Analysis (EDA) - Pie Chart 7 Most Color
      const most_color = countWordFrequencies(data_color['Color'],7)
      labels_most_color.value = Object.keys(most_color)
      series_most_color.value = Object.values(most_color)

      // Exploratory Data Analysis (EDA) - Pie Chart Season Comparison
      const season_comparison = countWordFrequencies(data_season['Season'])
      labels_season_comparison.value = Object.keys(season_comparison)
      series_season_comparison.value = Object.values(season_comparison)
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
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Most 7 Item Purchased -->
    <O_PieChartComponent 
      :series="series_most_item_purchased" 
      :labels="labels_most_item_purchased" 
      second_title="7 Most Item Purchased" 
      content="This show 7 most purchased item on the market"
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Most 5 Category -->
    <O_PieChartComponent 
      :series="series_most_category" 
      :labels="labels_most_category" 
      second_title="7 Most Category" 
      content="This show 5 most category for purchased item on the market"
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Most 7 Location -->
    <O_PieChartComponent 
      :series="series_most_location" 
      :labels="labels_most_location" 
      second_title="7 Most Location" 
      content="This show 7 most location for purchased item on the market"
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Most 7 Color -->
    <O_PieChartComponent 
      :series="series_most_color" 
      :labels="labels_most_color" 
      second_title="7 Most Color" 
      content="This show 7 most color for purchased item on the market"
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Season Comparison -->
    <O_PieChartComponent 
      :series="series_season_comparison" 
      :labels="labels_season_comparison" 
      second_title="Season Comparison" 
      content="This compare total customer purchase by its season"
    />
  </WelcomeItem>
</template>



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
  const labels_payment_method_comparison = ref([])
  const series_payment_method_comparison = ref([])
  const labels_shipping_type_comparison = ref([])
  const series_shipping_type_comparison = ref([])
  const labels_discount_applied_comparison = ref([])
  const series_discount_applied_comparison = ref([])
  const labels_frequency_of_purchases_comparison = ref([])
  const series_frequency_of_purchases_comparison = ref([])

  onMounted(async () => {
    const filePath = "/src/assets/shopping_trends.csv" 
    try {
      const data_gender_raw = await readCsv(filePath,['Gender'])
      const data_item_purchased = await readCsv(filePath,['Item Purchased'])
      const data_category = await readCsv(filePath,['Category'])
      const data_location = await readCsv(filePath,['Location'])
      const data_color = await readCsv(filePath,['Color'])
      const data_season = await readCsv(filePath,['Season'])
      const data_payment_method = await readCsv(filePath,['Payment Method'])
      const data_shipping_type = await readCsv(filePath,['Shipping Type'])
      const data_discount_applied = await readCsv(filePath,['Discount Applied'])
      const data_frequency_of_purchases = await readCsv(filePath,['Frequency of Purchases'])

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

      // Exploratory Data Analysis (EDA) - Pie Chart Payment Method Comparison
      const payment_method_comparison = countWordFrequencies(data_payment_method['Payment Method'])
      labels_payment_method_comparison.value = Object.keys(payment_method_comparison)
      series_payment_method_comparison.value = Object.values(payment_method_comparison)

      // Exploratory Data Analysis (EDA) - Pie Chart Shipping Type Comparison
      const shipping_type_comparison = countWordFrequencies(data_shipping_type['Shipping Type'])
      labels_shipping_type_comparison.value = Object.keys(shipping_type_comparison)
      series_shipping_type_comparison.value = Object.values(shipping_type_comparison)

      // Exploratory Data Analysis (EDA) - Pie Chart Discount Applied Comparison
      const discount_applied_comparison = countWordFrequencies(data_discount_applied['Discount Applied'])
      labels_discount_applied_comparison.value = Object.keys(discount_applied_comparison)
      series_discount_applied_comparison.value = Object.values(discount_applied_comparison)

      // Exploratory Data Analysis (EDA) - Pie Chart Frequency of Purchases
      const frequency_of_purchases_comparison = countWordFrequencies(data_frequency_of_purchases['Frequency of Purchases'])
      labels_frequency_of_purchases_comparison.value = Object.keys(frequency_of_purchases_comparison)
      series_frequency_of_purchases_comparison.value = Object.values(frequency_of_purchases_comparison)
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

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Payment Method Comparison -->
    <O_PieChartComponent 
      :series="series_payment_method_comparison" 
      :labels="labels_payment_method_comparison" 
      second_title="Payment Method Comparison" 
      content="This compare total customer purchase by its Payment Method"
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Shipping Type Comparison -->
    <O_PieChartComponent 
      :series="series_shipping_type_comparison" 
      :labels="labels_shipping_type_comparison" 
      second_title="Shipping Type Comparison" 
      content="This compare total customer purchase by its Shipping Type"
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Discount Applied Comparison -->
    <O_PieChartComponent 
      :series="series_discount_applied_comparison" 
      :labels="labels_discount_applied_comparison" 
      second_title="Discount Applied Comparison" 
      content="This compare total customer purchase by its Discount Applied"
    />
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <!-- Exploratory Data Analysis (EDA) - Pie Chart Frequency of Purchases Comparison -->
    <O_PieChartComponent 
      :series="series_frequency_of_purchases_comparison" 
      :labels="labels_frequency_of_purchases_comparison" 
      second_title="Frequency of Purchases Comparison" 
      content="This compare total customer purchase by its Frequency of Purchases"
    />
  </WelcomeItem>
</template>



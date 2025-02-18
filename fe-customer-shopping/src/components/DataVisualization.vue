<script setup>
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
</script>

<template>
  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <O_PieChartComponent 
      :series="[44, 55]" 
      :labels="['Team A', 'Team B']" 
      second_title="Gender Comparison" 
      content="This compare total customer purchase by its gender"
    />
  </WelcomeItem>
</template>

<script>
  import O_PieChartComponent from "@/components/organisms/O_PieChartComponent.vue"
  export default {
    components: {
      O_PieChartComponent
    }
  }
</script>
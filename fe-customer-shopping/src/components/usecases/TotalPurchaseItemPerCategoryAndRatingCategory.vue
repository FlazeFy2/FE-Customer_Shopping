<template>
    <O_ColumnChartComponent 
        :series="chartData" 
        :labels="chartLabels" 
        second_title="Total Purchase Item and Category per Rating Category" 
        content="This chart shows the total purchase item group by its rating category"
    />
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import O_ColumnChartComponent from "../organisms/O_ColumnChartComponent.vue"
    import { readCsv } from "@/utils/data_prepare"

    const chartData = ref([])
    const chartLabels = ref(['Low', 'Average', 'Good'])

    onMounted(async () => {
        const filePath = "/src/assets/shopping_trends.csv"
        try {
            const data_raw = await readCsv(filePath, ['Review Rating','Category'])
            data_raw['Rating Category'] = data_raw['Review Rating'].map(dt => {
                if(dt > 4.0){
                    return "Good"
                } else if(dt > 3.0){
                    return "Average"
                } else {
                    return "Low"
                }
            })

            const groupedData = {}

            data_raw['Category'].forEach((category, index) => {
                const rating = data_raw['Rating Category'][index]

                if (!groupedData[category]) {
                    groupedData[category] = { "Low": 0, "Average": 0, "Good": 0 }
                }
                if (rating in groupedData[category]) {
                    groupedData[category][rating] += 1 
                }
            })

            chartData.value = Object.entries(groupedData).map(([category, ratingData]) => ({
                name: category,
                data: chartLabels.value.map(rating => ratingData[rating] || 0)
            }))
        } catch (error) {
            console.error("Failed to load CSV:", error)
        }
    })
</script>

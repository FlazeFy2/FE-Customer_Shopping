<template>
    <O_ColumnChartComponent 
        :series="chartData" 
        :labels="chartLabels" 
        second_title="Total Purchase Item and Category per Age Group" 
        content="This chart shows the total purchase item group by its age group"
    />
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import O_ColumnChartComponent from "../organisms/O_ColumnChartComponent.vue"
    import { readCsv } from "@/utils/data_prepare"

    const chartData = ref([])
    const chartLabels = ref(['Teen', 'Adult', 'Middle-Aged', 'Elder'])

    onMounted(async () => {
        const filePath = "/src/assets/shopping_trends.csv"
        try {
            const data_raw = await readCsv(filePath, ['Age','Category'])
            data_raw['Age Group'] = data_raw['Age'].map(dt => {
                if(dt > 65.0){
                    return "Elder"
                } else if(dt > 50.0){
                    return "Middle-Aged"
                } else if(dt > 19.0){
                    return "Adult"
                } else {
                    return "Teen"
                }
            })

            const groupedData = {}

            data_raw['Category'].forEach((category, index) => {
                const age = data_raw['Age Group'][index]

                if (!groupedData[category]) {
                    groupedData[category] = { "Teen": 0, "Adult": 0, "Middle-Aged": 0, "Elder": 0 }
                }
                if (age in groupedData[category]) {
                    groupedData[category][age] += 1 
                }
            })

            chartData.value = Object.entries(groupedData).map(([category, ageData]) => ({
                name: category,
                data: chartLabels.value.map(age => ageData[age] || 0)
            }))
        } catch (error) {
            console.error("Failed to load CSV:", error)
        }
    })
</script>

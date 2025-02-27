<template>
    <O_ColumnChartComponent 
        :series="chartData" 
        :labels="chartLabels" 
        second_title="Total Purchase Item and Category per Region" 
        content="This chart shows the total purchase item group by its region"
    />
</template>

<script setup>
    import { ref, onMounted } from "vue"
    import O_ColumnChartComponent from "../organisms/O_ColumnChartComponent.vue"
    import { readCsv } from "@/utils/data_prepare"

    const chartData = ref([])
    const regions = {
        "West": ["Alaska", "California", "Hawaii", "Oregon", "Washington", "Nevada", "Idaho", "Montana", "Wyoming", "Utah", "Colorado","Arizona","New Mexico"],
        "Midwest": ["Illinois", "Indiana", "Iowa", "Kansas", "Michigan", "Minnesota", "Missouri", "Nebraska", "North Dakota", "Ohio", "South Dakota", "Wisconsin"],
        "South": ["Alabama", "Arkansas", "Florida", "Georgia", "Kentucky", "Louisiana", "Mississippi", "North Carolina", "Oklahoma", "South Carolina", "Tennessee", "Texas", "Virginia", "West Virginia"],
        "Northeast": ["Connecticut", "Delaware", "Maine", "Maryland", "Massachusetts", "New Hampshire", "New Jersey", "New York", "Pennsylvania", "Rhode Island", "Vermont"]
    }
    const chartLabels = ref(["West", "Midwest", "South", "Northeast"])

    onMounted(async () => {
        const filePath = "/src/assets/shopping_trends.csv"
        try {
            const data_raw = await readCsv(filePath, ['Location','Category'])
            data_raw['Region'] = data_raw['Location'].map(dt => {
                for (const [region, state] of Object.entries(regions)){
                    if(state.includes(dt)){
                        return region
                    }
                }
                return "Unknown"
            })

            const groupedData = {}

            data_raw['Category'].forEach((category, index) => {
                const region = data_raw['Region'][index]

                if (!groupedData[category]) {
                    groupedData[category] = { "West": 0, "Midwest": 0, "South": 0, "Northeast": 0 }
                }
                if (region in groupedData[category]) {
                    groupedData[category][region] += 1 
                }
            })

            chartData.value = Object.entries(groupedData).map(([category, regionData]) => ({
                name: category,
                data: chartLabels.value.map(region => regionData[region] || 0)
            }))
        } catch (error) {
            console.error("Failed to load CSV:", error)
        }
    })
</script>

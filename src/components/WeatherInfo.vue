<script setup>
defineProps({
    weatherDetails: Object,
});
</script>
<template>
    <v-card
        class="mx-auto"
    >
        <v-card-item :title="weatherDetails.title" />

        <v-card-text class="py-0">
        <v-row align="center" no-gutters>
            <v-col
                class="text-h2"
                cols="8"
            >
                {{ weatherDetails.temp }}&deg;C
            </v-col>

            <v-col cols="4" class="text-right">
            <v-img
                :width="88"
                :src="weatherDetails.icon"
            ></v-img>
            </v-col>
        </v-row>
        </v-card-text>

        <div class="d-flex py-3 justify-space-between">
        <v-list-item
            density="compact"
            prepend-icon="mdi-weather-windy"
        >
            <v-list-item-subtitle>{{ weatherDetails.wind }} km/h</v-list-item-subtitle>
        </v-list-item>

        <v-list-item
            density="compact"
            prepend-icon="mdi-water-percent"
        >
            <v-list-item-subtitle>{{ weatherDetails.humidity }}%</v-list-item-subtitle>
        </v-list-item>
        </div>
    </v-card>
</template>
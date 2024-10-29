<template>
   
   <div class="main-wrapper">
        <div class="main-container">
            <Navbar></Navbar>
            <div class="hero-section">
          
                <div class="hero-content">
                    <h1>Welcome to my Calendar </h1>
                    <span
                        >
                        <a href="https://www.facebook.com/jorge.peenagos/" target="_blank" class="doc-text"
                            >  FB : Jorge Ramirez Penagos.</a
                        >
                        
                    </span >
                    
              
                </div>
                
            </div>

            <div class="grid-section">
         
    
                <div class="card-slider">
        <div class="calendar-button-container">
            <Button type="button" label="Calendar" icon="pi pi-calendar"  badgeSeverity="contrast" outlined />
            <Calendar v-model="dateRange" placeholder="Select date" :selectionMode="'range'" @change="resetSliderValue">
                <template #footer>
                    <div class="slider-container">
                        <label for="days">Days of stay:</label>
                        <br>
                        <Slider v-model="sliderValue" :min="minValue" :max="maxValue" :step="step" :valueFormat="valueFormat" @change="onSliderChange" class="custom-slider" />
                        <br>
                        <InputText v-model="sliderValue" readonly />
                        <br>
                    </div>

                    <div class="button-container">
                        <Button type="button" label="5 Days" @click="setDays(5)" outlined />
                        <Button type="button" label="10 Days" @click="setDays(10)" outlined />
                        <Button type="button" label="30 Days" @click="setDays(30)" outlined />
                    </div>

                    <div class="action-buttons">
                        <Button type="button" label="Save" icon="pi pi-save" @click="saveDateRange" outlined />
                        <Button type="button" label="Clear" icon="pi pi-times" @click="clearSelection" outlined />
                    </div>
                </template>
            </Calendar>
            <div class="formatted-date-container">
                <label for="formattedDate">Selected Date Range:</label>
                <InputText v-model="formattedDateRange" readonly />
            </div>
        </div>
    </div>
        

            </div>
        </div>

    </div>
   


   

    
</template>

<script setup>
import { ref, computed } from "vue";
import Button from 'primevue/button';
import Calendar from 'primevue/calendar';
import Slider from 'primevue/slider';
import InputText from 'primevue/inputtext';
import Navbar from "./components/Navbar.vue";

const dateRange = ref(null);
const sliderValue = ref(1); 
const minValue = 1;       
const maxValue = 30;      
const step = 1;            


const formattedDateRange = computed(() => {
    if (dateRange.value && dateRange.value[0] && dateRange.value[1]) {
        const format = (date) => {
            const year = date.getFullYear();
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const day = String(date.getDate()).padStart(2, '0');
            return `${year}/${month}/${day}`;
        };
        return `${format(dateRange.value[0])} - ${format(dateRange.value[1])}`;
    }
    return "";
});

function setDays(days) {
    sliderValue.value = days;
    updateDateRange();
}

function updateDateRange() {
    if (dateRange.value && dateRange.value[0]) {
        const startDate = new Date(dateRange.value[0]);
        const endDate = new Date(startDate);
        endDate.setDate(startDate.getDate() + sliderValue.value - 1);
        dateRange.value = [startDate, endDate];
    }
}

function onSliderChange() {
    updateDateRange();
}

function resetSliderValue() {
    sliderValue.value = 1;
}

// Guarda el rango de fechas actual
function saveDateRange() {
    updateDateRange();
    console.log("Date range saved:", dateRange.value);
}

function clearSelection() {
    dateRange.value = null;
    sliderValue.value = 5;
}
</script>

<style scoped>
.card-slider {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 10px;
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
}

.calendar-button-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 1rem;
}

.slider-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 1rem;
    padding: 1rem;
    border-radius: 10px;
    width: 100%;
}

.custom-slider {
    width: 100%; 
}

.formatted-date-container {
    margin-top: 1rem;
    width: 100%;
    text-align: center;
}

.button-container {
    display: flex;
    justify-content: space-between;
    margin-top: 1rem;
}

.action-buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 1rem;
}
</style>

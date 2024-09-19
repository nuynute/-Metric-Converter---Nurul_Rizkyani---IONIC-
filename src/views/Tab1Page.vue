<template>
  <ion-page>
    <ion-content :fullscreen="true" class="content">
      <div class="header-wrapper">
        <ion-card class="header-card">
          <ion-card-header>
            <ion-card-title class="title">Metric Converter</ion-card-title>
            <ion-card-subtitle class="subtitle">By: Nurul Rizkyani</ion-card-subtitle>
          </ion-card-header>
        </ion-card>
      </div>

      <ion-card class="converter-card">
        <ion-card-content>
          <ion-item>
            <ion-label>Pilih Metrik</ion-label>
            <ion-select v-model="metricType">
              <ion-select-option value="length">Length</ion-select-option>
              <ion-select-option value="weight">Weight</ion-select-option>
              <ion-select-option value="temperature">Temperature</ion-select-option>
            </ion-select>
          </ion-item>

          <ion-item>
            <ion-label>Unit From</ion-label>
            <ion-select v-model="unitFrom">
              <ion-select-option v-for="unit in units[metricType]" :key="unit" :value="unit">{{ unit }}</ion-select-option>
            </ion-select>
          </ion-item>

          <ion-item>
            <ion-label>Unit To</ion-label>
            <ion-select v-model="unitTo">
              <ion-select-option v-for="unit in units[metricType]" :key="unit" :value="unit">{{ unit }}</ion-select-option>
            </ion-select>
          </ion-item>

          <ion-item>
            <ion-label>Value</ion-label>
            <ion-input v-model.number="inputValue" type="number" placeholder="Enter value"></ion-input>
          </ion-item>

          <div class="result">
            <h3>Hasil Konversi</h3>
            <p>{{ conversionResult }}</p>
          </div>
        </ion-card-content>
      </ion-card>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { IonPage, IonContent, IonItem, IonLabel, IonSelect, IonSelectOption, IonInput, IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent } from '@ionic/vue';

const metricType = ref<string | null>(null);
const unitFrom = ref<string | null>(null);
const unitTo = ref<string | null>(null);
const inputValue = ref<number | null>(null);

const units = {
  length: ['Meters', 'Kilometers', 'Miles'],
  weight: ['Grams', 'Kilograms', 'Pounds'],
  temperature: ['Celsius', 'Fahrenheit', 'Kelvin']
};

const conversionResult = computed(() => {
  if (inputValue.value == null || !unitFrom.value || !unitTo.value || !metricType.value) {
    return 'N/A';
  }

  let result: number;

  if (metricType.value === 'length') {
    // Length Conversion Logic
    if (unitFrom.value === 'Meters' && unitTo.value === 'Kilometers') {
      result = inputValue.value / 1000;
      return result.toFixed(2) + ' km';
    }
    if (unitFrom.value === 'Kilometers' && unitTo.value === 'Meters') {
      result = inputValue.value * 1000;
      return result.toFixed(2) + ' m';
    }
    if (unitFrom.value === 'Miles' && unitTo.value === 'Meters') {
      result = inputValue.value * 1609.34;
      return result.toFixed(2) + ' m';
    }
    if (unitFrom.value === 'Meters' && unitTo.value === 'Miles') {
      result = inputValue.value / 1609.34;
      return result.toFixed(2) + ' mi';
    }
    if (unitFrom.value === 'Kilometers' && unitTo.value === 'Miles') {
      result = inputValue.value * 0.621371;
      return result.toFixed(2) + ' mi';
    }
    if (unitFrom.value === 'Miles' && unitTo.value === 'Kilometers') {
      result = inputValue.value / 0.621371;
      return result.toFixed(2) + ' km';
    }
    return 'Length conversion not supported for these units';
  }

  if (metricType.value === 'weight') {
    // Weight Conversion Logic
    if (unitFrom.value === 'Grams' && unitTo.value === 'Kilograms') {
      result = inputValue.value / 1000;
      return result.toFixed(2) + ' kg';
    }
    if (unitFrom.value === 'Kilograms' && unitTo.value === 'Grams') {
      result = inputValue.value * 1000;
      return result.toFixed(2) + ' g';
    }
    if (unitFrom.value === 'Pounds' && unitTo.value === 'Grams') {
      result = inputValue.value * 453.592;
      return result.toFixed(2) + ' g';
    }
    if (unitFrom.value === 'Grams' && unitTo.value === 'Pounds') {
      result = inputValue.value / 453.592;
      return result.toFixed(2) + ' lb';
    }
    return 'Weight conversion not supported for these units';
  }

  if (metricType.value === 'temperature') {
    // Temperature Conversion Logic
    if (unitFrom.value === 'Celsius' && unitTo.value === 'Fahrenheit') {
      result = (inputValue.value * 9/5) + 32;
      return result.toFixed(2) + ' °F';
    }
    if (unitFrom.value === 'Fahrenheit' && unitTo.value === 'Celsius') {
      result = (inputValue.value - 32) * 5/9;
      return result.toFixed(2) + ' °C';
    }
    if (unitFrom.value === 'Celsius' && unitTo.value === 'Kelvin') {
      result = inputValue.value + 273.15;
      return result.toFixed(2) + ' K';
    }
    if (unitFrom.value === 'Kelvin' && unitTo.value === 'Celsius') {
      result = inputValue.value - 273.15;
      return result.toFixed(2) + ' °C';
    }
    return 'Temperature conversion not supported for these units';
  }

  return 'Conversion not supported';
});
</script>

<style scoped>
.content {
  padding: 16px;
  background: url('https://source.unsplash.com/random/1920x1080?abstract') no-repeat center center fixed;
  background-size: cover;
  display: flex;
  align-items: center;
  justify-content: center;
}

.header-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.header-card {
  background: rgba(255, 255, 255, 0.9); /* Slightly transparent white for the header card */
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  max-width: 400px; /* Adjusted max-width for header card */
  width: 100%;
}

.title {
  color: #9c27b0; /* Purple color for the title */
  font-weight: 500;
}

.subtitle {
  color: #7b1fa2; /* Darker purple for the subtitle */
}

.converter-card {
  background: rgba(255, 255, 255, 0.9); /* Slightly transparent white for the converter card */
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  max-width: 400px; /* Adjusted max-width for converter card */
  width: 100%;
  margin: 0 auto; /* Centering card horizontally */
}

.result {
  margin-top: 20px;
  text-align: center;
}

h3 {
  color: #9c27b0; /* Purple for result title */
}

p {
  font-size: 18px;
  font-weight: 500;
  color: #333;
}

ion-item {
  margin-bottom: 16px;
}

ion-label {
  color: #9c27b0; /* Purple color for labels */
  font-weight: 500;
}

ion-select, ion-input {
  --color: #9c27b0; /* Purple text color */
}

ion-input::part(native) {
  border-bottom: 1px solid #9c27b0; /* Purple bottom border for input */
}

ion-select::part(native) {
  border-bottom: 1px solid #9c27b0; /* Purple bottom border for select */
}

ion-select .select-icon {
  color: #9c27b0; /* Purple color for select icon */
}
</style>

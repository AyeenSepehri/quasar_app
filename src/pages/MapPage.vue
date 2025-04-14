<template>
  <q-page class="q-pa-md">
    <div class="text-h6 q-mb-md">مسیر حرکت ماشین در تهران</div>
    <div id="map" style="height: 500px;"></div>
  </q-page>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

// refs
const map = ref(null)
const marker = ref(null)
const currentStep = ref(0)

// ========== تنها بخش تغییر داده شده ==========
// مختصات دقیق میدان ولیعصر + ۱۰۰ متر فواصل شمالی
const path = (() => {
  const startLat = 35.7110; // میدان ولیعصر
  const startLon = 51.4073;
  const meterPerDegree = 0.0009009; // 100 متر معادل جغرافیایی
  const points = [];

  for (let i = 0; i < 10; i++) {
    points.push([
      startLat + (i * meterPerDegree),
      startLon
    ]);
  }
  return points;
})();
// ========== پایان تغییرات ==========

// آیکون ماشین
const carIcon = L.icon({
  iconUrl: '/icons/car-icon.svg',
  iconSize: [40, 40],
  iconAnchor: [20, 20],
  popupAnchor: [0, -20]
})

// حرکت (بدون تغییر)
const moveMapAndMarker = () => {
  const [lat, lng] = path[currentStep.value]

  map.value.setView([lat, lng], 16)
  if (marker.value) marker.value.setLatLng([lat, lng])

  currentStep.value = (currentStep.value + 1) % path.length
}

let interval = null

onMounted(() => {
  // ساخت نقشه با نقطه شروع اولیه
  map.value = L.map('map', {
    center: path[0], // استفاده از نقطه اول آرایه جدید
    zoom: 16,
    zoomControl: false
  })

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(map.value)

  // ساخت مارکر در نقطه شروع جدید
  marker.value = L.marker(path[0], { icon: carIcon }).addTo(map.value)

  interval = setInterval(moveMapAndMarker, 1000)
})

onUnmounted(() => clearInterval(interval))
</script>

<style scoped>
#map {
  width: 100%;
  border-radius: 8px;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
}
</style>

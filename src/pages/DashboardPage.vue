<template>
  <q-page class="q-pa-md">
    <q-btn
      color="primary"
      label="افزودن خودرو"
      icon="add"
      class="q-mb-md"
      @click="showAddDialog = true"
    />

    <q-table
      title="لیست خودروها"
      :rows="cars"
      :columns="columns"
      row-key="id"
      flat bordered
    >
      <template v-slot:body-cell-status="text">
        <q-td class="text-center">
          <span :class="text.row.status === 'روشن' ? 'text-green' : 'text-red'">{{text.row.status}}</span>
        </q-td>
      </template>
      <template v-slot:body-cell-actions="props">
        <q-td align="center">
          <q-btn
            color="primary"
            label="جزئیات"
            size="sm"
            @click="showDetails(props.row)"
          />
        </q-td>
      </template>
    </q-table>

    <q-dialog v-model="showDialog">
      <q-card style="min-width: 300px">
        <q-card-section class="q-pa-md">
          <div class="text-h6">جزئیات خودرو</div>
        </q-card-section>

        <q-card-section>
          <div>پلاک: {{ selectedCar?.plate }}</div>
          <div>مدل: {{ selectedCar?.model }}</div>
          <div>وضعیت: {{ selectedCar?.status }}</div>
          <div>مکان: {{ selectedCar?.location }}</div>
          <div>آخرین بروزرسانی: {{ selectedCar?.lastUpdate }}</div>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="بستن" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <AddCarDialog
      v-model="showAddDialog"
      @add="addCar"
    />


  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import AddCarDialog from 'components/modals/AddCarDialog.vue'

const showAddDialog = ref(false)
const showDialog = ref(false)
const selectedCar = ref(null)
const cars = ref([
  {
    id: 1,
    plate: '21 الف 123',
    model: 'پژو 206',
    status: 'روشن',
    location: 'تهران',
    lastUpdate: '۱۴۰۳/۰۱/۲۴ - ۱۴:۳۰',
  },
  {
    id: 2,
    plate: '48 ب 456',
    model: 'سمند',
    status: 'خاموش',
    location: 'اصفهان',
    lastUpdate: '۱۴۰۳/۰۱/۲۴ - ۱۲:۱۵',
  }
])

function addCar(newCar) {
  cars.value.push(newCar)
}

function showDetails(row) {
  selectedCar.value = row
  showDialog.value = true
}

const columns = [
  { name: 'plate', label: 'پلاک', field: 'plate', align: 'center' },
  { name: 'model', label: 'مدل خودرو', field: 'model', align: 'center' },
  { name: 'status', label: 'وضعیت', field: 'status', align: 'center' },
  { name: 'location', label: 'مکان', field: 'location', align: 'center' },
  { name: 'lastUpdate', label: 'آخرین بروزرسانی', field: 'lastUpdate', align: 'center' },
  { name: 'actions', label: 'عملیات', align: 'center' }
]

</script>

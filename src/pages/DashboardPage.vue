<template>
  <q-page class="q-pa-md">
    <q-btn
      color="primary"
      label="افزودن خودرو"
      icon="add"
      class="q-mb-md"
      @click="() => {
        showAddDialog = true
        isEdit = false
        selectedCar = null
      }"
    />

    <q-table
      title="لیست خودروها"
      :rows="cars"
      :columns="columns"
      row-key="id"
      flat
      bordered
    >
      <template v-slot:body-cell-status="text">
        <q-td class="text-center">
          <span :class="text.row.status === 'روشن' ? 'text-green' : 'text-red'">{{ text.row.status }}</span>
        </q-td>
      </template>

      <template v-slot:body-cell-actions="props">
        <q-td align="center">
          <q-btn
            color="primary"
            label="جزئیات"
            size="sm"
            @click="() => {
              showAddDialog = true
              isEdit = true
              showDetails(props.row)
            }"
          />
        </q-td>
      </template>
    </q-table>

    <AddCarDialog
      v-model="showAddDialog"
      @add="addCar"
      @update="updateCar"
      :isEditMode="isEdit"
      :initialValue="isEdit && selectedCar ? selectedCar : null"
    />
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import AddCarDialog from 'components/modals/AddCarDialog.vue'

const showAddDialog = ref(false)
const isEdit = ref(false)
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

function updateCar(updatedCar) {
  const index = cars.value.findIndex(c => c.id === updatedCar.id)
  if (index !== -1) {
    cars.value[index] = updatedCar
  }
}

function showDetails(row) {
  selectedCar.value = { ...row } // کپی مستقل برای ویرایش امن
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

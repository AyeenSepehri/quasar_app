<template>
  <q-dialog v-model="internalModelValue" persistent>
    <q-card style="min-width: 400px">
      <q-card-section>
        <div class="text-h6">افزودن خودرو جدید</div>
      </q-card-section>

      <q-card-section class="q-gutter-md">
        <q-input
          v-model="form.plate"
          label="پلاک"
          :error="!!errors.plate"
          :error-message="errors.plate"
        />
        <q-input
          v-model="form.model"
          label="مدل خودرو"
          :error="!!errors.model"
          :error-message="errors.model"
        />
        <q-select
          v-model="form.status"
          label="وضعیت"
          :options="['روشن', 'خاموش']"
          :error="!!errors.status"
          :error-message="errors.status"
        />
        <q-input
          v-model="form.location"
          label="مکان"
          :error="!!errors.location"
          :error-message="errors.location"
        />
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="انصراف" color="grey" @click="cancel" />
        <q-btn flat label="افزودن" color="primary" @click="submit" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useQuasar } from 'quasar'

const props = defineProps({
  modelValue: Boolean
})

const emit = defineEmits(['update:modelValue', 'add'])

const $q = useQuasar()

const form = ref({
  plate: '',
  model: '',
  status: '',
  location: ''
})

const errors = ref({
  plate: '',
  model: '',
  status: '',
  location: ''
})

const internalModelValue = computed({
  get: () => props.modelValue,
  set: (val) => emit('update:modelValue', val)
})

function validateForm() {
  errors.value = {
    plate: '',
    model: '',
    status: '',
    location: ''
  }

  let isValid = true

  // پلاک: حداقل ۵ حرف
  if (!form.value.plate || form.value.plate.length < 5) {
    errors.value.plate = 'پلاک باید حداقل ۵ کاراکتر باشد.'
    isValid = false
  }

  // مدل: اجباری
  if (!form.value.model) {
    errors.value.model = 'مدل خودرو الزامی است.'
    isValid = false
  }

  // وضعیت: اجباری
  if (!form.value.status) {
    errors.value.status = 'لطفاً وضعیت را انتخاب کنید.'
    isValid = false
  }

  // مکان: اجباری
  if (!form.value.location) {
    errors.value.location = 'مکان را وارد کنید.'
    isValid = false
  }

  return isValid
}

function submit() {
  if (validateForm()) {
    const newCar = {
      id: Date.now(),
      ...form.value,
      lastUpdate: new Date().toLocaleString('fa-IR')
    }
    emit('add', newCar)
    emit('update:modelValue', false)
    $q.notify({
      type: 'positive',
      message: 'خودرو با موفقیت اضافه شد'
    })
    reset()
  }
}

function cancel() {
  emit('update:modelValue', false)
  reset()
}

function reset() {
  form.value = {
    plate: '',
    model: '',
    status: '',
    location: ''
  }
  errors.value = {
    plate: '',
    model: '',
    status: '',
    location: ''
  }
}
</script>

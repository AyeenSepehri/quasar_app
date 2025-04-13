<template>
  <q-dialog v-model="internalModelValue" persistent>
    <q-card style="min-width: 400px">
      <q-card-section>
        <div v-if="isEditMode" class="text-h6">ویرایش خودرو</div>
        <div v-else class="text-h6">افزودن خودرو جدید</div>
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
        <q-btn
          flat
          :label="isEditMode ? 'ذخیره تغییرات' : 'افزودن'"
          color="primary"
          @click="submit"
        />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref, computed, watchEffect } from 'vue'
import { useQuasar } from 'quasar'

const props = defineProps({
  modelValue: Boolean,
  isEditMode: Boolean,
  initialValue: Object
})

const emit = defineEmits(['update:modelValue', 'add', 'update'])

const $q = useQuasar()

const form = ref({
  plate: '',
  model: '',
  status: '',
  location: ''
})

watchEffect(() => {
  if (props.modelValue) {
    if (props.isEditMode && props.initialValue) {
      form.value = {
        plate: props.initialValue.plate || '',
        model: props.initialValue.model || '',
        status: props.initialValue.status || '',
        location: props.initialValue.location || ''
      }
    } else {
      reset()  // در حالت افزودن فرم ریست شود
    }
  }
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

  if (!form.value.plate || form.value.plate.length < 5) {
    errors.value.plate = 'پلاک باید حداقل ۵ کاراکتر باشد.'
    isValid = false
  }

  if (!form.value.model) {
    errors.value.model = 'مدل خودرو الزامی است.'
    isValid = false
  }

  if (!form.value.status) {
    errors.value.status = 'لطفاً وضعیت را انتخاب کنید.'
    isValid = false
  }

  if (!form.value.location) {
    errors.value.location = 'مکان را وارد کنید.'
    isValid = false
  }

  return isValid
}

function submit() {
  if (validateForm()) {
    const car = {
      ...form.value,
      id: props.isEditMode ? props.initialValue.id : Date.now(),
      lastUpdate: new Date().toLocaleString('fa-IR')
    }

    if (props.isEditMode) {
      emit('update', car)
      $q.notify({
        type: 'positive',
        message: 'تغییرات خودرو ذخیره شد.'
      })
    } else {
      emit('add', car)
      $q.notify({
        type: 'positive',
        message: 'خودرو با موفقیت اضافه شد.'
      })
    }

    emit('update:modelValue', false)
    if (!props.isEditMode) reset()
  }
}

function cancel() {
  emit('update:modelValue', false)
  if (!props.isEditMode) reset()
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

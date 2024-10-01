<template>
  <v-card max-width="725" class="mx-auto">
    <v-card-title>Cadastro de Eventos</v-card-title>
    <v-card-text>
      <v-form>
        <v-text-field v-model="localEvent.name" prepend-inner-icon="mdi-calendar" variant="underlined" label="Nome do Evento" />
        <v-text-field v-model="localEvent.date" prepend-inner-icon="mdi-calendar-clock" variant="underlined" label="Data do Evento" type="date" />
        <v-text-field v-model="localEvent.location" prepend-inner-icon="mdi-map-marker" variant="underlined" label="Local do Evento" />
        <v-textarea v-model="localEvent.description" prepend-inner-icon="mdi-text" variant="underlined" label="Descrição do Evento" />
        <v-file-input v-model="localEvent.image" prepend-inner-icon="mdi-image" variant="underlined" label="Imagem do Evento" @change="previewImage" />
        <v-img v-if="imagePreview" :src="imagePreview" aspect-ratio="16/9" class="my-2"></v-img>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-btn color="error" @click="resetForm">Cancelar</v-btn>
      <v-spacer />
      <v-btn color="primary" :disabled="!isFormValid" @click="handleSaveEvent">Salvar</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  event: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['save-event'])

const localEvent = ref({ ...props.event, eventId: props.event.eventId || Date.now() })
const imagePreview = ref(null)

watch(props.event, (newEvent) => {
  localEvent.value = { ...newEvent, eventId: newEvent.eventId || Date.now() }
})

// Computed property to check if the form is valid
const isFormValid = computed(() => {
  return localEvent.value.name && localEvent.value.date && localEvent.value.location && localEvent.value.description
})

// Function to handle save event
const handleSaveEvent = () => {
  if (localEvent.value.image && typeof localEvent.value.image !== 'string') {
    const reader = new FileReader()
    reader.onload = (e) => {
      localEvent.value.image = e.target.result
      emit('save-event', { ...localEvent.value, eventId: localEvent.value.eventId })
      resetForm()
    }
    reader.readAsDataURL(localEvent.value.image)
  } else {
    emit('save-event', { ...localEvent.value, eventId: localEvent.value.eventId })
    resetForm()
  }
}

// Function to reset the form
const resetForm = () => {
  localEvent.value = {
    name: '',
    date: '',
    location: '',
    description: '',
    image: null,
    eventId: Date.now()
  }
  imagePreview.value = null
}

// Function to preview image
const previewImage = (event) => {
  const file = event.target.files[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (e) => {
      imagePreview.value = e.target.result
    }
    reader.readAsDataURL(file)
  } else {
    imagePreview.value = null
  }
}
</script>
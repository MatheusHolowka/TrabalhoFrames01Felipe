<template>
  <v-container>
    <EventForm :event="event" @save-event="saveEvent" />
  </v-container>
</template>

<script setup>
import { ref } from 'vue'
import EventForm from '../components/EventForm.vue';


const event = ref({
  name: '',
  date: '',
  location: '',
  description: '',
  image: null
})

// Function to save the event
const saveEvent = (newEvent) => {
  const existingEvents = JSON.parse(localStorage.getItem('events')) || []
  existingEvents.push(newEvent)
  localStorage.setItem('events', JSON.stringify(existingEvents))
  console.log('Evento salvo:', newEvent)

  resetForm()
}

// Function to reset the form
const resetForm = () => {
  event.value = {
    name: '',
    date: '',
    location: '',
    description: '',
    image: null
  }
}
</script>
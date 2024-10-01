<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="10" lg="8" class="mx-auto">
        <v-card>
          <v-card-title class="text-center">Eventos</v-card-title>
          <v-card-text>
            <v-list v-if="events.length">
              <v-list-item v-for="(event, index) in events" :key="index">
                <v-list-item-content>
                  <v-list-item-title>{{ event.name }}</v-list-item-title>
                  <v-list-item-subtitle>{{ event.date }}</v-list-item-subtitle>
                  <v-list-item-subtitle>{{ event.location }}</v-list-item-subtitle>
                  <v-list-item-subtitle>{{ event.description }}</v-list-item-subtitle>
                  <v-img v-if="event.image" :src="event.image" aspect-ratio="16/9" class="my-2"></v-img>
                </v-list-item-content>
                <v-list-item-action>
                  <!-- Chamando o método openParticipationForm passando o eventId -->
                  <v-btn color="primary" @click="openParticipationForm(event)">Participar</v-btn>
                </v-list-item-action>
              </v-list-item>
            </v-list>
            <div v-else class="text-center">
              <v-icon color="grey" size="64">mdi-emoticon-sad-outline</v-icon>
              <p>Nenhum evento cadastrado!</p>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Referência ao componente ParticipationForm com props -->
    <ParticipationForm ref="participationForm" :event="selectedEvent" />
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ParticipationForm from '../components/ParticipationForm.vue'

const events = ref([])
const selectedEvent = ref(null) // Para armazenar o evento selecionado

// Função para carregar eventos do localStorage
const loadEvents = () => {
  const storedEvents = JSON.parse(localStorage.getItem('events')) || []
  events.value = storedEvents
}

// Carregar eventos quando o componente for montado
onMounted(() => {
  loadEvents()
})

const participationForm = ref(null)

// Função para abrir o formulário de participação com o evento selecionado
const openParticipationForm = (event) => {
  selectedEvent.value = event // Armazena o evento selecionado
  participationForm.value.openDialog(event.eventId) // Passa o eventId
}
</script>

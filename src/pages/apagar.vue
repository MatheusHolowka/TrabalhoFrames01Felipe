<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="10" lg="8" class="mx-auto">
        <v-card>
          <v-card-title class="text-center">Eventos Cadastrados</v-card-title>
          <v-card-text>
            <v-list v-if="events.length">
              <v-list-item v-for="(event, index) in events" :key="event.eventId">
                <v-list-item-content>
                  <v-list-item-title>{{ event.name }}</v-list-item-title>
                  <v-list-item-subtitle>{{ event.date }}</v-list-item-subtitle>
                  <v-list-item-subtitle>{{ event.location }}</v-list-item-subtitle>
                  <v-list-item-subtitle>{{ event.description }}</v-list-item-subtitle>
                  <v-img
                    v-if="event.image"
                    :src="event.image"
                    aspect-ratio="16/9"
                    class="my-2"
                  ></v-img>
                </v-list-item-content>
                <v-list-item-action>
                  <!-- Passando o ID do evento em vez do índice -->
                  <v-btn color="primary" class="mr-2" @click="viewParticipants(event.eventId)">Visualizar Participantes</v-btn>
                  <v-btn color="error" @click="confirmDelete(event.eventId)">Apagar</v-btn>
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
    <ParticipantsDialog ref="participantsDialog" />
    <v-dialog v-model="confirmDialog" max-width="500">
      <v-card>
        <v-card-title class="headline">Confirmar Exclusão</v-card-title>
        <v-card-text>Você tem certeza que deseja apagar este evento?</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="cancelDelete">Cancelar</v-btn>
          <v-btn color="red darken-1" text @click="deleteEvent">Apagar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ParticipantsDialog from '../components/ParticipantsDialog.vue';

const events = ref([])
const confirmDialog = ref(false)
const eventIdToDelete = ref(null)

// Function to load events from local storage
const loadEvents = () => {
  const storedEvents = JSON.parse(localStorage.getItem('events')) || []
  events.value = storedEvents
}

// Load events when the component is mounted
onMounted(() => {
  loadEvents()
})

const participantsDialog = ref(null)

const viewParticipants = (eventId) => {
  participantsDialog.value.openDialog(eventId)
}

const confirmDelete = (eventId) => {
  eventIdToDelete.value = eventId
  confirmDialog.value = true
}

const cancelDelete = () => {
  eventIdToDelete.value = null
  confirmDialog.value = false
}

const deleteEvent = () => {
  events.value = events.value.filter(event => event.eventId !== eventIdToDelete.value)
  localStorage.setItem('events', JSON.stringify(events.value))
  confirmDialog.value = false
  eventIdToDelete.value = null
}
</script>
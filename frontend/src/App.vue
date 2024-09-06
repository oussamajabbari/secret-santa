<script setup>
import { ref } from 'vue'
import axios from 'axios'

const participants = ref([])
const newParticipant = ref('')
const draws = ref([])

// Test connection to backend
let r = axios.get('http://127.0.0.1:8000')
console.log(r)

function addParticipant() {
  const lastId =
    participants.value.length == 0 ? 0 : participants.value[participants.value.length - 1].id
  participants.value.push({
    id: lastId + 1,
    name: newParticipant.value
  })
  newParticipant.value = ''
}

function draw() {
  draws.value = []

  if (participants.value.length < 2) {
    console.log('need more participants !')
    return
  }

  let remainingGifters = participants.value.map((participant) => participant, participants)
  let remainingReceivers = participants.value.map((participant) => participant, participants)

  while (remainingGifters.length != 0) {
    let gifter = remainingGifters[Math.floor(Math.random() * remainingGifters.length)]
    let receiver = remainingReceivers[Math.floor(Math.random() * remainingReceivers.length)]

    if (remainingGifters.length > 1 && gifter.id == receiver.id) {
      console.log('same dudes !')
      continue
    }

    draws.value.push({
      id: gifter.id,
      gifter: gifter,
      receiver: receiver
    })

    remainingGifters.splice(remainingGifters.indexOf(gifter), 1)
    remainingReceivers.splice(remainingReceivers.indexOf(receiver), 1)
  }
}
</script>

<template>
  <h1>Secret Santa</h1>
  <h2>List of participants</h2>
  <ul>
    <li v-for="participant in participants" :key="participant.id">
      {{ participant.name }}
    </li>
  </ul>
  <input v-model="newParticipant" placeholder="Add participant" />
  <button @click="addParticipant()">Add</button>

  <h2>List of draws</h2>
  <button @click="draw()">Draw</button>

  <ul>
    <li v-for="draw in draws" :key="draw.id">
      {{ draw.gifter.name }} offers to {{ draw.receiver.name }}
    </li>
  </ul>
</template>

<style scoped></style>

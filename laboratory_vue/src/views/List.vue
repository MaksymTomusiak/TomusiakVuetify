<template>
  <v-text-field
    v-model="newItemTitle"
    label="Add new item"
    append-icon="mdi-plus"
    variant="outlined"
    clearable
    @click:append="addNewItem"
    @keyup.enter="addNewItem"
  ></v-text-field>
  <v-list
    lines="three"
    select-strategy="leaf"
  >
    <v-list-item
      v-for="item in shoppingList"
      :key="item.id"
      :title="item.title"
      :value="item.id"
      :class="{ 'bg-blue-lighten-3': item.bought }"
      @click="doneBought(item.id)"
    >
      <template v-slot:prepend>
        <v-list-item-action start>
          <v-checkbox-btn :model-value="item.bought"></v-checkbox-btn>
        </v-list-item-action>
      </template>
      <template v-slot:title>
        <v-list-item-title :class="{ 'text-decoration-line-through': item.bought }">
          {{ item.title }}
        </v-list-item-title>
      </template>
      <template v-slot:append>
        <v-list-item-action>
          <v-btn icon @click.stop="deleteItem(item.id)">
            <v-icon>mdi-delete</v-icon>
          </v-btn>
        </v-list-item-action>
      </template>
    </v-list-item>
  </v-list>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const shoppingList = ref([
  {
    id: 'notifications',
    title: 'Notifications',
    bought: false
  },
  {
    id: 'sound',
    title: 'Sound',
    bought: false
  },
  {
    id: 'widgets',
    title: 'Auto-add widgets',
    bought: false
  },
])
const newItemTitle = ref('')

const doneBought = (id: string) => {
  const item = shoppingList.value.filter(item => item.id === id)[0]
  item.bought = !item.bought
}

const deleteItem = (id: string) => {
  shoppingList.value = shoppingList.value.filter(item => item.id !== id)
}

const addNewItem = () => {
  if (newItemTitle.value.trim()) {
    shoppingList.value.push({
      id: Date.now().toString(),
      title: newItemTitle.value,
      bought: false
    })
    newItemTitle.value = ''
  }
}
</script>
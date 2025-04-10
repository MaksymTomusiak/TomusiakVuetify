<template>
  <v-card title="Students" flat>
    <template v-slot:text>
      <v-row>
        <v-col cols="12" sm="auto">
          <v-btn color="primary" prepend-icon="mdi-plus" @click="openAddDialog">
            Додати нового студента
          </v-btn>
        </v-col>
      </v-row>
      <v-text-field
        v-model="search"
        label="Search students"
        prepend-inner-icon="mdi-magnify"
        variant="outlined"
        hide-details
        single-line
        class="mt-4"
      ></v-text-field>
    </template>

    <v-data-table :headers="headers" :items="filteredStudents">
      <template v-slot:item.actions="{ item }">
        <div class="d-flex ga-2 justify-end">
          <v-icon
            color="medium-emphasis"
            icon="mdi-pencil"
            size="small"
            @click="editStudent(item)"
          ></v-icon>
          <v-icon
            color="medium-emphasis"
            icon="mdi-delete"
            size="small"
            @click="deleteStudent(item.id)"
          ></v-icon>
        </div>
      </template>
    </v-data-table>

    <v-dialog v-model="dialog" max-width="500">
      <v-card
        :title="isEditing ? 'Редагувати студента' : 'Додати студента'"
        :subtitle="isEditing ? 'Оновіть дані студента' : 'Введіть дані нового студента'"
      >
        <template v-slot:text>
          <v-row>
            <v-col cols="12">
              <v-text-field
                v-model="currentStudent.fullName"
                label="ПІП"
                variant="outlined"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model.number="currentStudent.birthYear"
                label="Рік народження"
                variant="outlined"
                type="number"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="currentStudent.phone"
                label="Телефон"
                variant="outlined"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="currentStudent.email"
                label="Email"
                variant="outlined"
              ></v-text-field>
            </v-col>
          </v-row>
        </template>
        <v-card-actions>
          <v-btn text="Скасувати" variant="plain" @click="dialog = false"></v-btn>
          <v-spacer></v-spacer>
          <v-btn text="Зберегти" color="primary" @click="saveStudent"></v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const search = ref('')
const dialog = ref(false)
const isEditing = ref(false)
const currentStudent = ref({
  id: '',
  fullName: '',
  birthYear: 0,
  phone: '',
  email: '',
})

const headers = [
  { title: 'ПІП', key: 'fullName', align: 'start' },
  { title: 'Рік народження', key: 'birthYear' },
  { title: 'Телефон', key: 'phone' },
  { title: 'Email', key: 'email' },
  { title: 'Дії', key: 'actions', align: 'end', sortable: false },
]

const students = ref([
  {
    id: '1',
    fullName: 'Іванов Іван Іванович',
    birthYear: 2000,
    phone: '+380661234567',
    email: 'ivanov@example.com',
  },
  {
    id: '2',
    fullName: 'Петрова Марія Олександрівна',
    birthYear: 2001,
    phone: '+380671234567',
    email: 'petrova@example.com',
  },
  {
    id: '3',
    fullName: 'Сидоренко Петро Васильович',
    birthYear: 1999,
    phone: '+380681234567',
    email: 'sydorenko@example.com',
  },
  {
    id: '4',
    fullName: 'Коваленко Олена Сергіївна',
    birthYear: 2002,
    phone: '+380691234567',
    email: 'kovalenko@example.com',
  },
  {
    id: '5',
    fullName: 'Мельник Андрій Вікторович',
    birthYear: 2000,
    phone: '+380501234567',
    email: 'melnyk@example.com',
  },
])

const filteredStudents = computed(() => {
  if (!search.value) return students.value
  const searchLower = search.value.toLowerCase()
  return students.value.filter(student =>
    student.fullName.toLowerCase().includes(searchLower) ||
    student.birthYear.toString().includes(searchLower) ||
    student.phone.toLowerCase().includes(searchLower) ||
    student.email.toLowerCase().includes(searchLower)
  )
})

const openAddDialog = () => {
  isEditing.value = false
  currentStudent.value = {
    id: '',
    fullName: '',
    birthYear: 0,
    phone: '',
    email: '',
  }
  dialog.value = true
}

const editStudent = (item: any) => {
  isEditing.value = true
  currentStudent.value = { ...item }
  dialog.value = true
}

const saveStudent = () => {
  if (currentStudent.value.fullName && currentStudent.value.birthYear) {
    if (isEditing.value) {
      const index = students.value.findIndex(s => s.id === currentStudent.value.id)
      if (index !== -1) {
        students.value[index] = { ...currentStudent.value }
      }
    } else {
      students.value.push({
        id: Date.now().toString(),
        fullName: currentStudent.value.fullName,
        birthYear: currentStudent.value.birthYear,
        phone: currentStudent.value.phone,
        email: currentStudent.value.email,
      })
    }
    dialog.value = false
  }
}

const deleteStudent = (id: string) => {
  students.value = students.value.filter(student => student.id !== id)
}
</script>
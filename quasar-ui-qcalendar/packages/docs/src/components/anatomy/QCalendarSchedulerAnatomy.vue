<template>
  <div
    class="row full-width q-pa-xs rounded-borders relative-position anatomy"
    style="border: 2px solid rgba(25, 118, 210, 0.65)"
  >
    <q-scroll-area class="col" style="max-width: 180px; width: 100%; max-height: 300px">
      <q-list dense>
        <q-item
          v-for="type in types"
          :key="type.name"
          clickable
          v-ripple
          :active="selected.name === type.name"
          @click="updateSelection(type)"
        >
          <q-item-section>
            {{ type.name }}
          </q-item-section>
        </q-item>
      </q-list>
    </q-scroll-area>

    <div class="col">
      <q-calendar-scheduler
        id="calendar-scheduler"
        ref="calendar"
        v-model:model-resources="resources"
        view="week"
        bordered
        resource-key="id"
        resource-label="name"
        style="height: 300px"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, watch, onMounted } from 'vue'
import { QCalendarScheduler } from '@quasar/quasar-ui-qcalendar'
import '@quasar/quasar-ui-qcalendar/index.css'

interface Type {
  name: string
  class: string
}

interface Resource {
  id: string
  name: string
  expanded?: boolean
  children?: Resource[]
}

const calendar = ref<HTMLElement | null>(null)
const types = reactive<Type[]>([
  { name: 'Head', class: 'q-calendar-scheduler__head' },
  { name: 'Head Resources', class: 'q-calendar-scheduler__head--resources' },
  { name: 'Head Weekdays', class: 'q-calendar-scheduler__head--days__weekdays' },
  { name: 'Head Day', class: 'q-calendar-scheduler__head--day' },
  { name: 'Head Weekday', class: 'q-calendar-scheduler__head--weekday' },
  { name: 'Head Date', class: 'q-calendar-scheduler__head--date' },
  { name: 'Head Day Button', class: 'q-calendar-scheduler__head--day__label' },
  { name: 'Head Day Event', class: 'q-calendar-scheduler__head--day__event' },
  { name: 'Body', class: 'q-calendar-scheduler__body' },
  { name: 'Container', class: 'q-calendar-scheduler__day--container' },
  { name: 'Resource Row', class: 'q-calendar-scheduler__resource--row' },
  { name: 'Resource', class: 'q-calendar-scheduler__resource' },
  { name: 'Resource Section', class: 'q-calendar-scheduler__resource--section' },
  { name: 'Parent', class: 'q-calendar__parent' },
  { name: 'Child', class: 'q-calendar__child' },
  { name: 'Resource Text', class: 'q-calendar-scheduler__resource--text' },
  { name: 'Resource Days', class: 'q-calendar-scheduler__resource--days' },
  { name: 'Resource Day', class: 'q-calendar-scheduler__day' },
  { name: 'Resource Day Section', class: 'q-calendar-scheduler__day--section' },
])
const selected = ref<Type>(types[0] as Type)
const el = ref<HTMLElement | null>(null)
const resources = ref<Resource[]>([
  { id: '1', name: 'John' },
  {
    id: '2',
    name: 'Board Room',
    expanded: false,
    children: [
      { id: '2.1', name: 'Room-1' },
      {
        id: '2.2',
        name: 'Room-2',
        expanded: false,
        children: [
          { id: '2.2.1', name: 'Partition-A' },
          { id: '2.2.2', name: 'Partition-B' },
          { id: '2.2.3', name: 'Partition-C' },
        ],
      },
    ],
  },
  { id: '3', name: 'Mary' },
  { id: '4', name: 'Susan' },
  { id: '5', name: 'Olivia' },
])

watch(
  () => selected.value,
  (current: Type) => {
    removeClass()
    addClass(current)
  },
)

onMounted(() => {
  el.value = document.getElementById('calendar-scheduler')
  setTimeout(() => {
    addClass(selected.value)
  }, 350)
})

function updateSelection(type: Type) {
  selected.value = type
}

function addClass(type: Type) {
  if (type.class !== undefined) {
    const els = el.value?.querySelectorAll('.' + type.class)
    if (els && els.length > 0) {
      els.forEach((el) => {
        el.classList.add('highlight')
      })
    }
  }
}

function removeClass() {
  const els = el.value?.querySelectorAll('.highlight')
  if (els && els.length > 0) {
    els.forEach((el) => {
      el.classList.remove('highlight')
    })
  }
}
</script>

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
      <q-calendar-month
        id="calendar-month"
        ref="calendar"
        bordered
        day-min-height="60"
        show-work-weeks
        show-day-of-year-label
        date-align="right"
      >
        <template #week />
      </q-calendar-month>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, watch, onMounted } from 'vue'
import { QCalendarMonth } from '@quasar/quasar-ui-qcalendar'
import '@quasar/quasar-ui-qcalendar/index.css'

interface Type {
  name: string
  class: string
}

const calendar = ref<HTMLElement | null>(null)
const types = reactive<Type[]>([
  { name: 'Head Workweek', class: 'q-calendar-month__head--workweek' },
  { name: 'Head', class: 'q-calendar-month__head' },
  { name: 'Workweek', class: 'q-calendar-month__workweek' },
  { name: 'Weekdays', class: 'q-calendar-month__head--weekdays' },
  { name: 'Weekday', class: 'q-calendar-month__head--weekday' },
  { name: 'Head Day Event', class: 'q-calendar-month__head--event' },
  { name: 'Week', class: 'q-calendar-month__week--wrapper' },
  { name: 'Week Days', class: 'q-calendar-month__week--days' },
  { name: 'Week Events', class: 'q-calendar-month__week--events' },
  { name: 'Day', class: 'q-calendar-month__day' },
  { name: 'Day Label Wrapper', class: 'q-calendar-month__day--label__wrapper' },
  { name: 'Day Label', class: 'q-calendar-month__day--label' },
  { name: 'Day of Year Label', class: 'q-calendar-month__day--day-of-year' },
  { name: 'Month Label', class: 'q-calendar-month__day--month' },
  { name: 'Day Content', class: 'q-calendar-month__day--content' },
  { name: 'Body', class: 'q-calendar-month__body' },
])
const selected = ref<Type>(types[0] as Type)
const el = ref<HTMLElement | null>(null)

watch(
  () => selected.value,
  (current: Type) => {
    removeClass()
    addClass(current)
  },
)

onMounted(() => {
  el.value = document.getElementById('calendar-month')
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

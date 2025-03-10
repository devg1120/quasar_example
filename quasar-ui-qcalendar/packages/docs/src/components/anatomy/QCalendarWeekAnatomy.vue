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
      <q-calendar-day
        id="calendar-week"
        ref="calendar"
        view="week"
        bordered
        :interval-minutes="15"
        :interval-count="96"
        :interval-height="20"
        style="height: 300px"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, watch, onMounted } from 'vue'
import { QCalendarDay } from '@quasar/quasar-ui-qcalendar'
import '@quasar/quasar-ui-qcalendar/index.css'

interface Type {
  name: string
  class: string
}

const calendar = ref<HTMLElement | null>(null)
const types = reactive<Type[]>([
  { name: 'Head Intervals', class: 'q-calendar-day__head--intervals' },
  { name: 'Interval', class: 'q-calendar-day__interval' },
  { name: 'Interval Section', class: 'q-calendar-day__interval--section' },
  { name: 'Head Day', class: 'q-calendar-day__head--day' },
  { name: 'Head Weekday', class: 'q-calendar-day__head--weekday' },
  { name: 'Head Weekday Label', class: 'q-calendar-day__head--weekday-label' },
  { name: 'Head Date', class: 'q-calendar-day__head--date' },
  { name: 'Head Day Button', class: 'q-calendar-day__head--day__label' },
  { name: 'Head Day Event', class: 'q-calendar-day__head--day__event' },
  { name: 'Head Days Events', class: 'q-calendar-day__head--days__event' },
  { name: 'Day Container', class: 'q-calendar-day__day-container' },
  { name: 'Day Body', class: 'q-calendar-day__body' },
  { name: 'Day', class: 'q-calendar-day__day' },
  { name: 'Day Interval', class: 'q-calendar-day__day-interval' },
  { name: 'Day Section', class: 'q-calendar-day__day-interval--section' },
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
  el.value = document.getElementById('calendar-week')
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

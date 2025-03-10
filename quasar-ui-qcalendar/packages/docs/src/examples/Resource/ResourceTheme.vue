<template>
  <div class="subcontent">
    <navigation-bar @today="onToday" @prev="onPrev" @next="onNext" />

    <div class="q-ma-sm row justify-center q-gutter-sm">
      <q-select
        v-model="selectedTheme"
        label="Choose a theme"
        outlined
        dense
        map-options
        emit-value
        options-dense
        :options="themesList"
        style="min-width: 180px"
      />
    </div>

    <div class="row justify-center">
      <div style="display: flex; max-width: 800px; width: 100%; max-height: 400px">
        <q-calendar-resource
          ref="calendar"
          v-model="selectedDate"
          v-model:model-resources="resources"
          resource-key="id"
          resource-label="name"
          animated
          bordered
          :style="selectedTheme"
          @change="onChange"
          @moved="onMoved"
          @resource-expanded="onResourceExpanded"
          @click-date="onClickDate"
          @click-time="onClickTime"
          @click-resource="onClickResource"
          @click-head-resources="onClickHeadResources"
          @click-interval="onClickInterval"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { QCalendarResource, today, Timestamp } from '@quasar/quasar-ui-qcalendar'
import '@quasar/quasar-ui-qcalendar/index.css'

import { ref, reactive, computed } from 'vue'
import NavigationBar from 'components/NavigationBar.vue'

type Theme = Record<string, string>

const calendar = ref<QCalendarResource>()
const selectedDate = ref(today())
const selectedTheme = ref<Theme>({})
const themes = reactive<Record<string, Theme>>({
  default: {},
  teal: {
    '--calendar-scrollbar-track': '#4db6ac',
    '--calendar-scrollbar-thumb': '#004d40',
    '--calendar-scrollbar-thumb-hover': '#00897b',
    '--calendar-scrollbar-track-dark': '#4db6ac',
    '--calendar-scrollbar-thumb-dark': '#004d40',
    '--calendar-scrollbar-thumb-hover-dark': '#00897b',
    '--calendar-border': '#4db6ac 1px solid',
    '--calendar-border-dark': '#e0f2f1 1px solid',
    '--calendar-border-section': '#80cbc4 1px dashed',
    '--calendar-border-section-dark': '#80cbc4 1px dashed',
    '--calendar-border-current': '#4db6ac 2px solid',
    '--calendar-border-current-dark': '#1de9b6 2px solid',
    '--calendar-mini-range-connector-hover-border': '#027BE3 1px dashed',
    '--calendar-mini-range-connector-hover-border-dark': '#ffff66 1px dashed',
    '--calendar-color': '#004d40',
    '--calendar-color-dark': '#e0f2f1',
    '--calendar-background': '#e0f2f1',
    '--calendar-background-dark': '#004d40',
    '--calendar-current-color': '#027BE3',
    '--calendar-current-color-dark': '#ffff66',
    '--calendar-current-background': '#00000000',
    '--calendar-current-background-dark': '#004d40',
    '--calendar-disabled-date-color': '#e0f2f1',
    '--calendar-disabled-date-color-dark': '#bebebe',
    '--calendar-disabled-date-background': '#80cbc4',
    '--calendar-disabled-date-background-dark': '#121212',
    '--calendar-active-date-color': '#000000',
    '--calendar-active-date-color-dark': '#ffff66',
    '--calendar-active-date-background': '#1de9b6',
    '--calendar-active-date-background-dark': '#4db6ac',
    '--calendar-outside-color': '#004d40',
    '--calendar-outside-color-dark': '#bebebe',
    '--calendar-outside-background': '#00000000',
    '--calendar-outside-background-dark': '#121212',
    '--calendar-selected-color': '#027BE3',
    '--calendar-selected-color-dark': '#027BE3',
    '--calendar-selected-background': '#cce7ff',
    '--calendar-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-color': 'unset',
    '--calendar-mini-selected-color-dark': '#027BE3',
    '--calendar-mini-selected-background': 'unset',
    '--calendar-mini-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-label-color': '#027BE3',
    '--calendar-mini-selected-label-color-dark': '#cce7ff',
    '--calendar-mini-selected-label-background': '#cce7ff',
    '--calendar-mini-selected-label-background-dark': '#027BE3',
    '--calendar-range-color': '#027BE3',
    '--calendar-range-color-dark': '#027BE3',
    '--calendar-range-background': '#cce7ff',
    '--calendar-range-background-dark': '#cce7ff',
    '--calendar-mini-range-color': '#cce7ff',
    '--calendar-mini-range-color-dark': '#027BE3',
    '--calendar-mini-range-background': 'unset',
    '--calendar-mini-range-background-dark': 'unset',
    '--calendar-mini-range-label-color': '#cce7ff',
    '--calendar-mini-range-label-color-dark': '#027BE3',
    '--calendar-mini-range-label-background': '#cce7ff',
    '--calendar-mini-range-label-background-dark': '#cce7ff',
    '--calendar-mini-range-connector-color': '#cce7ff',
    '--calendar-mini-range-connector-color-dark': '#ffff66',
    '--calendar-mini-range-hover-color': '#027BE3',
    '--calendar-mini-range-hover-color-dark': '#ffff66',
    '--calendar-mini-range-firstlast-color': '#cce7ff',
    '--calendar-mini-range-firstlast-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-background': 'unset',
    '--calendar-mini-range-firstlast-background-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-background': '#027BE3',
    '--calendar-mini-range-firstlast-label-background-dark': '#ffff66',
    '--calendar-intervals-width': '56px',
    '--calendar-work-week-width': '30px',
    '--calendar-mini-work-week-width': '30px',
    '--calendar-work-week-font-size': '1.0em',
    '--calendar-head-font-weight': '600',
  },
  brown: {
    '--calendar-scrollbar-track': '#a1887f',
    '--calendar-scrollbar-thumb': '#3e2723',
    '--calendar-scrollbar-thumb-hover': '#6d4c41',
    '--calendar-scrollbar-track-dark': '#a1887f',
    '--calendar-scrollbar-thumb-dark': '#3e2723',
    '--calendar-scrollbar-thumb-hover-dark': '#6d4c41',
    '--calendar-border': '#a1887f 1px solid',
    '--calendar-border-dark': '#efebe9 1px solid',
    '--calendar-border-section': '#bcaaa4 1px dashed',
    '--calendar-border-section-dark': '#bcaaa4 1px dashed',
    '--calendar-border-current': '#a1887f 2px solid',
    '--calendar-border-current-dark': '#8d6e63 2px solid',
    '--calendar-mini-range-connector-hover-border': '#027BE3 1px dashed',
    '--calendar-mini-range-connector-hover-border-dark': '#ffff66 1px dashed',
    '--calendar-color': '#3e2723',
    '--calendar-color-dark': '#efebe9',
    '--calendar-background': '#efebe9',
    '--calendar-background-dark': '#3e2723',
    '--calendar-current-color': '#027BE3',
    '--calendar-current-color-dark': '#efebe9',
    '--calendar-current-background': '#00000000',
    '--calendar-current-background-dark': '#3e2723',
    '--calendar-disabled-date-color': '#efebe9',
    '--calendar-disabled-date-color-dark': '#bebebe',
    '--calendar-disabled-date-background': '#bcaaa4',
    '--calendar-disabled-date-background-dark': '#121212',
    '--calendar-active-date-color': '#efebe9',
    '--calendar-active-date-color-dark': '#ffff66',
    '--calendar-active-date-background': '#8d6e63',
    '--calendar-active-date-background-dark': '#a1887f',
    '--calendar-outside-color': '#3e2723',
    '--calendar-outside-color-dark': '#bebebe',
    '--calendar-outside-background': '#00000000',
    '--calendar-outside-background-dark': '#121212',
    '--calendar-selected-color': '#027BE3',
    '--calendar-selected-color-dark': '#027BE3',
    '--calendar-selected-background': '#cce7ff',
    '--calendar-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-color': 'unset',
    '--calendar-mini-selected-color-dark': '#027BE3',
    '--calendar-mini-selected-background': 'unset',
    '--calendar-mini-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-label-color': '#027BE3',
    '--calendar-mini-selected-label-color-dark': '#cce7ff',
    '--calendar-mini-selected-label-background': '#cce7ff',
    '--calendar-mini-selected-label-background-dark': '#027BE3',
    '--calendar-range-color': '#027BE3',
    '--calendar-range-color-dark': '#027BE3',
    '--calendar-range-background': '#cce7ff',
    '--calendar-range-background-dark': '#cce7ff',
    '--calendar-mini-range-color': '#cce7ff',
    '--calendar-mini-range-color-dark': '#027BE3',
    '--calendar-mini-range-background': 'unset',
    '--calendar-mini-range-background-dark': 'unset',
    '--calendar-mini-range-label-color': '#cce7ff',
    '--calendar-mini-range-label-color-dark': '#027BE3',
    '--calendar-mini-range-label-background': '#cce7ff',
    '--calendar-mini-range-label-background-dark': '#cce7ff',
    '--calendar-mini-range-connector-color': '#cce7ff',
    '--calendar-mini-range-connector-color-dark': '#ffff66',
    '--calendar-mini-range-hover-color': '#027BE3',
    '--calendar-mini-range-hover-color-dark': '#ffff66',
    '--calendar-mini-range-firstlast-color': '#cce7ff',
    '--calendar-mini-range-firstlast-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-background': 'unset',
    '--calendar-mini-range-firstlast-background-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-background': '#027BE3',
    '--calendar-mini-range-firstlast-label-background-dark': '#ffff66',
    '--calendar-intervals-width': '56px',
    '--calendar-work-week-width': '30px',
    '--calendar-mini-work-week-width': '30px',
    '--calendar-work-week-font-size': '1.0em',
    '--calendar-head-font-weight': '600',
  },
  'deep purple': {
    '--calendar-scrollbar-track': '#9575cd',
    '--calendar-scrollbar-thumb': '#311b92',
    '--calendar-scrollbar-thumb-hover': '#5e35b1',
    '--calendar-scrollbar-track-dark': '#9575cd',
    '--calendar-scrollbar-thumb-dark': '#311b92',
    '--calendar-scrollbar-thumb-hover-dark': '#5e35b1',
    '--calendar-border': '#9575cd 1px solid',
    '--calendar-border-dark': '#ede7f6 1px solid',
    '--calendar-border-section': '#b39ddb 1px dashed',
    '--calendar-border-section-dark': '#b39ddb 1px dashed',
    '--calendar-border-current': '#9575cd 2px solid',
    '--calendar-border-current-dark': '#651fff 2px solid',
    '--calendar-mini-range-connector-hover-border': '#027BE3 1px dashed',
    '--calendar-mini-range-connector-hover-border-dark': '#ffff66 1px dashed',
    '--calendar-color': '#311b92',
    '--calendar-color-dark': '#ede7f6',
    '--calendar-background': '#ede7f6',
    '--calendar-background-dark': '#311b92',
    '--calendar-current-color': '#027BE3',
    '--calendar-current-color-dark': '#651fff',
    '--calendar-current-background': '#00000000',
    '--calendar-current-background-dark': '#311b92',
    '--calendar-disabled-date-color': '#ede7f6',
    '--calendar-disabled-date-color-dark': '#bebebe',
    '--calendar-disabled-date-background': '#b39ddb',
    '--calendar-disabled-date-background-dark': '#121212',
    '--calendar-active-date-color': '#ede7f6',
    '--calendar-active-date-color-dark': '#ffff66',
    '--calendar-active-date-background': '#651fff',
    '--calendar-active-date-background-dark': '#9575cd',
    '--calendar-outside-color': '#311b92',
    '--calendar-outside-color-dark': '#bebebe',
    '--calendar-outside-background': '#00000000',
    '--calendar-outside-background-dark': '#121212',
    '--calendar-selected-color': '#027BE3',
    '--calendar-selected-color-dark': '#027BE3',
    '--calendar-selected-background': '#cce7ff',
    '--calendar-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-color': 'unset',
    '--calendar-mini-selected-color-dark': '#027BE3',
    '--calendar-mini-selected-background': 'unset',
    '--calendar-mini-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-label-color': '#027BE3',
    '--calendar-mini-selected-label-color-dark': '#cce7ff',
    '--calendar-mini-selected-label-background': '#cce7ff',
    '--calendar-mini-selected-label-background-dark': '#027BE3',
    '--calendar-range-color': '#027BE3',
    '--calendar-range-color-dark': '#027BE3',
    '--calendar-range-background': '#cce7ff',
    '--calendar-range-background-dark': '#cce7ff',
    '--calendar-mini-range-color': '#cce7ff',
    '--calendar-mini-range-color-dark': '#027BE3',
    '--calendar-mini-range-background': 'unset',
    '--calendar-mini-range-background-dark': 'unset',
    '--calendar-mini-range-label-color': '#cce7ff',
    '--calendar-mini-range-label-color-dark': '#027BE3',
    '--calendar-mini-range-label-background': '#cce7ff',
    '--calendar-mini-range-label-background-dark': '#cce7ff',
    '--calendar-mini-range-connector-color': '#cce7ff',
    '--calendar-mini-range-connector-color-dark': '#ffff66',
    '--calendar-mini-range-hover-color': '#027BE3',
    '--calendar-mini-range-hover-color-dark': '#ffff66',
    '--calendar-mini-range-firstlast-color': '#cce7ff',
    '--calendar-mini-range-firstlast-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-background': 'unset',
    '--calendar-mini-range-firstlast-background-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-background': '#027BE3',
    '--calendar-mini-range-firstlast-label-background-dark': '#ffff66',
    '--calendar-intervals-width': '56px',
    '--calendar-work-week-width': '30px',
    '--calendar-mini-work-week-width': '30px',
    '--calendar-work-week-font-size': '1.0em',
    '--calendar-head-font-weight': '600',
  },
  indigo: {
    '--calendar-scrollbar-track': '#7986cb',
    '--calendar-scrollbar-thumb': '#1a237e',
    '--calendar-scrollbar-thumb-hover': '#3949ab',
    '--calendar-scrollbar-track-dark': '#7986cb',
    '--calendar-scrollbar-thumb-dark': '#1a237e',
    '--calendar-scrollbar-thumb-hover-dark': '#3949ab',
    '--calendar-border': '#7986cb 1px solid',
    '--calendar-border-dark': '#e8eaf6 1px solid',
    '--calendar-border-section': '#9fa8da 1px dashed',
    '--calendar-border-section-dark': '#9fa8da 1px dashed',
    '--calendar-border-current': '#7986cb 2px solid',
    '--calendar-border-current-dark': '#3d5afe 2px solid',
    '--calendar-mini-range-connector-hover-border': '#027BE3 1px dashed',
    '--calendar-mini-range-connector-hover-border-dark': '#ffff66 1px dashed',
    '--calendar-color': '#1a237e',
    '--calendar-color-dark': '#e8eaf6',
    '--calendar-background': '#e8eaf6',
    '--calendar-background-dark': '#1a237e',
    '--calendar-current-color': '#027BE3',
    '--calendar-current-color-dark': '#3d5afe',
    '--calendar-current-background': '#00000000',
    '--calendar-current-background-dark': '#1a237e',
    '--calendar-disabled-date-color': '#e8eaf6',
    '--calendar-disabled-date-color-dark': '#bebebe',
    '--calendar-disabled-date-background': '#9fa8da',
    '--calendar-disabled-date-background-dark': '#121212',
    '--calendar-active-date-color': '#e8eaf6',
    '--calendar-active-date-color-dark': '#ffff66',
    '--calendar-active-date-background': '#3d5afe',
    '--calendar-active-date-background-dark': '#7986cb',
    '--calendar-outside-color': '#1a237e',
    '--calendar-outside-color-dark': '#bebebe',
    '--calendar-outside-background': '#00000000',
    '--calendar-outside-background-dark': '#121212',
    '--calendar-selected-color': '#027BE3',
    '--calendar-selected-color-dark': '#027BE3',
    '--calendar-selected-background': '#cce7ff',
    '--calendar-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-color': 'unset',
    '--calendar-mini-selected-color-dark': '#027BE3',
    '--calendar-mini-selected-background': 'unset',
    '--calendar-mini-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-label-color': '#027BE3',
    '--calendar-mini-selected-label-color-dark': '#cce7ff',
    '--calendar-mini-selected-label-background': '#cce7ff',
    '--calendar-mini-selected-label-background-dark': '#027BE3',
    '--calendar-range-color': '#027BE3',
    '--calendar-range-color-dark': '#027BE3',
    '--calendar-range-background': '#cce7ff',
    '--calendar-range-background-dark': '#cce7ff',
    '--calendar-mini-range-color': '#cce7ff',
    '--calendar-mini-range-color-dark': '#027BE3',
    '--calendar-mini-range-background': 'unset',
    '--calendar-mini-range-background-dark': 'unset',
    '--calendar-mini-range-label-color': '#cce7ff',
    '--calendar-mini-range-label-color-dark': '#027BE3',
    '--calendar-mini-range-label-background': '#cce7ff',
    '--calendar-mini-range-label-background-dark': '#cce7ff',
    '--calendar-mini-range-connector-color': '#cce7ff',
    '--calendar-mini-range-connector-color-dark': '#ffff66',
    '--calendar-mini-range-hover-color': '#027BE3',
    '--calendar-mini-range-hover-color-dark': '#ffff66',
    '--calendar-mini-range-firstlast-color': '#cce7ff',
    '--calendar-mini-range-firstlast-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-background': 'unset',
    '--calendar-mini-range-firstlast-background-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-background': '#027BE3',
    '--calendar-mini-range-firstlast-label-background-dark': '#ffff66',
    '--calendar-intervals-width': '56px',
    '--calendar-work-week-width': '30px',
    '--calendar-mini-work-week-width': '30px',
    '--calendar-work-week-font-size': '1.0em',
    '--calendar-head-font-weight': '600',
  },
  blue: {
    '--calendar-scrollbar-track': '#64b5f6',
    '--calendar-scrollbar-thumb': '#0d47a0',
    '--calendar-scrollbar-thumb-hover': '#1e88e5',
    '--calendar-scrollbar-track-dark': '#64b5f6',
    '--calendar-scrollbar-thumb-dark': '#0d47a0',
    '--calendar-scrollbar-thumb-hover-dark': '#1e88e5',
    '--calendar-border': '#64b5f6 1px solid',
    '--calendar-border-dark': '#e3f2fd 1px solid',
    '--calendar-border-section': '#90caf9 1px dashed',
    '--calendar-border-section-dark': '#90caf9 1px dashed',
    '--calendar-border-current': '#64b5f6 2px solid',
    '--calendar-border-current-dark': '#2979ff 2px solid',
    '--calendar-mini-range-connector-hover-border': '#027BE3 1px dashed',
    '--calendar-mini-range-connector-hover-border-dark': '#ffff66 1px dashed',
    '--calendar-color': '#0d47a0',
    '--calendar-color-dark': '#e3f2fd',
    '--calendar-background': '#e3f2fd',
    '--calendar-background-dark': '#0d47a0',
    '--calendar-current-color': '#027BE3',
    '--calendar-current-color-dark': '#2979ff',
    '--calendar-current-background': '#00000000',
    '--calendar-current-background-dark': '#0d47a0',
    '--calendar-disabled-date-color': '#e3f2fd',
    '--calendar-disabled-date-color-dark': '#bebebe',
    '--calendar-disabled-date-background': '#90caf9',
    '--calendar-disabled-date-background-dark': '#121212',
    '--calendar-active-date-color': '#e3f2fd',
    '--calendar-active-date-color-dark': '#ffff66',
    '--calendar-active-date-background': '#2979ff',
    '--calendar-active-date-background-dark': '#64b5f6',
    '--calendar-outside-color': '#0d47a0',
    '--calendar-outside-color-dark': '#bebebe',
    '--calendar-outside-background': '#00000000',
    '--calendar-outside-background-dark': '#121212',
    '--calendar-selected-color': '#027BE3',
    '--calendar-selected-color-dark': '#027BE3',
    '--calendar-selected-background': '#cce7ff',
    '--calendar-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-color': 'unset',
    '--calendar-mini-selected-color-dark': '#027BE3',
    '--calendar-mini-selected-background': 'unset',
    '--calendar-mini-selected-background-dark': '#cce7ff',
    '--calendar-mini-selected-label-color': '#027BE3',
    '--calendar-mini-selected-label-color-dark': '#cce7ff',
    '--calendar-mini-selected-label-background': '#cce7ff',
    '--calendar-mini-selected-label-background-dark': '#027BE3',
    '--calendar-range-color': '#027BE3',
    '--calendar-range-color-dark': '#027BE3',
    '--calendar-range-background': '#cce7ff',
    '--calendar-range-background-dark': '#cce7ff',
    '--calendar-mini-range-color': '#cce7ff',
    '--calendar-mini-range-color-dark': '#027BE3',
    '--calendar-mini-range-background': 'unset',
    '--calendar-mini-range-background-dark': 'unset',
    '--calendar-mini-range-label-color': '#cce7ff',
    '--calendar-mini-range-label-color-dark': '#027BE3',
    '--calendar-mini-range-label-background': '#cce7ff',
    '--calendar-mini-range-label-background-dark': '#cce7ff',
    '--calendar-mini-range-connector-color': '#cce7ff',
    '--calendar-mini-range-connector-color-dark': '#ffff66',
    '--calendar-mini-range-hover-color': '#027BE3',
    '--calendar-mini-range-hover-color-dark': '#ffff66',
    '--calendar-mini-range-firstlast-color': '#cce7ff',
    '--calendar-mini-range-firstlast-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-background': 'unset',
    '--calendar-mini-range-firstlast-background-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color': '#cce7ff',
    '--calendar-mini-range-firstlast-label-color-dark': '#cce7ff',
    '--calendar-mini-range-firstlast-label-background': '#027BE3',
    '--calendar-mini-range-firstlast-label-background-dark': '#ffff66',
    '--calendar-intervals-width': '56px',
    '--calendar-work-week-width': '30px',
    '--calendar-mini-work-week-width': '30px',
    '--calendar-work-week-font-size': '1.0em',
    '--calendar-head-font-weight': '600',
  },
})
const resources = ref<Record<string, string>[]>([
  { id: '1', name: 'John' },
  { id: '2', name: 'Board Room' },
  { id: '3', name: 'Mary' },
  { id: '4', name: 'Susan' },
  { id: '5', name: 'Olivia' },
])

const themesList = computed(() => {
  const list: { label: string; value: Theme }[] = []
  Object.keys(themes).forEach((theme) => {
    list.push({
      label: theme,
      value: { ...themes[theme] },
    })
  })
  console.info('themesList', list)
  return list
})

function onToday() {
  if (calendar.value) {
    calendar.value.moveToToday()
  }
}
function onPrev() {
  if (calendar.value) {
    calendar.value.prev()
  }
}
function onNext() {
  if (calendar.value) {
    calendar.value.next()
  }
}
function onMoved(data: Timestamp) {
  console.info('onMoved', data)
}
function onChange(data: { start: Timestamp; end: Timestamp; days: Timestamp[] }) {
  console.info('onChange', data)
}
function onResourceExpanded(data: Timestamp) {
  console.info('onResourceExpanded', data)
}
function onClickDate(data: Timestamp) {
  console.info('onClickDate', data)
}
function onClickTime(data: Timestamp) {
  console.info('onClickTime', data)
}
function onClickResource(data: Timestamp) {
  console.info('onClickResource', data)
}
function onClickHeadResources(data: Timestamp) {
  console.info('onClickHeadResources', data)
}
function onClickInterval(data: Timestamp) {
  console.info('onClickInterval', data)
}
</script>

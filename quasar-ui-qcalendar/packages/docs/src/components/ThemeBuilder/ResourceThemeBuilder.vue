<template>
  <div class="q-gutter-sm">
    <div class="column">
      <div class="row full-width">
        <div class="column justify-center">
          <div class="row q-mb-lg">
            <span class="no-wrap"
              >Note: setting Resource Height to 0 will make it 'auto' height</span
            >
          </div>
          <div class="row no-wrap" style="width: 600px">
            <span class="col-shrink no-wrap" style="min-width: 142px">Resource Height</span>
            <q-slider v-model="resourceHeight" label label-always :min="0" :max="200" class="col" />
          </div>
          <div class="row no-wrap" style="width: 600px">
            <span class="col-shrink no-wrap" style="min-width: 142px">Resource Min. Height</span>
            <q-slider
              v-model="resourceMinHeight"
              label
              label-always
              :min="0"
              :max="200"
              class="col"
            />
          </div>
        </div>
      </div>
    </div>
    <q-calendar-resource
      v-model="selectedDate"
      v-model:model-resources="resources"
      :resource-height="resourceHeight"
      :resource-min-height="resourceMinHeight"
      resource-key="id"
      resource-label="name"
      locale="en-US"
      bordered
      style="max-height: 400px"
      :style="styles"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onBeforeMount } from 'vue'
import { QCalendarResource } from '@quasar/quasar-ui-qcalendar'
import '@quasar/quasar-ui-qcalendar/index.css'

interface Props {
  modelValue: string
  styles: Record<string, any>
}

interface Resource {
  id: string
  name: string
  expanded?: boolean
  children?: Resource[]
}

const props = defineProps<Props>()

const selectedDate = ref('')
const resourceHeight = ref(70)
const resourceMinHeight = ref(20)
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
  () => props.modelValue,
  (val) => {
    selectedDate.value = val
  },
)

onBeforeMount(() => {
  selectedDate.value = props.modelValue
})
</script>

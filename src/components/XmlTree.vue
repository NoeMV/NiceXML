<script setup lang="ts">
import { ref, computed } from 'vue'

const props = defineProps<{
  node: any;
  name: string;
  isRoot?: boolean;
}>()

const emit = defineEmits(['node-selected', 'node-deselected'])

const isOpen = ref(props.isRoot || false)
const isSelected = ref(false)

const isObject = computed(() => typeof props.node === 'object' && props.node !== null)
const isArray = computed(() => Array.isArray(props.node))

const attributes = computed(() => {
  if (isObject.value && props.node['_attrs']) {
    return props.node['_attrs']
  }
  return {}
})

const textValue = computed(() => {
  if (isObject.value && props.node['#text']) {
    return props.node['#text']
  }
  if (!isObject.value && !isArray.value) {
    return props.node
  }
  return null
})

const childNodes = computed(() => {
  if (isObject.value) {
    const children: Record<string, any> = {}
    for (const key in props.node) {
      if (key !== '_attrs' && key !== '#text') {
        children[key] = props.node[key]
      }
    }
    return children
  }
  return {}
})

const hasChildren = computed(() => {
  if (isArray.value) return props.node.length > 0
  if (isObject.value) return Object.keys(childNodes.value).length > 0 || Object.keys(attributes.value).length > 0
  return false
})

function onSelectionChange() {
  if (isSelected.value) {
    emit('node-selected', { name: props.name, ...props.node });
  } else {
    emit('node-deselected', { name: props.name, ...props.node });
  }
}

</script>

<template>
  <div class="ml-6 border-l border-gray-300">
    <div class="flex items-center">
      <button @click="isOpen = !isOpen" v-if="hasChildren" class="text-lg mr-1 focus:outline-none w-6 text-center">
        {{ isOpen ? '▼' : '►' }}
      </button>
      <div v-else class="w-6 mr-1"></div>
      <input type="checkbox" v-model="isSelected" @change="onSelectionChange" class="mr-2" />
      <span class="font-bold text-brown-700">{{ name }}</span>
      <span v-if="textValue && !hasChildren" class="ml-2 text-gray-700">= "{{ textValue }}"</span>
    </div>

    <div v-if="isOpen && hasChildren" class="pl-4">
      <div v-if="Object.keys(attributes).length > 0" class="ml-6 border-l border-dashed border-gray-300">
        <div class="font-semibold text-purple-600 pl-2">Attributes:</div>
        <div v-for="(value, key) in attributes" :key="key" class="ml-8">
          <span class="text-green-600">{{ key }}</span>:
          <span class="text-gray-700">"{{ value }}"</span>
        </div>
      </div>
      <XmlTree v-for="(value, key) in childNodes" :key="key" :node="value" :name="key" @node-selected="$emit('node-selected', $event)" @node-deselected="$emit('node-deselected', $event)" />
      <div v-if="isArray">
        <XmlTree v-for="(item, index) in node" :key="index" :node="item" :name="`[${index}]`" @node-selected="$emit('node-selected', $event)" @node-deselected="$emit('node-deselected', $event)" />
      </div>
       <div v-if="textValue && hasChildren" class="ml-6 pl-2 text-gray-700">
        <span class="font-semibold">Value:</span> "{{ textValue }}"
      </div>
    </div>
  </div>
</template>

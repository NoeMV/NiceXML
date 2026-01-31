<script setup lang="ts">
import { ref, computed } from 'vue'

const props = defineProps<{
  node: any;
  name: string;
  isRoot?: boolean;
}>()

const isOpen = ref(props.isRoot || false)

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
</script>

<template>
  <div class="ml-6 border-l border-gray-300 dark:border-gray-600">
    <div class="flex items-center">
      <button @click="isOpen = !isOpen" v-if="hasChildren" class="text-lg mr-1 focus:outline-none w-6 text-center">
        {{ isOpen ? '▼' : '►' }}
      </button>
      <div v-else class="w-6 mr-1"></div>
      <span class="font-bold text-blue-600 dark:text-blue-400">{{ name }}</span>
      <span v-if="textValue && !hasChildren" class="ml-2 text-gray-700 dark:text-gray-300">= "{{ textValue }}"</span>
    </div>

    <div v-if="isOpen && hasChildren" class="pl-4">
      <div v-if="Object.keys(attributes).length > 0" class="ml-6 border-l border-dashed border-gray-300 dark:border-gray-600">
        <div class="font-semibold text-purple-600 dark:text-purple-400 pl-2">Attributes:</div>
        <div v-for="(value, key) in attributes" :key="key" class="ml-8">
          <span class="text-green-600 dark:text-green-400">{{ key }}</span>:
          <span class="text-gray-700 dark:text-gray-300">"{{ value }}"</span>
        </div>
      </div>
      <XmlNode v-for="(value, key) in childNodes" :key="key" :node="value" :name="key" />
      <div v-if="isArray">
        <XmlNode v-for="(item, index) in node" :key="index" :node="item" :name="`[${index}]`" />
      </div>
       <div v-if="textValue && hasChildren" class="ml-6 pl-2 text-gray-700 dark:text-gray-300">
        <span class="font-semibold">Value:</span> "{{ textValue }}"
      </div>
    </div>
  </div>
</template>

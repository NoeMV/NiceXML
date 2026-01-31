<script setup lang="ts">
import { ref } from 'vue'
import { XMLParser } from 'fast-xml-parser'
import XmlNode from './components/XmlNode.vue'

const xmlContent = ref<string | null>(null)
const parsedXml = ref<any>(null)
const fileInput = ref<HTMLInputElement | null>(null)

const parser = new XMLParser({
  ignoreAttributes: false,
  attributesGroupName: "_attrs",
  attributeNamePrefix: "",
  textNodeName: "#text"
});

function handleFileChange(event: Event) {
  const target = event.target as HTMLInputElement
  const file = target.files?.[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (e) => {
      const content = e.target?.result as string
      xmlContent.value = content
      try {
        parsedXml.value = parser.parse(content)
      } catch (error) {
        console.error("Error parsing XML:", error)
        parsedXml.value = { error: "Invalid XML file" }
      }
    }
    reader.readAsText(file)
  }
}

function openFileDialog() {
  fileInput.value?.click()
}

function reset() {
  xmlContent.value = null
  parsedXml.value = null
  if(fileInput.value) {
    fileInput.value.value = ''
  }
}
</script>

<template>
  <div class="min-h-screen bg-gray-100 dark:bg-gray-900 text-gray-900 dark:text-gray-100 flex flex-col items-center p-4">
    <div class="w-full max-w-4xl mx-auto">
      <h1 class="text-4xl font-bold text-center mb-8">XML Viewer</h1>
      
      <div class="bg-white dark:bg-gray-800 shadow-xl rounded-lg p-6 mb-6">
        <input type="file" ref="fileInput" @change="handleFileChange" accept=".xml" class="hidden" />
        <button @click="openFileDialog" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg transition-all duration-300 ease-in-out">
          Upload XML File
        </button>
      </div>

      <div v-if="parsedXml" class="bg-white dark:bg-gray-800 shadow-xl rounded-lg p-6">
        <h2 class="text-3xl font-bold mb-6">XML Content</h2>
        <div class="font-mono text-left overflow-auto max-h-[70vh] p-4 bg-gray-200 dark:bg-gray-700 rounded-lg">
          <XmlNode :node="parsedXml" name="Root" :is-root="true" />
        </div>
        <button @click="reset" class="mt-6 w-full bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-6 rounded-lg transition-all duration-300 ease-in-out">
          Upload another file
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>

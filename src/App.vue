<script setup lang="ts">
import { ref } from 'vue'
import { XMLParser } from 'fast-xml-parser'
import XmlTree from './components/XmlTree.vue'
import SelectedNodesTable from './components/SelectedNodesTable.vue'

type ViewState = 'upload' | 'tree' | 'table'

const xmlContent = ref<string | null>(null)
const parsedXml = ref<any>(null)
const fileInput = ref<HTMLInputElement | null>(null)
const viewState = ref<ViewState>('upload')
const selectedNodes = ref<any[]>([])

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
        viewState.value = 'tree'
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

function confirmSelection() {
  viewState.value = 'table'
}

function reset() {
  xmlContent.value = null
  parsedXml.value = null
  selectedNodes.value = []
  viewState.value = 'upload'
  if(fileInput.value) {
    fileInput.value.value = ''
  }
}

function handleNodeSelected(node: any) {
  selectedNodes.value.push(node);
}

function handleNodeDeselected(node: any) {
  selectedNodes.value = selectedNodes.value.filter(n => n !== node);
}
</script>

<template>
  <div class="min-h-screen bg-beige-100 text-gray-800 flex flex-col items-center p-4">
    <div class="w-full max-w-4xl mx-auto">
      <h1 class="text-4xl font-bold text-center mb-8 text-brown-700">Payroll XML Viewer</h1>
      
      <div v-if="viewState === 'upload'" class="bg-white shadow-xl rounded-lg p-6 mb-6">
        <input type="file" ref="fileInput" @change="handleFileChange" accept=".xml" class="hidden" />
        <button @click="openFileDialog" class="w-full bg-orange-500 hover:bg-orange-600 text-white font-bold py-3 px-6 rounded-lg transition-all duration-300 ease-in-out">
          Upload Payroll XML
        </button>
      </div>

      <div v-if="viewState === 'tree'" class="bg-white shadow-xl rounded-lg p-6">
        <h2 class="text-3xl font-bold mb-6 text-brown-700">Select Nodes to Display</h2>
        <div class="font-mono text-left overflow-auto max-h-[70vh] p-4 bg-beige-200 rounded-lg">
          <XmlTree :node="parsedXml" name="Root" :is-root="true" @node-selected="handleNodeSelected" @node-deselected="handleNodeDeselected" />
        </div>
        <div class="flex justify-between mt-6">
          <button @click="reset" class="w-1/3 bg-gray-400 hover:bg-gray-500 text-white font-bold py-3 px-6 rounded-lg transition-all duration-300 ease-in-out">
            Back
          </button>
          <button @click="confirmSelection" class="w-1/3 bg-orange-500 hover:bg-orange-600 text-white font-bold py-3 px-6 rounded-lg transition-all duration-300 ease-in-out">
            Confirm Selection
          </button>
        </div>
      </div>

      <div v-if="viewState === 'table'" class="bg-white shadow-xl rounded-lg p-6">
        <h2 class="text-3xl font-bold mb-6 text-brown-700">Selected Payroll Data</h2>
        <SelectedNodesTable :nodes="selectedNodes" />
        <button @click="reset" class="mt-6 w-full bg-orange-500 hover:bg-orange-600 text-white font-bold py-3 px-6 rounded-lg transition-all duration-300 ease-in-out">
          Upload another file
        </button>
      </div>
    </div>
  </div>
</template>

<style>
.bg-beige-100 { background-color: #f5f5dc; }
.bg-beige-200 { background-color: #eadece; }
.text-brown-700 { color: #5d4037; }
.bg-orange-500 { background-color: #f97316; }
.bg-orange-600 { background-color: #ea580c; }
</style>

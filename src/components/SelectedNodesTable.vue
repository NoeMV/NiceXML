<script setup lang="ts">

const props = defineProps<{
  nodes: any[];
}>()

const getProperties = (node: any) => {
  const properties = { ...node };
  delete properties.name; // Don't display name as a property
  return properties;
}
</script>

<template>
  <div class="overflow-x-auto">
    <table class="min-w-full bg-white border border-gray-300">
      <thead>
        <tr class="bg-orange-200">
          <th class="py-2 px-4 border-b">Node Name</th>
          <th class="py-2 px-4 border-b">Properties</th>
        </tr>
      </thead>
      <tbody>
        <template v-for="(node, index) in nodes" :key="index">
          <tr class="even:bg-beige-100">
            <td class="py-2 px-4 border-b">{{ node.name }}</td>
            <td class="py-2 px-4 border-b">
              <ul>
                <li v-for="(value, key) in getProperties(node)" :key="key">
                  <strong>{{ key }}:</strong> 
                  <pre>{{ JSON.stringify(value, null, 2) }}</pre>
                </li>
              </ul>
            </td>
          </tr>
        </template>
        <tr v-if="nodes.length === 0">
          <td colspan="2" class="py-4 px-4 text-center text-gray-500">No nodes selected.</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.bg-orange-200 { background-color: #ffedd5; }
.bg-beige-100 { background-color: #f5f5dc; }
</style>

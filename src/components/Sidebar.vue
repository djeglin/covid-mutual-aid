<template>
  <aside>
    <button @click="close"><span>Close</span>X</button>
    <h2>Area IDs containing selected point:</h2>
    <ul>
      <li
        v-for="area in filteredAreas"
        :key="area[0]"
        @mouseover="setTargetArea(area)"
        @mouseout="setTargetArea(null)"
      >
        {{ area[0] }}
      </li>
    </ul>
  </aside>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

@Component({
  props: {
    areas: {
      type: Array,
      default: () => [],
    },
    minArea: {
      type: Number,
      required: true,
    },
    maxArea: {
      type: Number,
      required: true,
    },
  },
})
export default class Sidebar extends Vue {
  targetArea = null

  get filteredAreas() {
    if (!this.areas.length) return this.areas
    return this.areas.filter(
      area => area[2] >= this.minArea && area[2] <= this.maxArea
    )
  }

  setTargetArea(area) {
    if (area === null) {
      this.targetArea = null
    } else {
      this.targetArea = area[0]
    }
    this.$emit('setTargetArea', this.targetArea)
  }

  close() {
    this.$emit('close')
  }
}
</script>

<style scoped>
aside {
  @apply w-1/5 bg-white p-4 relative;
}
h2 {
  @apply pt-0 mt-0;
}
ul {
  @apply p-0 mt-8 flex flex-wrap;
}
li {
  @apply block p-1 border-solid border-black border mr-2 mb-2 text-xl font-bold cursor-pointer;
}
li:hover {
  background-color: #00ff0099;
}
button {
  @apply absolute top-0 right-0 mt-4 mr-4 bg-white border-solid border-2 border-black font-bold text-xl cursor-pointer;
  transition: all 0.2s ease;
}
button:hover {
  @apply bg-black text-white;
}
button span {
  @apply hidden;
}
</style>

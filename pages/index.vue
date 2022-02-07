<template>
  <div>
    <downloader :items="items" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api'
import Downloader from '../components/Downloader.vue'

export default defineComponent({
  components: { Downloader },
  setup() {},
  data: () => ({
    items: []
  }),
  async fetch() {
    this.items = await fetch('http://localhost:3000/sample.json').then((res) =>
      res.json()
    )
  }
})
</script>

<style scoped></style>

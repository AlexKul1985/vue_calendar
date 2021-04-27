<template>
  <div id="app">
    <Calendar @change="onChange"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import Calendar from './components/Calendar/CalendarMain.vue'

@Component({
  components: {
    Calendar
  }
})
export default class App extends Vue {
  onChange (range: [number, number]) {
    console.log('Range', range)
  }
}
</script>

<style lang="sass">
</style>

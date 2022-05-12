<template lang="pug">
    .calendar-number( @click="onChangeActive")
        .calendar-number__circle( :class="{'active': isActive}" )
        .calendar-number__num
            | {{number}}

</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component
export default class CalendarDay extends Vue {
    @Prop({
      default: 2
    }) number!: number

    @Prop({
      default: false
    }) isActive!: boolean

    @Prop({
      default: false
    }) canBeMadeActive!: boolean

    get isCanToChangeActive () {
      return !this.canBeMadeActive || this.isActive
    }

    public onChangeActive () {
      if (this.isCanToChangeActive) {
        this.$emit('update:isActive', !this.isActive)
        this.$emit('change')
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
.calendar-number
      position: relative
      color: #A9D3AB
      width: calc(100%/7)
      height: 100%
      cursor: pointer
      &:hover
        .calendar-number__circle
          display: block
      &__num
        position: absolute
        top: 50%
        left: 50%
        transform: translate(-50%,-50%)
        cursor: pointer
      &__circle
        display: none
        position: absolute
        top: 0
        left: 0
        right: 0
        bottom: 0
        border-radius: 50%
        background: #1F4921
        border: 3px solid #2B662E
        box-sizing: border-box
        cursor: pointer
        opacity: 0.2
        &.active
          display: block
          opacity: 1
</style>

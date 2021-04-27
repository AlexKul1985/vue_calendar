<template lang="pug">
  .calendar-header
    .calendar-header__top.top
        .top__current-date.current-date
            .current-date__left
                include icons/arrow_left
            .current-date__text May 2019
            .current-date__right
                include icons/arrow_right

    .calendar-header__bottom.bottom
        .bottom__item( v-for="d in data")
            | {{d}}

</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'

@Component
export default class Calendar extends Vue {
    @Prop({
      default: () => []
    }) data!: Array<string>
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
.calendar-header
  width: 100%
  height: 80px
  font-family: 'Trebuchet MS','IBM Plex Sans', sans-serif
  .top
    height: 50%
    display: flex
    align-items: center
    .current-date
        display: flex
        width: 118px
        height: 24px
        margin: 0 auto
        justify-content: space-around
        align-items: center
        color: #fff
        &__left
            color: #87C289
            cursor: pointer
        &__right
            color: #87C289
            cursor: pointer
  .bottom
      display: flex
      height: 50%
      justify-content: space-around
      align-items: center
      &__item
          font-weight: bold
          font-size: 10px
          text-transform: uppercase
          color: #fff
          text-align: center
</style>

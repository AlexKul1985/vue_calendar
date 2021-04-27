<template lang="pug">
  .calendar
    CalendarHeader( :data="namesDays" )
    .numbers
      .numbers__line(
        v-for="lineDays,indexLine in showLinesDays"
        )
        .select-line(:style="styleActiveLine(indexLine)")
        CalendarDay(
          v-for = "day,indexDay in lineDays"
          :number="day.number"
          :isActive.sync = "day.isActive"
          :canBeMadeActive = "isMoreOrEqualTwoIsActive"
          @change = "addDay(indexLine,indexDay)"
          ).day
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import CalendarHeader from './CalendarHeader.vue'
import CalendarDay from './CalendarDay.vue'

@Component({ components: { CalendarHeader, CalendarDay } })
export default class Calendar extends Vue {
  public namesDays = ['sun', 'mon', 'tue', 'web', 'thu', 'fri', 'sat'];
  public showLinesDays: Array<Array<{
      number: number;
      isActive: boolean;
    }>> = []

  private rangeDays: Array<number> = []
  private readonly COUNT_DAYS_TO_LINE = 7;
  private readonly MAX_ACTIVE_POINT = 2;

  get days () {
    return [28, 29, 30, ...new Array(31).fill(undefined).map((v, i) => i + 1), 1]
  }

  styleActiveLine (indexLine: number) {
    const line = this.mapPointsPoistions[indexLine]
    let result = {}
    if (this.countActiveItems === this.MAX_ACTIVE_POINT) {
      if (this.isOneLine) {
        if (line && line.length) {
          result = {
            width: `calc(${line[1] - line[0] + 1}*100%/${this.COUNT_DAYS_TO_LINE})`,
            left: `calc(${line[0]}*100%/${this.COUNT_DAYS_TO_LINE})`
          }
        } else {
          result = {
            width: 0,
            left: 0
          }
        }
      } else {
        const minIndexLine = this.indexesLines[0]
        const maxIndexLine = this.indexesLines[1]
        if (this.indexesLines.includes(indexLine)) {
          if (indexLine === minIndexLine) {
            result = {
              width: `calc(${this.COUNT_DAYS_TO_LINE - line[0]}*100%/${this.COUNT_DAYS_TO_LINE})`,
              left: `calc(${line[0]}*100%/${this.COUNT_DAYS_TO_LINE})`
            }
          } else if (indexLine === maxIndexLine) {
            result = {
              width: `calc(${line[0] + 1}*100%/${this.COUNT_DAYS_TO_LINE})`,
              left: 0
            }
          }
        } else if (indexLine > minIndexLine && indexLine < maxIndexLine) {
          result = {
            width: `calc(${this.COUNT_DAYS_TO_LINE}*100%/${this.COUNT_DAYS_TO_LINE})`,
            left: 0
          }
        }
      }
    }

    return result
  }

  get isOneLine () {
    return this.indexesLines.length === 1
  }

  get indexesLines () {
    return Object.keys(this.mapPointsPoistions).map(Number)
  }

  get linesDays () {
    const lines: Array<Array<{
      number: number;
      isActive: boolean;
    }>> = []
    for (let i = 0; i < this.days.length; i++) {
      const line: Array<{
      number: number;
      isActive: boolean;
    }> = []
      for (let j = i; j <= i + this.COUNT_DAYS_TO_LINE - 1; j++) {
        const day = this.numberConvertToDayObj(this.days[j])
        line.push(day)
      }
      i = i + this.COUNT_DAYS_TO_LINE - 1
      lines.push(line)
    }
    return lines
  }

  get mapPointsPoistions () {
    const mapPointsPoistions: any = {}
    this.showLinesDays.forEach((line: any, indexLine: number) => {
      const activeIndexes = line.map(
        (day: any, indexDay: number) => ({ isActive: day.isActive, indexDay })
      ).filter(
        (day: any, i: number) => day.isActive
      ).map((day: any) => day.indexDay)
      activeIndexes.forEach((indexDay: number, i: number) => {
        if (!(indexLine in mapPointsPoistions)) {
          mapPointsPoistions[indexLine] = []
        }
        mapPointsPoistions[indexLine].push(indexDay)
      })
    })
    return mapPointsPoistions
  }

  get countActiveItems () {
    let count = 0
    Object.keys(this.mapPointsPoistions).forEach((indexLine: string) => {
      count += this.mapPointsPoistions[indexLine].length
    })
    return count
  }

  get isMoreOrEqualTwoIsActive (): boolean {
    return this.countActiveItems >= this.MAX_ACTIVE_POINT
  }

  public addDay (indexLine: number, indexDay: number) {
    const day = this.showLinesDays[indexLine][indexDay]
    if (day.isActive) {
      this.rangeDays.push(day.number)
    } else {
      const findIndex = this.rangeDays.findIndex((numberDay: number) => day.number === numberDay)
      this.rangeDays.splice(findIndex, 1)
    }
    this.$emit('change', this.rangeDays)
  }

  private numberConvertToDayObj (num: number): {
      number: number;
      isActive: boolean;
    } {
    return {
      number: num,
      isActive: false
    }
  }

  private created () {
    this.showLinesDays = this.linesDays
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
.calendar
  width: 328px
  height: calc(316px + 5*5px)
  background: #000
  font-family:  'Trebuchet MS','IBM Plex Sans', sans-serif
  border-radius: 10px
  padding: 10px
  .numbers
    height: 236px
    &__line
      position: relative
      width: 100%
      height: calc(100%/5)
      display: flex
      margin-bottom: 5px
      .select-line
        position: absolute
        top: 50%
        // left: calc(1*100%/7)
        transform: translateY(-50%)
        height: 100%
        // width: calc(1*100%/7)
        background: #132C14
        border-radius: 40px
        box-sizing: border-box
</style>

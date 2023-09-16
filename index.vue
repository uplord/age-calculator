<template>
  <div class="inner-container">

    <BaseInput label="Date of birth" id="date" type="date" v-model="date" @change="ageCalcualate()" />

    <div class="date">
      <div class="value">{{ years }}</div>
      <div class="value">{{ months }}</div>
      <div class="value">{{ days }}</div>
    </div>

    <BlockAlert type="error" title="Error" :message="error" v-if="error" />

  </div>
</template>

<script>
  export default {
    data() {
      return {
        date: new Date().toISOString().split('T')[0],
        years: '0 Years',
        months: '0 Months',
        days: '0 Days',
        error: ''
      }
    },
    methods: {
      ageCalcualate() {
        this.error = ''

        const today = new Date(),
          birthTime = new Date (this.date).getTime(),
          currentTime = today.getTime()

        if (birthTime > currentTime) {
          this.years = '-'
          this.months = '-'
          this.days = '-'
          this.error = 'This date is in the future'
          return
        }

        const diffTime = currentTime - birthTime

        this.years = Math.floor(diffTime / (1000 * 60 * 60 * 24 * 365))
        this.years += ' Year' + (this.years != 1 ? 's' : '')

        this.months = Math.floor(
          (diffTime % (1000 * 60 * 60 * 24 * 365)) / (1000 * 60 * 60 * 24 * 30.44)
        )
        this.months += ' Month' + (this.months != 1 ? 's' : '')

        this.days = Math.floor(
          (diffTime % (1000 * 60 * 60 * 24 * 30.44)) / (1000 * 60 * 60 * 24)
        )
        this.days += ' Day' + (this.days != 1 ? 's' : '')

      }
    },
    mounted() {
      this.ageCalcualate()
    }
  }
</script>

<style lang="less" scoped>
  @import 'style';
</style>
<template>
    <div class="status-input"><div class="">
    <input  v-model="dayStart" type="number" name="dayStart"  placeholder="Начинается через Х дней." />
    <button @click="defDayStart">Найти</button></div>
    
    <div class="">
        <input v-model="dayEnd" type="number" name="dayEnd" placeholder="заканчивается через Х дней" />
    <button @click="defDayEnd">Найти</button></div>
    <div class="">
        <input v-model="dayOver" type="number" name="dayOver" placeholder="закончилось Х дней назад" />
    <button @click="defDayOver">Найти</button></div>
</div>
</template>

<script>
export default {

    data() {
        return {
            dayStart: '',
            dayEnd: '',
            dayOver: '',
            currentDay: '',
            yesterDay: ''
        }
    },

    methods: {
      defYesterDay() {
        this.$emit('yesterDay', new Date().getTime() + 86400000)
      },
      defCurrentDay() {
        this.$emit('currentDay', new Date().getTime())
      },
      defDayStart() {
     this.$emit('dayStart', this.dayStart)
      },
      defDayEnd() {
     this.$emit('dayEnd', this.dayEnd)
      },
      defDayOver() {
     this.$emit('dayOver', this.dayOver)
      },
    }
}
</script>

<style></style>
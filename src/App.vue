<template>
  <button @click="console.log(this.myJson)">Проверить список</button>
  <button @click="sortStartDays(myJson, startDay)">Сортировать по дате начала.</button>
  <button @click="sortEndDays(myJson, -(endDay))">Сортировать по дате окончания.</button>
  <button @click="sortOverDays(myJson, overDay)">Сортировка по прошедшим диетам.</button>
  <button @click="sortCurrentDay(myJson, console.log('1'))"><span>заканчивается сегодня</span></button>
  <button @click="sortYesterDay(myJson, yesterDays)"><span>заканчивается завтра</span></button>
  <StatusInput :userList="myJson" @dayStart="dayStart" @dayEnd="dayEnd" @dayOver="dayOver"></StatusInput>
  <table cellspacing="10">
    <tr>
      <th>id</th>
      <th>Имя</th>
      <th>Диета</th>
      <th>Тариф</th>
      <th>Адресс</th>
      <th>Телефон</th>
      <th>Дата начала/окончания</th>
      <th>Скидка/сумма/Сколько оплачено</th>
      <th>Статус</th>
      <th>Комментарий курьера</th>
      <th>внутренний комментарий</th>
    </tr>
    <stroke v-for="user in myJson" :key="user.o_id" :user="user"></stroke>
  </table>
</template>

<script>
import json from './data.json'
import Stroke from "@/components/Stroke";
import StatusInput from "@/components/StatusInput"


export default {
  components: {
    Stroke, StatusInput
  },
  data() {
    return {
      counterStart: 0,
      counterEnd: 0,
      counterOver: 0,
      counterCurrent: 0,
      counterYesterday: 0,
      myJson: json,
      startDay: '',
      endDay: '',
      overDay: '',
      currentDays: (new Date().getTime() / (1000 * 60 * 60 * 24)),
      yesterDays: ((new Date().getTime() + 86400000) / (1000 * 60 * 60 * 24)),
    }
  },
  computed: {

  },

  methods: {

    dayStart(dayStart) {
      this.startDay = dayStart
    },
    dayEnd(dayEnd) {
      this.endDay = dayEnd
    },
    dayOver(dayOver) {
      this.overDay = dayOver
    },
    getStartDay(obj) {
      for (let i = 0; i < obj.dates.length; i++) {
        console.log(Math.floor((new Date().getTime() - new Date(obj.dates[i].start_date).getTime()) / (1000 * 60 * 60 * 24)))
        return Math.floor((new Date(obj.dates[i].start_date).getTime() - new Date().getTime()) / (1000 * 60 * 60 * 24))
      }
    },
    getEndDay(obj) {
      for (let i = 0; i < obj.dates.length; i++) {
        console.log(Math.floor((new Date().getTime() - new Date(obj.dates[i].end_date).getTime()) / (1000 * 60 * 60 * 24)))
        return Math.floor((new Date().getTime() - new Date(obj.dates[i].end_date).getTime()) / (1000 * 60 * 60 * 24))
      }
    },
    getOverDay(obj) {
      for (let i = 0; i < obj.dates.length; i++) {
        console.log(Math.floor((new Date(obj.dates[i].end_date).getTime() - new Date().getTime()) / (1000 * 60 * 60 * 24)))
        return Math.floor((new Date(obj.dates[i].end_date).getTime() - new Date().getTime()) / (1000 * 60 * 60 * 24))
      }
    },
    getForCurrent(obj) {
      for (let i = 0; i < obj.dates.length; i++) {
        console.log([new Date(obj.dates[i].end_date).getMonth() + 1, new Date(obj.dates[i].end_date).getDate()].map(v => `0${v}`.slice(-2)).join(''))
        return [new Date(obj.dates[i].end_date).getMonth() + 1, new Date(obj.dates[i].end_date).getDate()].map(v => `0${v}`.slice(-2)).join('')
      }
    },
    sortStartDays(arr, num) {
      num = --num
      console.log(num)
      this.counterStart++
      if (this.counterStart % 2 !== 0) {
        return arr.sort((l, r) =>
          Math.abs(this.getStartDay(l) - num) - Math.abs(this.getStartDay(r) - num));
      } else {
        arr.sort((l, r) =>
          Math.abs(this.getStartDay(l) - num) - Math.abs(this.getStartDay(r) - num));
        return arr.reverse()
      }
    },
    sortEndDays(arr, num) {
      console.log(num)
      this.counterEnd++
      if (this.counterEnd % 2 !== 0) {
        return arr.sort((l, r) =>
          Math.abs(this.getEndDay(l) - num) - Math.abs(this.getEndDay(r) - num));
      } else {
        arr.sort((l, r) =>
          Math.abs(this.getEndDay(l) - num) - Math.abs(this.getEndDay(r) - num));
        return arr.reverse()
      }

    },
    sortOverDays(arr, num) {
      num = -(++num)
      console.log(num)
      this.counterOver++
      if (this.counterOver % 2 !== 0) {
        return arr.sort((l, r) =>
          Math.abs(this.getOverDay(l) - num) - Math.abs(this.getOverDay(r) - num));
      } else {
        arr.sort((l, r) =>
          Math.abs(this.getOverDay(l) - num) - Math.abs(this.getOverDay(r) - num));
        return arr.reverse()
      }
    },
    sortCurrentDay(arr) {
      const dt = new Date();
      const now = +[dt.getMonth() + 1, dt.getDate()].map(v => `0${v}`.slice(-2)).join('');
      console.log(now)
      this.counterCurrent++
      if (this.counterCurrent % 2 !== 0) {
        return arr.sort((l, r) => Math.abs(this.getForCurrent(l).replace(/[^\d]+/, '') - now) - Math.abs(this.getForCurrent(r).replace(/[^\d]+/, '') - now));

      } else {
        arr.sort((l, r) =>
          Math.abs(this.getForCurrent(l).replace(/[^\d]+/, '') - now) - Math.abs(this.getForCurrent(r).replace(/[^\d]+/, '') - now));
        return arr.reverse()
      }
    },
    sortYesterDay(arr, num) {
      const dt = new Date();
      const now = +[dt.getMonth() + 1, dt.getDate() + 1].map(v => `0${v}`.slice(-2)).join('');
      console.log(now)
      this.counterYesterday++
      if (this.counterYesterday % 2 !== 0) {
        return arr.sort((l, r) => Math.abs(this.getForCurrent(l).replace(/[^\d]+/, '') - now) - Math.abs(this.getForCurrent(r).replace(/[^\d]+/, '') - now));
      } else {
        arr.sort((l, r) =>
          Math.abs(this.getForCurrent(l).replace(/[^\d]+/, '') - now) - Math.abs(this.getForCurrent(r).replace(/[^\d]+/, '') - now));
        return arr.reverse()
      }
    },

  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  border: none;
  box-sizing: border-box;
}

input {
  margin-bottom: 25px;
}

button {
  margin-bottom: 25px;
  width: 100px;
  outline: 1px solid red;
}

.status-input {
  display: flex;
  flex-direction: column;
}


tr {
  outline: 2px solid red;
}

th {
  border: 1px solid green;
}

td {
  border: 1px solid green;
}
</style>
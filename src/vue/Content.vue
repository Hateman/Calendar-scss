<template>
<div>
  <article id="calendar">
    <!-- текущая дата -->
    <header>
      <div class="current-date">

        <div class="current-day">
          {{ weekdayNames[currentDay] }}
        </div>

        <div class="today">
          <div><div class="arrow-up" @click="dateUp()"></div></div>
          <div><div class="arrow-up" @click="monthUp()"></div></div>
          <div><div class="arrow-up" @click="currentDate.year += 1"></div></div>
          <div>{{ currentDate.date }}</div>
          <div>{{ month[currentDate.month] }}</div>
          <div>{{ currentDate.year }}</div>
          <div><div class="arrow-down" @click="dateDown()"></div></div>
          <div><div class="arrow-down" @click="monthDown()"></div></div>
          <div><div class="arrow-down" @click="currentDate.year -= 1"></div></div>
        </div>
        
      </div>
    </header>

    <section>
      <!-- названия дней недели -->
      <div class="weekdays">
        <div class="weekday" v-for="(weekday, index) in weekdays" :key="index">
          {{ weekday }}
        </div>
      </div>

      <div class="date">
        <!-- дни пред. месяца -->
        <div class="day-hidden" v-for="(n, index) in (firstMonthDay -1)" :key="'prev'+index">
          {{ (prevMonthDays +1) - firstMonthDay + n }}
        </div>

        <!-- дни текущего месяца -->
        <div class="day" 
            :class="{ active: n === currentDate.date}"
            @click="currentDate.date = n"
            v-for="(n, index) in currentMonthDays" 
            :key="'day'+index">
          {{ n }}
        </div>

        <!-- дни след месяца -->
        <div class="day-hidden" v-for="(n, index) in (43 - (currentMonthDays + firstMonthDay))" :key="'next'+index">
          {{ n }}
        </div>
      </div>

    </section>
  </article>
</div>
</template>

<script>
  export default {
    data: function() {
      return {
        weekdays: ['Mo','Tu','We','Th','Fr','Sa','Su'],
        weekdayNames: ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'],
        month: ['January','February','March','April','May','June','July','August','September','October','November','December'],
        currentDate: {
          date: 0,
          month: 0,
          year: 0
        }
      }
    },
    computed: {
      prevMonthDays() {
        let year = this.currentDate.month === 0 ? this.currentDate.year - 1 : this.currentDate.year;
        let month = this.currentDate.month === 0 ? 12 : this.currentDate.month;
        return new Date(year, month, 0).getDate();
      },
      firstMonthDay() {
        let firstDay = new Date(this.currentDate.year, this.currentDate.month, 1).getDay();
        if(firstDay === 0) firstDay = 7;
        return firstDay;
      },
      currentDay() {
        return new Date(this.currentDate.year, this.currentDate.month, this.currentDate.date).getDay();
      },
      currentMonthDays() {
        return new Date(this.currentDate.year, this.currentDate.month +1, 0).getDate();
      }
    },
    methods: {
      getCurrentDate() {
        let today = new Date();
        this.currentDate.date = today.getDate();
        this.currentDate.month = today.getMonth();
        this.currentDate.year = today.getFullYear();
      },
      dateUp() {
        if(this.currentDate.date === this.currentMonthDays) {
          this.currentDate.date = 1;
          this.monthUp();
        }
        else {
          this.currentDate.date += 1;
        }
      },
      dateDown() {
        if(this.currentDate.date === 1) {
          this.currentDate.date = this.prevMonthDays;
          this.monthDown();
        }
        else {
          this.currentDate.date -= 1;
        }
      },
      monthUp() {
        if(this.currentDate.month === 11) {
          this.currentDate.month = 0;
          this.currentDate.year += 1;
        }
        else {
          this.currentDate.month += 1;
        }        
      },
      monthDown() {
        if(this.currentDate.month === 0) {
          this.currentDate.month = 11;
          this.currentDate.year -= 1;
        }
        else {
          this.currentDate.month -= 1;
        } 
      }
    },
    created() {
      this.getCurrentDate();
    }
  }
</script>

<style lang="scss" src="./cont.scss"></style>

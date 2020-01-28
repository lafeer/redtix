<template>
  <div class="calendar">
    <div class="weekdays">
      <div v-for="(day, i) in days" v-bind:key="day + i">{{day}}</div>
    </div>
    <!-- blank cards before start of current month-->
    <div class="calendar-title">
      <i class="chevron-left" @click="subtractMonth"></i>
      <h4>{{fullMonth + ' ' + year}}</h4>
      <i class="chevron-right" @click="addMonth"></i>
    </div>

    <!-- date cards for current month -->
    <div class="dates">
      <div
        v-for="blank in firstDayOfMonth"
        v-bind:key="'0'+blank"
        class="event-date blank"
      >
      </div>
      <div
        v-for="eventDate in eventDays"
        v-bind:key="eventDate.date"
        class="event-date"
        :class="{
          'current-day': getDayOfMonth(eventDate.date) == initialDate
            && month == initialMonth
            && year == initialYear,
          'sold-out': eventDate.soldOut
        }"
      >
        <div
          class="event-date__wrapper"
          :class="{
            'unavailable': !eventDate.showtimes
            || eventDate.soldOut,
          }"
        >
          <div class="date-header">
            <span class="day">{{getDayOfMonth(eventDate.date)}}</span>
            <div class="day-full">
              <span class="date">{{getDateOfMonth(eventDate.date)}}</span>
              <span class="month">{{` ${month}`}}</span>
              <span class="unavailable" v-show="!eventDate.showtimes">
                Unavailable
              </span>
            </div>
          </div>
          <div class="date-content">
            <button
              v-for="showtime in eventDate.showtimes"
              v-bind:key="showtime"
              class="btn btn-showtime"
              :class="{
                'btn-selected': selectedShow
                  && selectedShow.date === eventDate.date
                  && selectedShow.showtime === showtime
              }"
              :disabled="eventDate.soldOut"
              @click="$emit('select-show', eventDate, showtime)"
            >
              {{formatShowtime(showtime)}}
            </button>
            <span class="unavailable" v-show="!eventDate.showtimes">
              Unavailable
            </span>
            <button class="btn btn-soldout" v-if="eventDate.soldOut">SOLD OUT</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  name: 'Calendar',
  props: {
    eventDates: Array,
    selectedShow: Object,
  },
  data() {
    return {
      today: moment(),
      dateContext: moment(),
      days: ['S', 'M', 'T', 'W', 'T', 'F', 'S'],
    };
  },
  computed: {
    year() {
      const t = this;
      return t.dateContext.format('Y');
    },
    month() {
      const t = this;
      return t.dateContext.format('MMM');
    },
    fullMonth() {
      const t = this;
      return t.dateContext.format('MMMM');
    },
    daysInMonth() {
      const t = this;
      return t.dateContext.daysInMonth();
    },
    currentDate() {
      const t = this;
      return t.dateContext.get('date');
    },
    firstDayOfMonth() {
      const t = this;
      const firstDay = moment(t.dateContext).subtract(
        t.currentDate - 1,
        'days',
      );
      return firstDay.weekday();
    },
    initialDate() {
      const t = this;
      return t.today.get('date');
    },
    initialMonth() {
      const t = this;
      return t.today.format('MMM');
    },
    initialYear() {
      const t = this;
      return t.today.format('Y');
    },
    eventDays() {
      const t = this;
      const arr = Array.from(Array(t.daysInMonth), (monthDay, i) => {
        const date = moment(`${i + 1}-${t.month}-${t.year}`, 'D-MMM-Y').format('DD-MM-YYYY');
        const day = t.eventDates.find(eventDate => eventDate.date === date);
        return {
          ...(day && day),
          date,
        };
      });
      return arr;
    },
  },
  methods: {
    addMonth() {
      const t = this;
      t.dateContext = moment(t.dateContext).add(1, 'month');
    },
    subtractMonth() {
      const t = this;
      t.dateContext = moment(t.dateContext).subtract(1, 'month');
    },
    getDayOfMonth(date) {
      return moment(date, 'DD-MM-YYY').format('ddd');
    },
    getDateOfMonth(date) {
      return moment(date, 'DD-MM-YYY').format('D');
    },
    formatShowtime(showtime) {
      return moment(showtime, 'HH:mm').format('h:mm a');
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.calendar-title {
  display: none;
}

.weekdays {
  color: rgba(29, 29, 29, 0.45);
  font-weight: 800;
  font-size: 16px;
  text-transform: uppercase;
  text-align: left;
  display: none;
}

.dates {
  @include calendar-layout($grid-template-columns: minmax(50px, 1fr), $grid-gap: 0);
  box-shadow: 0px 0px 14px rgba(29, 29, 29, 0.08);

  .event-date {
    background: #FFFFFF;
    border-radius: 0;

    &:not(:last-child) {
      border-bottom: 1px solid rgba(29, 29, 29, 0.2);
    }

    &.blank {
      background: 0;
      border: 0;
      box-shadow: none;
      cursor: initial;
    }

    &__wrapper {
      display: flex;
      min-height: 70px;

      &.unavailable {
        .day, .date, .month {
          opacity: 0.35;
        }
      }
    }

    &.sold-out {
      .btn-showtime {
        display: none;
      }

      .btn-soldout {
        background-color: #EB5757;
        color: white;
        cursor: initial;
        font-weight: 600;
      }
    }
  }

  .date-header {
    padding: 10px;
    border-right: 1px solid rgba(29, 29, 29, 0.2);
    text-align: center;
    min-width: 80px;
    display: flex;
    flex-flow: column wrap;
    align-items: center;

    .day {
      flex: 1 0 auto;
      font-size: 14px;
    }

    .date, .month {
      font-size: 18px;
      font-weight: 600;
    }

    .unavailable {
      display: none;
      font-size: 12px;
      color: rgba(29, 29, 29, 0.45);
    }
  }

  .date-content {
    padding: 10px;
    flex: 1;
    display: flex;
    align-items: center;

    .unavailable {
      color: rgba(29, 29, 29, 0.45);
      font-size: 14px;
    }
  }

  .btn-showtime {
    width: auto;
    display: inline-block;
    margin: 5px;

    &.btn-selected, &:hover:not(:disabled) {
      background: linear-gradient(0deg, rgba(241, 59, 84, 0.6), rgba(241, 59, 84, 0.6));
      border: 0;
      box-shadow: 0px 5px 16px rgba(29, 29, 29, 0.14);
      color: #ffffff;
    }
  }
}

@include breakpoint($md-device) {
  .calendar-title {
    display: block;
  }
  .weekdays {
    @include calendar-layout();
    border-bottom: 1px solid #fff;
    font-size: 18px;
    font-weight: 400;
    text-align: center;
  }
  .dates {
    @include calendar-layout();
    box-shadow: none;

    .event-date {
      box-shadow: 0px 0px 25px rgba(29, 29, 29, 0.08);
      border-bottom: 0 !important;
      border-radius: 3px;
      overflow: hidden;
      position: relative;

      &__wrapper {
        display: block;
        height: auto;

        &.unavailable {
          display: block;
          opacity: 0.35;

          .day, .date, .month {
            opacity: 1;
          }
        }
      }

      &.sold-out {
        .btn-showtime {
          display: block;
        }

        .btn-soldout {
          display: none;
        };

        &:after {
          content: 'SOLD OUT';
          font-size: 16px;
          font-weight: 900;
          padding: 6px 0;
          color:rgba(241, 59, 84, 0.8);
          border-top: 1px solid rgba(241, 59, 84, 0.8);
          border-bottom: 1px solid rgba(241, 59, 84, 0.8);
          position: absolute;
          text-align: center;
          width: 150%;
          transform: translate(-50%, -50%) rotate(-45deg);
          left: 50%;
          top: 50%;
        }
      }
    }

    .date-header {
      border-right: 0;
      flex-flow: row nowrap;
      justify-content: space-between;

      .day, .month {
        display: none;
      }

      .day-full {
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
      }

      .unavailable {
        display: block;
      }
    }

    .date-content {
      display: block;

      .unavailable {
        display: none;
      }
    }

    .btn-showtime {
      width: 100%;
      display: block;
      margin: 5px 0;
    }
  }
}
</style>

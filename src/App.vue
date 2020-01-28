<template>
  <div id="app">
    <main>
      <h1>Pick a date &amp; time</h1>
      <Calendar :event-dates="eventDates" @select-show="setSelectedShow" :selected-show="selectedShow" />
    </main>
    <aside>
      <EventCard :event-details="eventDetails" :selected-show="selectedShow" />
    </aside>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue';
import EventCard from './components/EventCard.vue';

export default {
  name: 'app',
  components: {
    Calendar,
    EventCard,
  },
  data() {
    return {
      selectedShow: null,
      eventDetails: {
        name: 'Mean Girls Theatre Kuala Lumpur',
        dates: '4th Jan 2020 to 26th Jan 2020',
        venue: 'Istana Budaya, Kuala Lumpur, Malaysia',
        tags: ['Concert', 'Pop'],
        socials: {
          website: '',
          twitter: '',
          facebook: '',
          whatsapp: 'https://airasiaredtix.com',
        },
      },
      eventDates: [
        {
          date: '04-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '05-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '06-01-2020',
          showtimes: ['20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '09-01-2020',
          showtimes: ['20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '10-01-2020',
          showtimes: ['20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '11-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '12-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '14-01-2020',
          showtimes: ['20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '16-01-2020',
          showtimes: ['20:00'],
          priceText: 'RM 200',
          soldOut: true,
        },
        {
          date: '17-01-2020',
          showtimes: ['20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '18-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '19-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '20-01-2020',
          showtimes: ['14:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '22-01-2020',
          showtimes: ['20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '24-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
        {
          date: '26-01-2020',
          showtimes: ['14:00', '20:00'],
          priceText: 'RM 200',
          soldOut: false,
        },
      ],
    };
  },
  methods: {
    setSelectedShow(show, showtime) {
      const t = this;
      t.selectedShow = {
        ...show,
        showtime,
      };
    },
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto');
</style>

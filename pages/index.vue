    <template>
      <div>
        <h1>Events</h1>
        <EventCard
          v-for="(event, index) in events"
          :key="index"
          :event="event"
          :data-index="index"
        />
      </div>
    </template>
    <script>
    import EventCard from '@/components/EventCard.vue'
    import { mapState } from 'vuex'  // <--- To map event
    export default {
      head() {
        return {
          title: 'Event Listing'
        }
      },
      // two way can do this
      // asyncData({ $axios, error }) {
      //   return $axios.get('http://localhost:3000/events').then(response => {
      //     return {
      //       events: response.data
      //     }
      //   }).catch(e => {
      //     error({ statusCode: 503, message: 'Unable to fetch events at this time, please try again' })
      //   })
      // },
      // both is same as above
      // Using ES6 destructuring we can simplify this to:
      async fetch({ store, error }) {
        try {
          await store.dispatch('events/fetchEvents')
        } catch (e) {
          error({
            statusCode: 503,
            message: 'Unable to fetch events events at this time'
          })
        }
      },
      components: {
        EventCard
      },
      computed: mapState({
        events: state => state.events.events
      })
    }
    </script>
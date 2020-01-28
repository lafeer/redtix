<template>
  <div class="event-card">
    <div class="event-card__header">
      <div class="event-card__details">
        <h2>{{eventDetails.name}}</h2>
        <p class="event-detail">
          <icon-base class="icon">
            <icon-calendar />
          </icon-base>
          {{eventDetails.dates}}
        </p>
        <p class="event-detail">
          <icon-base class="icon" width="18" height="18" view-box="0 0 18 18">
            <icon-location />
          </icon-base>
          {{eventDetails.venue}}</p>
        <button
          v-for="tag in eventDetails.tags"
          v-bind:key="tag"
          class="btn btn--inline tag"
        >
          {{tag}}
        </button>
        <a class="social-share--mobile">
          <icon-base width="19" height="22" view-box="0 0 19 22"><icon-share /></icon-base>
        </a>
      </div>
      <div class="social-share">
        <a
          v-for="(link, social) in eventDetails.socials"
          v-bind:key="social"
          :href="link"
          class="social-link"
        >
          <icon-base width="22" height="22" view-box="0 0 22 22">
            <icon-link v-if="social === 'website'"/>
            <icon-twitter v-if="social === 'twitter'"/>
            <icon-facebook v-if="social === 'facebook'"/>
            <icon-whatsapp v-if="social === 'whatsapp'"/>
          </icon-base>
        </a>
      </div>
    </div>
    <div class="event-card__footer">
      <h3 class="event-ticket">
        <span class="icon">
          <icon-base width="15" height="15" view-box="0 0 15 15">
            <icon-ticket />
          </icon-base>
        </span>
        Event Tickets
      </h3>
      <div class="row--total">
        <span class="label h4">Total</span>
        <span class="total">{{selectedShow ? selectedShow.priceText : 'RM 00.00'}}</span>
      </div>
      <button
        class="btn btn-submit"
        :disabled="!selectedShow"
      >
        Checkout
      </button>
    </div>
  </div>
</template>

<script>
import IconBase from './IconBase.vue';
import IconCalendar from './icons/IconCalendar.vue';
import IconFacebook from './icons/IconFacebook.vue';
import IconLink from './icons/IconLink.vue';
import IconLocation from './icons/IconLocation.vue';
import IconShare from './icons/IconShare.vue';
import IconTwitter from './icons/IconTwitter.vue';
import IconWhatsapp from './icons/IconWhatsapp.vue';
import IconTicket from './icons/IconTicket.vue';

export default {
  name: 'EventCard',
  props: {
    eventDetails: Object,
    total: Number,
    selectedShow: Object,
  },
  components: {
    IconBase,
    IconCalendar,
    IconFacebook,
    IconLink,
    IconLocation,
    IconShare,
    IconTicket,
    IconTwitter,
    IconWhatsapp,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.event-card {
  border-radius: 5px;

  &__details {
    padding: 0;
  }

  &__footer {
    display: none;
    padding: 20px;
  }
}

.event-detail {
  font-size: 14px;
  line-height: 21px;
  display: flex;
  align-items: center;
}

.icon {
  margin-right: 10px;
}

.tag {
  color: #C4113E;
  border-color: #C4113E;
  box-shadow: none;
  margin-right: 8px;
  font-weight: 600;
  cursor: initial;

  &:first-of-type {
    background-color: #56CCF2;
    color: #FFFFFF;
    border-color: #56CCF2;
  }
}

.event-ticket {
  display: flex;
  align-items: center;
}

.social-share {
  display: none;
  justify-content: center;
  margin-top: 20px;
  padding: 10px;
  box-shadow: 0px 5px 20px rgba(29, 29, 29, 0.1);

  &--mobile {
    position: relative;
    float: right;
    margin: 5px 0;
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background-color: #FFFFFF;
    box-shadow: 0px 4px 30px rgba(29, 29, 29, 0.2);

    .icon {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
  }
}

.social-link {
  margin: 0 10px;
}

.row--total {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid rgba(29, 29, 29, 0.1);
  border-bottom: 1px solid rgba(29, 29, 29, 0.1);
  padding: 10px 0;

  .total {
    color: #1D1D1D;
    font-size: 26px;
    font-weight: 900;
  }
}

.btn-submit {
  background: linear-gradient(180deg, #F13B54 0%, #C33A8C 100%);
  border: 0;
  border-radius: 3px;
  height: 48px;
  color: #FFFFFF;
  font-weight: 500;
  font-size: 18px;
  margin-top: 16px;

  &:disabled {
    background: linear-gradient(0deg, rgba(29, 29, 29, 0.2), rgba(29, 29, 29, 0.2));
  }
}

@include breakpoint($md-device) {
  .event-card {
    background: #FFFFFF;
    box-shadow: 0px 5px 20px rgba(29, 29, 29, 0.1);

    &__footer {
      display: block;
    }
    &__header {
      box-shadow: 0px 5px 20px rgba(29, 29, 29, 0.1);
    }
    &__details {
      padding: 20px 20px 0;
    }
  }

  .social-share {
    display: flex;

    &--mobile {
      display: none;
    }
  }
}
</style>

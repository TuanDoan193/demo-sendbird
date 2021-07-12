<template>
  <div class="channel">

    <channel-users v-if="channel"/>

    <messages v-if="channel"/>

    <message-sender/>

  </div>
</template>

<script>

import sendBird from '@/services/SendBird.js'
import Messages from '@/components/Messages'
import MessageSender from '@/components/MessageSender'
import ChannelUsers from '@/components/ChannelUsers'
import { mapState } from 'vuex'

export default {
  name: 'Channel',

  components: {
    Messages,
    MessageSender,
    ChannelUsers
  },

  computed: {
    ...mapState([
      'channel'
    ])
  },

  watch: {
    channel: {
      handler (newValue) {
        this.init(newValue.url)
      }
    }
  },

  mounted () {

    this.init('sendbird_open_channel_4406_5684198a9fee5a42b4602c8421b7ad621e8ee0e7')

  },

  methods: {
    init (url) {

      sendBird
        .getChannel(url)
        .then((channel) => {
          this.$store.commit('SET_CHANNEL', channel)
        })
        .catch((error) => {
          console.error(error)
        })

    }
  }

}
</script>

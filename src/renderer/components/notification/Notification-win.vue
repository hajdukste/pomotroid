<template></template>

<script>
import { EventBus } from '@/utils/EventBus'
import { logger } from '@/utils/logger'

const notifier = require('node-notifier')
const path = require('path')

export default {
  name: 'Notification-win',

  computed: {
    timeWork() {
      return this.$store.getters.timeWork
    }
  },

  methods: {
    callNotification(opts) {
      notifier.notify(
        {
          appName: 'com.splode.pomotroid',
          title: opts.title || 'Focus Round Complete',
          message: opts.message,
          icon: opts.icon || path.join(__static, 'icon.png'),
          sound: false
        },
        (err, res) => {
          if (err) {
            logger.error(err)
          }
        }
      )
    },

    notifyWork() {
      this.callNotification({
        title: 'Break Finished',
        message: `Begin focusing for ${this.timeWork} minutes.`
      })
    }
  },

  mounted() {
    EventBus.$on('ready-work', this.notifyWork)
  },

  beforeDestroy() {
    EventBus.$off('ready-work', this.notifyWork)
  }
}
</script>

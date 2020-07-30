<template>
  <div
    :data-sitekey="$recaptcha2.siteKey"
    :data-size="$recaptcha2.size || dataSize"
    :data-theme="dataTheme"
    :data-badge="dataBadge"
    :data-tabindex="dataTabindex"

    data-callback="recaptchaSuccessCallback"
    data-expired-callback="recaptchaExpiredCallback"
    data-error-callback="recaptchaErrorCallback"
    class="g-recaptcha"
  />
</template>

<script>
export default {
  beforeDestroy () {
    this.$recaptcha2.destroy()
  },

  methods: {
    onError (message) {
      return this.$emit('error', message)
    },

    onSuccess (token) {
      return this.$emit('success', token)
    },

    onExpired () {
      return this.$emit('expired')
    }
  },

  mounted () {
    this.$recaptcha2.init()

    this.$recaptcha2.on('recaptcha-error', this.onError)
    this.$recaptcha2.on('recaptcha-success', this.onSuccess)
    this.$recaptcha2.on('recaptcha-expired', this.onExpired)
  },

  props: {
    dataTheme: {
      default: 'light',
      type: String,

      validator: value => {
        return ['dark', 'light'].includes(value)
      }
    },

    dataSize: {
      default: 'normal',
      type: String,

      validator: value => {
        return ['compact', 'normal', 'invisible'].includes(value)
      }
    },
    
    dataBadge: {
      default: 'bottomright',
      type: String,
      
      validator: value => {
        return ['bottomright', 'bottomleft', 'inline'].includes(value)
      }
    },
    
    dataTabindex: {
      default: 0,
      type: Number
    }
  }
}
</script>

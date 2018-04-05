<template>
  <div class="cookie-message" :class="containerPosition" v-if="isOpen">
    <div class="cookie-message__content">
      <slot name="cookie-message__message">
        {{ cookieMessage }}
      </slot>
    </div>
    <div class="cookie-message__buttons">
      <div :class="cookieButtonClass" @click="acceptGDPR">
        {{ cookieButtonText }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cookieButtonText: {
      type: String,
      default: 'Accept'
    },
    cookieMessage: {
      type: String,
      default: 'We use cookies to improve your user experience on this website.'
    },
    cookiePosition: {
      type: String,
      default: 'bottom'
    },
    cookieButtonClass: {
      type: String,
      default: 'cookie-message__button'
    }
  },
  data() {
    return {
      isOpen: false
    }
  },
  computed: {
    containerPosition() {
      return `cookie-message__${this.cookiePosition}`
    }
  },
  created() {
    if (!this.getAcceptStatus() === true) {
      this.isOpen = true
    }
  },
  methods: {
    setAccept() {
      localStorage.setItem('cookie:accepted', true)
    },
    getAcceptStatus() {
      return localStorage.getItem('cookie:accepted')
    },
    acceptGDPR() {
      this.setAccept()
      this.isOpen = false
      this.$emit('accept')
    }
  }
}
</script>

<style>
  .cookie-message {
    position: fixed;
    overflow: hidden;
    box-sizing: border-box;
    z-index: 1;
    width: 100%;
    background: #EEE;
    color: #FFF;
    padding: 1.250em 0;
  }

  .cookie-message__top {
    top: 0;
    left: 0;
    right: 0;
  }

  .cookie-message__bottom {
    bottom: 0;
    left: 0;
    right: 0;
  }
  .cookie-message__button {
    cursor: pointer;
    text-align: center;
    background: red;
    padding: 10px;
    border: none;
    border-radius: 0;
  }
  .cookie-message__button:hover {
    background: red;
    color: white;
  }
</style>

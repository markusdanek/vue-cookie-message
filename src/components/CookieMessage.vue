<template>
  <div class="cookie-message" :class="containerPosition" v-if="isOpen">
    <div class="cookie-message__content item">
      <slot name="cookie-message__text">
        {{ cookieMessage }}
      </slot>
    </div>
    <div class="cookie-message__buttons item">
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
      localStorage.setItem('gdpr:accepted', true);
    },
    getAcceptStatus() {
      return localStorage.getItem('gdpr:accepted');
    },
    acceptGDPR() {
      this.setAccept();
      this.isOpen = false;
      this.$emit('accept');
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
    background: #000;
    color: #FFF;
    padding: 1.250em 0;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: center;
    align-content: center;
    align-items: center;
  }

  .item {
    -webkit-order: 0;
    -ms-flex-order: 0;
    order: 0;
    -webkit-flex: 0 1 auto;
    -ms-flex: 0 1 auto;
    flex: 0 1 auto;
    -webkit-align-self: center;
    -ms-flex-item-align: center;
    align-self: center;
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
    margin-left: 20px;
  }
  .cookie-message__button:hover {
    background: red;
    color: white;
  }
  @media only screen and (max-width: 600px) {
    .cookie-message {
      flex-direction: column;
    }
    .item {
      margin-bottom: 20px;
    }
  }
</style>

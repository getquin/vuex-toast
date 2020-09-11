<template>
  <div class="toast-message" :class="messageTypeClass(m)" :key="m.id" role="note">
    <div class="toast-message-text" v-if="html" v-html="m.text"></div>
    <div class="toast-message-text" v-else>{{ m.text }}</div>
    <button class="toast-button" aria-label="Close" type="button" @click="close(m.id)"></button>
  </div>
</template>

<script>
import { REMOVE_TOAST_MESSAGE } from '../module'

export default {
  name: 'ToastMessage',
  props: {
    m: Object,
    html: Boolean
  },

  methods: {
    close(id) {
      this.$store.dispatch(
        this.normalizedNamespace + REMOVE_TOAST_MESSAGE,
        id
      )
    },

    messageTypeClass(message) {
      return `toast-type-${message.type}`
    }
  }
}
</script>

<style lang="scss">
.toast-message {
  position: relative;
  box-sizing: border-box;
  margin-bottom: 10px;
  padding: 15px;
  width: 100%;
  color: #fff;
  transition: 400ms cubic-bezier(0.17, 0.67, 0.17, 0.98);
  transition-property: opacity, transform;
}

.toast-button {
  position: absolute;
  top: 0;
  right: 5px;
  padding: 2px;
  background-color: transparent;
  border-width: 0;
  font-size: 1.5em;
  color: inherit;
  cursor: pointer;
}

.toast-button::before {
  content: '\d7';
}
</style>

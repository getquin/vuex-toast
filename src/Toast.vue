<template>
  <transition-group tag="div" name="toast" class="toast">
    <div class="toast-message" v-for="m in messages" :key="m.id" role="note">
      <div class="toast-message-text">{{ m.text }}</div>
      <button class="toast-button" aria-label="Close" type="button" @click="close(m.id)"></button>
    </div>
  </transition-group>
</template>

<script>
import Vue from 'vue'
import { mapGetters, mapActions } from 'vuex'
import { REMOVE_TOAST_MESSAGE } from './module'

export default {
  computed: mapGetters({
    messages: 'toastMessages'
  }),

  methods: mapActions({
    close: REMOVE_TOAST_MESSAGE
  }),

  transitions: {
    toast: {
      beforeEnter(el) {
        el.style.maxHeight = '0'
      },
      enter(el) {
        Vue.nextTick(() => {
          el.style.maxHeight = `${el.scrollHeight}px`
        })
      },
      beforeLeave(el) {
        el.style.maxHeight = `${el.scrollHeight}px`
      },
      leave(el) {
        Vue.nextTick(() => {
          el.style.maxHeight = '0'
        })
      }
    }
  }
}
</script>

<style>
.toast {
  position: fixed;
  top: 10px;
  right: 10px;
  width: 350px;
  z-index: 10000;
}

.toast-message {
  position: relative;
  box-sizing: border-box;
  margin-bottom: 10px;
  padding: 15px;
  width: 100%;
  background-color: #666;
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

.toast-enter-active,
.toast-leave {
  opacity: 1;
}

.toast-enter,
.toast-leave-active {
  opacity: 0;
  transform: translateY(-30px);
}

.toast-leave-active {
  position: absolute;
}
</style>
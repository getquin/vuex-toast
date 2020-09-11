<template>
  <div class="toast" :class="positionClass">
    <toast-transition>
      <ToastMessage
        v-for="m in messages"
        :m="m"
        :html="html"
        :key="m.id"
        :close="close"
      />
    </toast-transition>
  </div>
</template>

<script>
import { REMOVE_TOAST_MESSAGE } from './module'
import { DefaultTransition as ToastTransition } from './config'
import ToastMessage from './components/ToastMessage'

export default {
  props: {
    position: {
      validator(value) {
        return /^(:?n|s|nw|ne|sw|se)$/.test(value)
      },
      default: 'ne'
    },

    namespace: {
      type: String,
      default: ''
    },

    html: Boolean
  },

  computed: {
    messages() {
      return this.$store.getters[this.normalizedNamespace + 'toastMessages']
    },

    positionClass() {
      return `toast-position-${this.position}`
    },

    normalizedNamespace() {
      if (this.namespace === '') {
        return ''
      }

      if (!/\/$/.test(this.namespace)) {
        return this.namespace + '/'
      }

      return this.namespace
    }
  },

  methods: {
    close(id) {
      this.$store.dispatch(
        this.normalizedNamespace + REMOVE_TOAST_MESSAGE,
        id
      )
    },

  },

  components: {
    ToastTransition,
    ToastMessage
  }
}
</script>

<style lang="scss">
$width: 350px;

.toast {
  position: fixed;
  width: $width;
  z-index: 10000;
}

/**
 * Transition
 */
.toast-enter-active,
.toast-leave {
  opacity: 1;
}

.toast-enter,
.toast-leave-active {
  opacity: 0;
}

.toast-leave-active {
  position: absolute;
}

/**
 * Position
 */
.toast-position-n {
  top: 10px;
  left: 50%;
  margin-left: -$width / 2;
}

.toast-position-s {
  bottom: 10px;
  left: 50%;
  margin-left: -$width / 2;
}

.toast-position-ne {
  top: 10px;
  right: 10px;
}

.toast-position-nw {
  top: 10px;
  left: 10px;
}

.toast-position-se {
  bottom: 10px;
  right: 10px;
}

.toast-position-sw {
  bottom: 10px;
  left: 10px;
}

/**
 * Transition with position
 */
.toast-position-n,
.toast-position-ne,
.toast-position-nw {
  .toast-enter,
  .toast-leave-active {
    transform: translateY(-20px);
  }
}

.toast-position-s,
.toast-position-se,
.toast-position-sw {
  .toast-enter {
    transform: translateY(20px);
  }

  .toast-leave-active {
    transform: translateY(-100%) translateY(20px);
  }
}

/**
 * Types
 */
.toast-type-info {
  background-color: #43b4ec;
}

.toast-type-success {
  background-color: #3add93;
}

.toast-type-warning {
  background-color: #efd700;
}

.toast-type-danger {
  background-color: #f3755e;
}
</style>

<template>
  <div>
    <button @click="showToast = true">Show Toast</button>
    <app-toast message="Message has been sent" position="top-right" :show="showToast" @hide="showToast = false"></app-toast>
  </div>
</template>

<script>
import Toast from './components/toast/Toast.vue';

export default {
  data() {
    return {
      showToast: false
    }
  },
  components: {
    appToast: Toast
  }
}
</script>

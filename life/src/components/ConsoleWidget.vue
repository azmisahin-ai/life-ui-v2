<template>
  <div class="console-widget">
    <h4>{{ title }}</h4>
    <div class="console-messages" ref="consoleMessages" style="height: 100px; overflow: hidden;">
      <div v-for="(item, index) in messages" :key="index" class="console-message" :class="item.type">
        {{ item.message }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    data: Array // Assuming data is an array of message objects
  },
  computed: {
    messages() {
      return this.data || [];
    }
  },
  watch: {
    data(newVal) {
      this.$nextTick(() => {
        this.scrollToBottom();
      });
    }
  },
  mounted() {
    this.scrollToBottom(); // Component is mounted, scroll to bottom
  },
  updated() {
    this.scrollToBottom(); // Component is updated, scroll to bottom
  },
  methods: {
    scrollToBottom() {
      const consoleMessages = this.$refs.consoleMessages;
      consoleMessages.scrollTop = consoleMessages.scrollHeight;
    }
  }
};
</script>

<style scoped>
.console-widget {
  background-color: #424242;
  color: white;
  padding: 10px;
  border-radius: 5px;
}

.console-message {
  word-wrap: break-word;
  /* Uzun kelimeleri otomatik olarak kır */
  max-width: 100%;
  /* En fazla genişlik belirle */
  overflow: hidden;
  /* Taşan içerikleri gizle */



}

.console-message {
  margin-bottom: 5px;
  padding: 5px;
  border-radius: 3px;
  width: 100%;
}

.info {
  color: #3dd2ecc0;
}

.warning {
  color: #e1b42ec8;
}

h4 {
  margin: 0.1em;
}
</style>

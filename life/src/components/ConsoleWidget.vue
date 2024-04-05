<!-- ConsoleWidget.vue -->

<template>
  <div class="console-widget">
    <h3>{{ title }}</h3>
    <div class="console-messages" ref="consoleMessages" style="height: 100%; overflow-y: auto;">
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

  mounted() {
    // Scroll to bottom when component is mounted or data changes
    this.scrollToBottom();
  },
  updated() {
    // Scroll to bottom when component is updated (e.g., new messages added)
    this.scrollToBottom();
  },
  methods: {
    scrollToBottom() {
      this.$refs.consoleMessages.scrollTop = this.$refs.consoleMessages.scrollHeight;
    }
  }
};
</script>

<style scoped>
.console-widget {
  background-color: #333;
  color: white;
  padding: 10px;
  border-radius: 5px;
}

.console-messages {
  height: 100%;
  overflow-y: auto;
}

.console-message {
  margin-bottom: 5px;
  padding: 5px;
  border-radius: 3px;
}

.info {
  color: #3e88d696;
}

.warning {
  color: #e1b42e8c;
}

/* Add more styles for different message types if needed */
</style>
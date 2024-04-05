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
    this.scrollToBottom(); // Component is mounted, scroll to bottom
  },
  updated() {
    this.scrollToBottom(); // Component is updated, scroll to bottom
  },
  methods: {
    scrollToBottom() {
      // Access the consoleMessages element and scroll to the bottom
      const consoleMessages = this.$refs.consoleMessages;
      consoleMessages.scrollTop = consoleMessages.scrollHeight;
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

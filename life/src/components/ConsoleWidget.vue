<!-- ConsoleWidget.vue -->

<template>
    <div class="console-widget">
      <h3>{{ title }}</h3>
      <div class="console-messages" ref="consoleMessages" style="height: 100%; overflow-y: auto;">
        <div v-for="(message, index) in messages" :key="index" class="console-message" :class="getMessageClass(message.type)">
          {{ message.message }}
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
    methods: {
      getMessageClass(type) {
        return {
          'info': type === 'info',
          'warning': type === 'warning',
          // Add more classes for different message types if needed
        };
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
    background-color: #007bff;
  }
  
  .warning {
    background-color: #ffc107;
  }
  
  /* Add more styles for different message types if needed */
  </style>
  
<script setup>
import { ref, watchEffect } from 'vue';
import io from 'socket.io-client';
import axios from 'axios';

const { title } = defineProps({
  title: String,
});

const numberOfInstances = ref(1);
const lifetimeSeconds = ref(0.1);
const lifecycle = ref(0.1);
const numberOfReplicas = ref(1);
const numberOfGeneration = ref(1);
const maxMatchLimit = ref(1);

const programAddress = ref('');
const socket = ref(null);
const isConnected = ref(false);
const consoleData = ref([]);
const simulationStatus = ref('');
const samplerStatus = ref('');
const instanceStatus = ref('');

const consoleOverlayRef = ref(null);

const addDataToConsole = (data) => {
  consoleData.value.push(data);
  scrollConsoleToBottom();
};

const scrollConsoleToBottom = () => {
  if (consoleOverlayRef.value) {
    consoleOverlayRef.value.scrollTop = consoleOverlayRef.value.scrollHeight;
  }
};

watchEffect(() => {
  scrollConsoleToBottom();
});

const updateValue = (event, variableName) => {
  switch (variableName) {
    case 'numberOfInstances':
      numberOfInstances.value = parseFloat(event.target.value);
      break;
    case 'lifetimeSeconds':
      lifetimeSeconds.value = parseFloat(event.target.value);
      break;
    case 'lifecycle':
      lifecycle.value = parseFloat(event.target.value);
      break;
    case 'numberOfReplicas':
      numberOfReplicas.value = parseInt(event.target.value);
      break;
    case 'numberOfGeneration':
      numberOfGeneration.value = parseInt(event.target.value);
      break;
    case 'maxMatchLimit':
      maxMatchLimit.value = parseInt(event.target.value);
      break;
    default:
      break;
  }
};

const toggleConnection = () => {
  if (isConnected.value) {
    disconnectSocket();
  } else {
    connectSocket();
  }
};

const connectSocket = () => {
  if (programAddress.value) {
    socket.value = io(programAddress.value);

    socket.value.on('connect', () => {
      console.log('Connected to socket server:', programAddress.value);
      isConnected.value = true;
      isStartDisabled.value = false;

      socket.value.on('simulation_status', (data) => {
        simulationStatus.value = data;
      });

      socket.value.on('simulation_sampler_status', (data) => {
        samplerStatus.value = data;
      });

      socket.value.on('simulation_instance_status', (data) => {
        instanceStatus.value = data;
        addDataToConsole(data);
      });
    });

    socket.value.on('connect_error', (error) => {
      console.error('Socket connection error:', error);
      disconnectSocket();

    });
  }
};

const disconnectSocket = () => {
  if (socket.value) {
    socket.value.disconnect();
    socket.value = null;
    isConnected.value = false;
    isStartDisabled.value = true;
    console.log('Disconnected from socket server');
  }
};

// Buton etkinlik durumları için ref değerleri
const isStartDisabled = ref(true);
const isPauseDisabled = ref(true);
const isResumeDisabled = ref(true);
const isStopDisabled = ref(true);

const startSimulation = async () => {
  if (!isConnected.value) {
    console.error('Socket is not connected');
    return;
  }

  isStartDisabled.value = true;
  isPauseDisabled.value = false;
  isResumeDisabled.value = true;
  isStopDisabled.value = false;

  try {
    const response = await axios.post(`${programAddress.value}/socket/v1/simulation/start`, {
      simulation_type: 'Core',
      number_of_instances: numberOfInstances.value,
      lifetime_seconds: lifetimeSeconds.value,
      lifecycle: lifecycle.value,
      number_of_replicas: numberOfReplicas.value,
      number_of_generation: numberOfGeneration.value,
      max_match_limit: maxMatchLimit.value
    });

    if (response.status === 200) {
      simulationStatus.value = response.data;

    } else {
      console.error('Failed to start simulation');
    }
  } catch (error) {
    console.error('Error starting simulation:', error);
  }
};

const pauseSimulation = async () => {
  if (!isConnected.value) {
    console.error('Socket is not connected');
    return;
  }

  isStartDisabled.value = true;
  isPauseDisabled.value = true;
  isResumeDisabled.value = false;
  isStopDisabled.value = false;

  try {
    const response = await axios.get(`${programAddress.value}/socket/v1/simulation/pause`);

    if (response.status === 200) {
      simulationStatus.value = response.data;
    }
  } catch (error) {
    console.error('Error pausing simulation:', error);
  }
};

const resumeSimulation = async () => {
  if (!isConnected.value) {
    console.error('Socket is not connected');
    return;
  }

  isStartDisabled.value = true;
  isPauseDisabled.value = false;
  isResumeDisabled.value = true;
  isStopDisabled.value = false;

  try {
    const response = await axios.get(`${programAddress.value}/socket/v1/simulation/continue`);

    if (response.status === 200) {
      simulationStatus.value = response.data;
    }
  } catch (error) {
    console.error('Error resuming simulation:', error);
  }
};

const stopSimulation = async () => {
  if (!isConnected.value) {
    console.error('Socket is not connected');
    return;
  }

  isStartDisabled.value = false;
  isPauseDisabled.value = true;
  isResumeDisabled.value = true;
  isStopDisabled.value = true;

  try {
    const response = await axios.get(`${programAddress.value}/socket/v1/simulation/stop`);

    if (response.status === 200) {
      simulationStatus.value = response.data;
    }
  } catch (error) {
    console.error('Error stopping simulation:', error);
  }
};
</script>

<template>
  <div class="simulation">
    <!-- Main panel -->
    <panel class="panel main">
      <!-- Canvas ve diğer bileşenler buraya gelecek -->
      <div class="canvas-container">
        <div class="title-overlay">
          <Widget class="title" :title="title"></Widget>
        </div>
        <Widget class="canvas" title=""></Widget>
      </div>

      <div class="console-overlay" ref="consoleOverlay" style="height: 200px; overflow-y: auto; color: white;">
        <Widget class="console" title="Console" :data="consoleData"></Widget>
      </div>

      <div class="information-overlay">
        <Widget class="simulation-status" title="Simulation Status" :data="simulationStatus"></Widget>
        <Widget class="sampler-status" title="Sampler Status" :data="samplerStatus"></Widget>
        <Widget class="instance-status" title="Instance Status" :data="instanceStatus"></Widget>
      </div>
    </panel>

    <!-- Sidebar panel -->
    <panel class="panel sidebar">
      <toolbar class="toolbar">
        <!-- Socket bağlantısı -->
        <div class="widget socket">
          <label>Program Address</label>
          <input v-model="programAddress" name="program_address" type="url" placeholder="ws://example.com" title="The main server where the cores are simulated." />
          <button @click="toggleConnection" :class="{ connected: isConnected }" title="The main server connection where the cores are simulated.">
            {{ isConnected ? 'Disconnect' : 'Connect' }}
          </button>
        </div>

        <!-- Simülasyon ayarları -->
        <div class="widget simulations">
          <div class="tab core">
            <label for="number_of_instances">Number of Instances: {{ numberOfInstances }}</label>
            <input name="number_of_instances" type="range" min="1" max="100" step="1" v-model="numberOfInstances"
              @input="updateValue($event, 'numberOfInstances')" title="Number of samples to be created. The first community created from nothing." />
            <label for="lifetime_seconds">Lifetime: {{ lifetimeSeconds }}</label>
            <input name="lifetime_seconds" type="range" min="0.1" max="60.0" step="0.1" v-model="lifetimeSeconds"
              @input="updateValue($event, 'lifetimeSeconds')" title="The particle's lifetime is in seconds. The assumed lifetime or energy of the particle." />
            <label for="lifecycle">Lifecycle: {{ lifecycle }}</label>
            <input name="lifecycle" type="range" min="0.1" max="60.0" step="0.1" v-model="lifecycle"
              @input="updateValue($event, 'lifecycle')" title="Minute life cycle of the particle. Time management. Examples of time travel of nuclei."/>
            <label for="number_of_replicas">Number of Replicas: {{ numberOfReplicas }}</label>
            <input name="number_of_replicas" type="range" min="1" max="20" step="1" v-model="numberOfReplicas"
              @input="updateValue($event, 'numberOfReplicas')" title="Number of copies to be created. When pairing occurs and the limit of new copies to be created."/>
            <label for="number_of_generation">Number of Generations: {{ numberOfGeneration }}</label>
            <input name="number_of_generation" type="range" min="1" max="10" step="1" v-model="numberOfGeneration"
              @input="updateValue($event, 'numberOfGeneration')" title="Generation depth. If the maximum throughput is reached or the maximum copy value is 0, it stops replicating." />
            <label for="max_match_limit">Maximum Match Limit: {{ maxMatchLimit }}</label>
            <input name="max_match_limit" type="range" min="1" max="10" step="1" v-model="maxMatchLimit"
              @input="updateValue($event, 'maxMatchLimit')" title="Maximum pairing limit. If any of the cores have been paired before or the maximum limit has been reached, it will skip pairing." />
          </div>
          <div class="tab particles"></div>
        </div>

        <div class="widget appearance">
          <label>Appearance</label>
          <select name="appearance" title="Change your perspective on simulation.">
            <option selected>Simulation</option>
            <option>Directory Tree</option>
            <option>Family Tree</option>
          </select>
        </div>
        <div class="widget action">
          <button @click="startSimulation" :disabled="isStartDisabled" title="Initializes time with all initial parameters.">
            Start
          </button>
          <button @click="pauseSimulation" :disabled="isPauseDisabled" title="Pauses time with all initial parameters.">
            Pause
          </button>
          <button @click="resumeSimulation" :disabled="isResumeDisabled" title="Resumes time with all initial parameters.">
            Resume
          </button>
          <button @click="stopSimulation" :disabled="isStopDisabled" title="Stop time with all starting parameters.">
            Stop
          </button>
        </div>
        <div class="widget formula">
          <label>Formula</label>
          <textarea name="formula" placeholder="5.5 * self.generation" title="It safely evaluates the formula entered by the user and updates its life time."></textarea>
          <button name="apply" title="Transfers the process of applying the formula to all copies to the queue. Updates are made to each core that gives a signal in the time stream.">Apply</button>
        </div>
      </toolbar>
    </panel>
  </div>
</template>

<script>
import Widget from "./Widget.vue";

export default {
  components: {
    Widget,
  },
  setup() {
    return {
      consoleData,
      consoleOverlayRef
    };
  },
};
</script>

<style>
/* Reset default margin and padding */
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
  font-size: 16px;
}

/* Main simulation container */
.simulation {
  display: flex;
  height: 100%;
}

/* Panels */
.panel {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 10px;
}

/* Main panel */
.main {
  position: relative;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-right: 10px;
  flex: 1;
  /* Main panel takes up remaining space */
}

.canvas-container {
  flex: 1;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.title-overlay {
  background-color: #ccc;
  padding: 10px;
  border-radius: 5px;
}

.canvas {
  width: 100%;
  height: 100%;
}

.console-overlay {
  background-color: #333;
  color: white;
  padding: 10px;
  flex: 0.25;
  /* Console takes 25% of main panel height */
  overflow-y: auto;
  /* Enable vertical scrolling if needed */
}

.information-overlay {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 25%;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 10px;
  border-radius: 5px;
}

/* Sidebar panel */
.sidebar {
  flex: 0 0 25%;
  /* Sidebar takes up 25% of simulation width */
  max-width: 25%;
  /* Max width to prevent exceeding 25% */
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 5px;
  overflow-y: auto;
  /* Enable vertical scrolling if needed */
}


.toolbar {
  padding: 10px;
}

.widget {
  margin-bottom: 20px;
}

.widget label {
  display: block;
  margin-bottom: 5px;
}

.widget input,
.widget select,
.widget textarea {
  width: 100%;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
}

.widget button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.widget button:hover {
  background-color: #0056b3;
}

.widget.socket {
  margin-bottom: 20px;
}

.widget.socket button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.widget.socket button.connected {
  background-color: #dc3545;
  /* Red color when connected */
}
</style>

<script setup>
const { title } = defineProps({
  title: String,
});

import { ref, watchEffect } from 'vue';

import io from 'socket.io-client';
import axios from 'axios';

import ConsoleWidget from './ConsoleWidget.vue';
import SimulationStatusWidget from './SimulationStatusWidget.vue';
import SamplerStatusWidget from './SamplerStatusWidget.vue';
import InstanceStatusWidget from './InstanceStatusWidget.vue';
import AppearanceTree from './AppearanceTree.vue';

const programAddress = ref('');
const socket = ref(null);
const isConnected = ref(false);

const numberOfInstances = ref(20);
const lifetimeSeconds = ref(10);
const lifecycle = ref(0.1);
const numberOfReplicas = ref(5);
const numberOfGeneration = ref(5);
const maxMatchLimit = ref(5);

const consoleOverlayRef = ref(null);

const simulationStatus = ref({});
const samplerStatus = ref({});
const instanceStatus = ref({});

const applcationLogList = ref([]);
const instanceStatusList = ref([])

const isStartDisabled = ref(true);
const isPauseDisabled = ref(true);
const isResumeDisabled = ref(true);
const isStopDisabled = ref(true);

const userFormula = ref('');
const evaluatedFormula = ref('');
const formulaError = ref(null);

const appearance = ref('AppearanceStatistic');

const MAX_DATA_LENGTH = 100;

const addApplcationLogData = (data) => {
  // Veriyi diziye ekle
  applcationLogList.value.push(data);

  // Dizinin uzunluğunu kontrol et
  if (applcationLogList.value.length > MAX_DATA_LENGTH) {
    // Diziye eklenen ilk kaydı kaldır
    applcationLogList.value.shift(); // Dizinin başından bir elemanı kaldırır
  }

  // Scroll işlemi
  scrollConsoleToBottom();
};

const addInstanceStatusData = (data) => {
  // instanceStatusList içinde data.id'ye sahip öğeyi bul
  const existingItemIndex = instanceStatusList.value.findIndex(item => item.id === data.id);

  if (existingItemIndex !== -1) {
    // Eğer data.id'ye sahip öğe varsa, bu öğenin life_status değerini kontrol et
    if (data.life_status === 'Stopped') {
      // Eğer life_status değeri "Stopped" ise, öğeyi listeden kaldır
      instanceStatusList.value.splice(existingItemIndex, 1);
    } else {
      // Eğer life_status değeri "Stopped" değilse, öğeyi güncelle
      Object.assign(instanceStatusList.value[existingItemIndex], data);
    }
  } else {
    // Eğer data.id'ye sahip öğe yoksa ve life_status değeri "Stopped" değilse, yeni öğeyi ekle
    if (data.life_status !== 'Stopped') {
      instanceStatusList.value.push(data);
    }
  }
};


const scrollConsoleToBottom = () => {
  if (consoleOverlayRef.value) {
    consoleOverlayRef.value.scrollTop = consoleOverlayRef.value.scrollHeight;
  }
};

const connectSocket = () => {
  if (programAddress.value) {

    socket.value = io(programAddress.value, {
      transports: ['websocket'],
      upgrade: false
    });

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
        addInstanceStatusData(data);
      });

      socket.value.on('application_log', (data) => {
        addApplcationLogData(data);
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
    // Disable buttons when disconnected
    isStartDisabled.value = true;
    isPauseDisabled.value = true;
    isResumeDisabled.value = true;
    isStopDisabled.value = true;

    console.log('Disconnected from socket server');
  }
};

const toggleConnection = () => {
  if (isConnected.value) {
    disconnectSocket();
  } else {
    connectSocket();
  }
};

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
      // simulation_type: 'Core',
      simulation_type: 'Particles',
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

const evaluateFormula = (formula) => {
  try {
    const evaluated = vm.run(`return ${formula}`);
    // Handle the evaluated result
    return evaluated;
  } catch (error) {
    console.error('Error evaluating formula:', error);
    throw new Error('Invalid formula');
  }
};

const applyFormula = async () => {
  if (!isConnected.value) {
    console.error('Socket is not connected');
    return;
  }

  formulaError.value = null;

  try {
    // Check syntax without actually evaluating the formula
    new Function(`return ${userFormula.value}`);
  } catch (error) {
    formulaError.value = 'Invalid formula syntax';
    console.error('Invalid formula syntax:', error);
    return;
  }

  try {
    const response = await axios.post(`${programAddress.value}/socket/v1/simulation/update/all`, {
      formula: userFormula.value,
    });

    if (response.status === 200) {
      const result = response.data;
      evaluatedFormula.value = result;
      console.log('Formula applied successfully:', result);
      // Display result to user or update UI as needed
    }
  } catch (error) {
    console.error('Error applying formula:', error);
  }
};

const changeAppearance = (event) => {
  // appearance değerini yeni görünüşe göre güncelle


  // appearance değerine göre içeriği güncelle
  switch (appearance.value) {

    case 'DirectoryTree':
      appearance.value = "DirectoryTree";
      break;
    case 'FamilyTree':
      appearance.value = "FamilyTree";
      break;
    case 'AppearanceStatistic':
      appearance.value = "AppearanceStatistic";
      break;
    case 'AppearanceParticle':
      appearance.value = "AppearanceParticle";
      break;


    default:
      appearance.value = "AppearanceStatistic";
      break;
  }
};

watchEffect(() => {
  scrollConsoleToBottom();
});

</script>

<template>
  <div class="simulation">
    <!-- Main panel -->
    <div class="panel main">

      <div class="appearance-container">

        <AppearanceTree class="canvas" title="AppearanceTree" :appearance="appearance" :dataList="instanceStatusList">
        </AppearanceTree>

      </div>

      <div class="console-overlay" ref="consoleOverlay">
        <ConsoleWidget class="console" :title="'Console'" :dataList="applcationLogList"></ConsoleWidget>
      </div>

    </div>
    <div class="widget-container">
      <SimulationStatusWidget class="widget" :simulationType="simulationStatus.simulation_type"
        :simulationStatus="simulationStatus.simulation_status"></SimulationStatusWidget>

      <SamplerStatusWidget class="widget" :name="samplerStatus.name"
        :numberOfInstances="samplerStatus.number_of_instance" :lifetimeSeconds="samplerStatus.lifetime_seconds"
        :lifecycle="samplerStatus.lifecycle" :numberOfInstancesCreated="samplerStatus.number_of_instance_created"
        :numberOfInstancesMatched="samplerStatus.number_of_instance_matched"
        :numberOfInstancesReplicated="samplerStatus.number_of_instance_replicated"></SamplerStatusWidget>

      <InstanceStatusWidget class="widget" :id="instanceStatus.id" :parentId="instanceStatus.parent_id"
        :lifetimeSeconds="instanceStatus.lifetime_seconds" :lifeCreatedTime="instanceStatus.life_created_time"
        :lifeStartTime="instanceStatus.life_start_time" :elapsedLifespan="instanceStatus.elapsed_lifespan"
        :lifecycle="instanceStatus.lifecycle" :lifeStatus="instanceStatus.life_status"
        :numberOfCopies="instanceStatus.number_of_copies" :generation="instanceStatus.generation"
        :matchCount="instanceStatus.match_count" :fitness="instanceStatus.fitness" :codes="instanceStatus.codes">
      </InstanceStatusWidget>

    </div>
    <!-- Sidebar panel -->
    <div class="panel sidebar">

      <div class="toolbar">

        <!-- Socket bağlantısı -->
        <div class="widget socket">
          <label>Program Address</label>
          <input v-model="programAddress" name="program_address" type="url" placeholder="ws://example.com"
            title="The main server where the cores are simulated." />
          <button @click="toggleConnection" :class="{ connected: isConnected }"
            title="The main server connection where the cores are simulated.">
            {{ isConnected ? 'Disconnect' : 'Connect' }}
          </button>
        </div>

        <div class="widget appearance">
          <label>Appearance</label>
          <select v-model="appearance" name="appearance" @change="changeAppearance"
            title="Change your perspective on simulation.">

            <option value="DirectoryTree">Directory</option>
            <option value="FamilyTree">Family</option>
            <option value="AppearanceStatistic">Statistic</option>
            <option value="AppearanceParticle">Particle</option>

          </select>
        </div>

        <!-- Simülasyon ayarları -->
        <div class="widget simulations">

          <div class="tab core">

            <label for="number_of_instances">Number of Instances: {{ numberOfInstances }}</label>
            <input name="number_of_instances" type="range" min="1" max="100" step="1" v-model="numberOfInstances"
              @input="updateValue($event, 'numberOfInstances')"
              title="Number of samples to be created. The first community created from nothing." />

            <label for="lifetime_seconds">Lifetime: {{ lifetimeSeconds }}</label>
            <input name="lifetime_seconds" type="range" min="0.1" max="60.0" step="0.1" v-model="lifetimeSeconds"
              @input="updateValue($event, 'lifetimeSeconds')"
              title="The particle's lifetime is in seconds. The assumed lifetime or energy of the particle." />

            <label for="lifecycle">Lifecycle: {{ lifecycle }}</label>
            <input name="lifecycle" type="range" min="0.1" max="60.0" step="0.1" v-model="lifecycle"
              @input="updateValue($event, 'lifecycle')"
              title="Minute life cycle of the particle. Time management. Examples of time travel of nuclei." />

            <label for="number_of_replicas">Number of Replicas: {{ numberOfReplicas }}</label>
            <input name="number_of_replicas" type="range" min="1" max="20" step="1" v-model="numberOfReplicas"
              @input="updateValue($event, 'numberOfReplicas')"
              title="Number of copies to be created. When pairing occurs and the limit of new copies to be created." />


            <label for="max_match_limit">Maximum Match Limit: {{ maxMatchLimit }}</label>
            <input name="max_match_limit" type="range" min="1" max="10" step="1" v-model="maxMatchLimit"
              @input="updateValue($event, 'maxMatchLimit')"
              title="Maximum pairing limit. If any of the cores have been paired before or the maximum limit has been reached, it will skip pairing." />


            <label for="number_of_generation">Number of Generations: {{ numberOfGeneration }}</label>
            <input name="number_of_generation" type="range" min="1" max="10" step="1" v-model="numberOfGeneration"
              @input="updateValue($event, 'numberOfGeneration')"
              title="Generation depth. If the maximum throughput is reached or the maximum copy value is 0, it stops replicating." />


          </div>

          <div class="tab particles"></div>
        </div>



        <div class="widget action">
          <button @click="startSimulation" :disabled="isStartDisabled"
            title="Initializes time with all initial parameters.">
            Start
          </button>
          <button @click="pauseSimulation" :disabled="isPauseDisabled" title="Pauses time with all initial parameters.">
            Pause
          </button>
          <button @click="resumeSimulation" :disabled="isResumeDisabled"
            title="Resumes time with all initial parameters.">
            Resume
          </button>
          <button @click="stopSimulation" :disabled="isStopDisabled" title="Stop time with all starting parameters.">
            Stop
          </button>
        </div>

        <div class="widget formula">

          <label>Formula</label>

          <input type="text" v-model="userFormula" name="formula" placeholder="5.5 * self.generation"
            title="It safely evaluates the formula entered by the user and updates its life time."
            list="formula-suggestions"></input>

          <datalist id="formula-suggestions">
            <option value="5.5 * self.generation"></option>
            <option value="Math.pow(self.generation, 2)"></option>
            <option value="Math.sin(self.generation)"></option>
            <!-- Add more example formulas -->
          </datalist>

          <span v-if="formulaError" class="error-message">{{ formulaError }}</span>

          <button @click="applyFormula" :disabled="!isStartDisabled || isStopDisabled"
            title="Apply the formula to all copies.">Apply</button>
        </div>

      </div>

    </div>

  </div>
</template>

<style>
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
  font-size: 16px;
}

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
  overflow-y: hidden
}

.main {
  position: relative;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-right: 10px;
  flex: 1;
}

.appearance-container {
  flex: 1;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  width: 100%;
  height: 100%;
}

.canvas {
  width: 100%;
  height: 100%;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 20px;
}

.title-overlay {
  background-color: #ccc;
  padding: 10px;
  border-radius: 5px;
}

.console-overlay {
  background-color: rgba(19, 19, 19, 0.744);
  color: white;
  padding: 10px;
  flex: 0.25;
  overflow: hidden;
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


.sidebar {
  flex: 0 0 25%;
  max-width: 25%;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 5px;
  overflow-y: auto;
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
}

.error-message {
  color: red;
  font-size: 12px;
}

.widget-container {

  overflow-y: auto;
  width: 12rem;
  overflow-x: hidden
}
</style>

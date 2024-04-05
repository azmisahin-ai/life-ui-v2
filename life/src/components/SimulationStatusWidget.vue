<template>
    <div class="widget-container">
      <h3>Simulation Status</h3>
      <div class="status-item">
        <span class="status-label">Type:</span>
        <span class="status-value">{{ simulationType }}</span>
      </div>
      <div class="status-item">
        <span class="status-label">Status:</span>
        <span class="status-value">{{ simulationStatus }}</span>
      </div>
    </div>
  </template>
  

<script>
export default {
    props: {
        simulationType: String,
        simulationStatus: String
    },
    computed: {
        statusClass() {
            // Dinamik olarak durum sınıfı belirle
            return {
                'status-running': this.simulationStatus === 'Running',
                'status-paused': this.simulationStatus === 'Paused',
                'status-stopped': this.simulationStatus === 'Stopped'
                // Ek durumlar eklenebilir
            };
        }
    }
};
</script>

<style scoped>
.simulation-status-widget {
    background-color: #f0f0f0;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ddd;
    width: 250px;
    /* Ölçüleri ayarlayabilirsiniz */
}

h3 {
    margin-bottom: 10px;
}

.status {
    padding: 8px;
    border-radius: 3px;
    font-weight: bold;
}

.status-running {
    color: green;
}

.status-paused {
    color: orange;
}

.status-stopped {
    color: red;
}
/* Özel stiller burada tanımlanır */
.widget-container {
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 10px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.widget-container h3 {
  margin-bottom: 10px;
  color: #007bff;
}

.status-item {
  margin-bottom: 8px;
}

.status-label {
  font-weight: bold;
}

.status-value {
  color: #333;
}
</style>
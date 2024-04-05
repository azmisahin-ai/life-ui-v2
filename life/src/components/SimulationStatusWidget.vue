<template>
    <div class="simulation-status-widget">
        <h3>{{ simulationType }}</h3>
        <div class="status" :class="statusClass">
            {{ simulationStatus }}
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
</style>
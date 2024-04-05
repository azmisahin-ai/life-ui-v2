<template>
    <div class="widget-container">
        <h3>{{ name }}</h3>
        <div class="status-item">
            <p>Number of Instances: {{ numberOfInstances }}</p>
        </div>
        <div class="status-item">
            <p>Lifetime (seconds): {{ lifetimeSeconds }}</p>
        </div>
        <div class="status-item">
            <p>Life Cycle: {{ lifecycle }}</p>
        </div>
        <div class="status-item">
            <p>Instances Created: {{ numberOfInstancesCreated }}</p>
        </div>
        <div class="status-item">
            <p>Instances Matched: {{ numberOfInstancesMatched }}</p>
        </div>
        <div class="status-item">
            <p>Instances Replicated: {{ numberOfInstancesReplicated }}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        name: String,
        numberOfInstances: Number,
        lifetimeSeconds: Number,
        lifecycle: Number,
        numberOfInstancesCreated: Number,
        numberOfInstancesMatched: Number,
        numberOfInstancesReplicated: Number
    }
};
</script>

<style scoped>
.sampler-status-widget {
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

.status-item {
    margin-bottom: 8px;
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
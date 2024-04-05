<template>
    <div class="instance-status-widget">
        <h3>{{ name }}</h3>
        <div class="status-item">
            <p>ID: {{ id }}</p>
        </div>
        <div class="status-item">
            <p>Parent ID: {{ parentId }}</p>
        </div>
        <div class="status-item">
            <p>Lifetime (seconds): {{ lifetimeSeconds }}</p>
        </div>
        <div class="status-item">
            <p>Life Created Time: {{ formattedTime(lifeCreatedTime) }}</p>
        </div>
        <div class="status-item">
            <p>Life Start Time: {{ formattedTime(lifeStartTime) }}</p>
        </div>
        <div class="status-item">
            <p>Elapsed Lifespan: {{ elapsedLifespan }}</p>
        </div>
        <div class="status-item">
            <p>Life Cycle: {{ lifecycle }}</p>
        </div>
        <div class="status-item">
            <p>Life Status: {{ lifeStatus }}</p>
        </div>
        <div class="status-item">
            <p>Number of Copies: {{ numberOfCopies }}</p>
        </div>
        <div class="status-item">
            <p>Generation: {{ generation }}</p>
        </div>
        <div class="status-item">
            <p>Match Count: {{ matchCount }}</p>
        </div>
        <div class="status-item">
            <p>Fitness: {{ fitness }}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        name: String,
        id: Number,
        parentId: Number,
        lifetimeSeconds: Number,
        lifeCreatedTime: Number,
        lifeStartTime: Number,
        elapsedLifespan: Number,
        lifecycle: Number,
        lifeStatus: String,
        numberOfCopies: Number,
        generation: Number,
        matchCount: Number,
        fitness: Number
    },
    methods: {
        formattedTime(timestamp) {
            const date = new Date(timestamp * 1000);
            return date.toLocaleString(); // You can customize the date format
        }
    }
};
</script>

<style scoped>
.instance-status-widget {
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
</style>
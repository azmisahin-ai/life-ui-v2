<template>
    <div class="widget-container">
        <h3>{{ name }}</h3>
        <div class="status-item">
            <p>ID: <b>{{ id }}</b></p>
        </div>
        <div class="status-item">
            <p>Parent ID:<b> {{ parentId }} </b> </p>

        </div>
        <div class="status-item">
            <p>Lifetime (seconds): </p>
            <b>{{ lifetimeSeconds }}</b>
        </div>
        <div class="status-item">
            <p>Life Created Time: </p>
            <b>{{ formattedTime(lifeCreatedTime) }}</b>
        </div>
        <div class="status-item">
            <p>Life Start Time: </p>
            <b>{{ formattedTime(lifeStartTime) }}</b>
        </div>
        <div class="status-item">
            <p>Elapsed Lifespan: </p>
            <b>{{ elapsedLifespan }}</b>
        </div>
        <div class="status-item">
            <p>Life Cycle: </p>
            <b>{{ lifecycle }}</b>
        </div>
        <div class="status-item">
            <p>Life Status: </p>
            <b>{{ lifeStatus }}</b>
        </div>
        <div class="status-item">
            <p>Number of Copies: </p>
            <b>{{ numberOfCopies }}</b>
        </div>
        <div class="status-item">
            <p>Generation: </p>
            <b>{{ generation }}</b>
        </div>
        <div class="status-item">
            <p>Match Count: </p>
            <b>{{ matchCount }}</b>
        </div>
        <div class="status-item">
            <p>Fitness: </p>
            <b>{{ fitness }}</b>
        </div>
        <div class="status-item">
            <p>Codes: </p>
            <b>{{ codes }}</b>
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
        fitness: Number,
        codes:Array
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

/* Özel stiller burada tanımlanır */
.widget-container {
    margin-bottom: 20px;
    border: 1px solid #ddd;
    border-radius: 5px;
    padding: 10px;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
   
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
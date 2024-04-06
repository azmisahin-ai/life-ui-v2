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
            <p>Assembly Codes:</p>
            <div class="assembly-codes">
                <div v-for="(assembly, index) in assemblyCodes" :key="index" class="assembly-code">
                    {{ assembly }}
                </div>
            </div>
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
        codes: Array,
    },
    computed: {
        assemblyCodes() {
            if (!this.codes || this.codes.length === 0) {
                return [];
            }
            return this.codes.map(code => this.assemblyCodeFromNumber(code));
        },
    },
    methods: {
        formattedTime(timestamp) {
            const date = new Date(timestamp * 1000);
            return date.toLocaleString(); // Customize date format as needed
        },
        assemblyCodeFromNumber(code) {
            switch (code) {
                case 0x00: return "NOP";
                case 0x01: return "LXI B, 16-bit Data";
                case 0x02: return "STAX B";
                case 0x03: return "INX B";
                case 0x04: return "INR B";
                case 0x05: return "DCR B";
                case 0x06: return "MVI B, 8-bit Data";
                case 0x07: return "RLC";
                case 0x08: return "NOP";
                case 0x09: return "DAD B";
                case 0x0A: return "LDAX B";
                case 0x0B: return "DCX B";
                case 0x0C: return "INR C";
                case 0x0D: return "DCR C";
                case 0x0E: return "MVI C, 8-bit Data";
                case 0x0F: return "RRC";
                case 0x10: return "NOP";
                case 0x11: return "LXI D, 16-bit Data";
                case 0x12: return "STAX D";
                case 0x13: return "INX D";
                case 0x14: return "INR D";
                case 0x15: return "DCR D";
                case 0x16: return "MVI D, 8-bit Data";
                case 0x17: return "RAL";
                case 0x18: return "NOP";
                case 0x19: return "DAD D";
                case 0x1A: return "LDAX D";
                case 0x1B: return "DCX D";
                case 0x1C: return "INR E";
                case 0x1D: return "DCR E";
                case 0x1E: return "MVI E, 8-bit Data";
                case 0x1F: return "RAR";
                // Add more cases for other opcodes up to 0xFF (255 in decimal)
                default: return `Opcode ${code}`;
            }
        },
    },
};
</script>

<style scoped>
.assembly-widget {
    background-color: #f0f0f0;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ddd;
    width: 250px;
}

h3 {
    margin-bottom: 10px;
    color: #007bff;
}

.status-item {
    margin-bottom: 8px;
}

.assembly-codes {
    padding-left: 15px;
}

.assembly-code {
    margin-bottom: 5px;
    font-family: monospace;
}
</style>
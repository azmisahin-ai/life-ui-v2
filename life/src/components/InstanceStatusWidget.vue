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
                case 0x20: return "NOP";
                case 0x21: return "LXI H, 16-bit Data";
                case 0x22: return "SHLD 16-bit Address";
                case 0x23: return "INX H";
                case 0x24: return "INR H";
                case 0x25: return "DCR H";
                case 0x26: return "MVI H, 8-bit Data";
                case 0x27: return "DAA";
                case 0x28: return "NOP";
                case 0x29: return "DAD H";
                case 0x2A: return "LHLD 16-bit Address";
                case 0x2B: return "DCX H";
                case 0x2C: return "INR L";
                case 0x2D: return "DCR L";
                case 0x2E: return "MVI L, 8-bit Data";
                case 0x2F: return "CMA";
                case 0x30: return "NOP";
                case 0x31: return "LXI SP, 16-bit Data";
                case 0x32: return "STA 16-bit Address";
                case 0x33: return "INX SP";
                case 0x34: return "INR M";
                case 0x35: return "DCR M";
                case 0x36: return "MVI M, 8-bit Data";
                case 0x37: return "STC";
                case 0x38: return "NOP";
                case 0x39: return "DAD SP";
                case 0x3A: return "LDA 16-bit Address";
                case 0x3B: return "DCX SP";
                case 0x3C: return "INR A";
                case 0x3D: return "DCR A";
                case 0x3E: return "MVI A, 8-bit Data";
                case 0x3F: return "CMC";
                case 0x40: return "MOV B, B";
                case 0x41: return "MOV B, C";
                case 0x42: return "MOV B, D";
                case 0x43: return "MOV B, E";
                case 0x44: return "MOV B, H";
                case 0x45: return "MOV B, L";
                case 0x46: return "MOV B, M";
                case 0x47: return "MOV B, A";
                case 0x48: return "MOV C, B";
                case 0x49: return "MOV C, C";
                case 0x4A: return "MOV C, D";
                case 0x4B: return "MOV C, E";
                case 0x4C: return "MOV C, H";
                case 0x4D: return "MOV C, L";
                case 0x4E: return "MOV C, M";
                case 0x4F: return "MOV C, A";
                case 0x50: return "MOV D, B";
                case 0x51: return "MOV D, C";
                case 0x52: return "MOV D, D";
                case 0x53: return "MOV D, E";
                case 0x54: return "MOV D, H";
                case 0x55: return "MOV D, L";
                case 0x56: return "MOV D, M";
                case 0x57: return "MOV D, A";
                case 0x58: return "MOV E, B";
                case 0x59: return "MOV E, C";
                case 0x5A: return "MOV E, D";
                case 0x5B: return "MOV E, E";
                case 0x5C: return "MOV E, H";
                case 0x5D: return "MOV E, L";
                case 0x5E: return "MOV E, M";
                case 0x5F: return "MOV E, A";
                case 0x60: return "MOV H, B";
                case 0x61: return "MOV H, C";
                case 0x62: return "MOV H, D";
                case 0x63: return "MOV H, E";
                case 0x64: return "MOV H, H";
                case 0x65: return "MOV H, L";
                case 0x66: return "MOV H, M";
                case 0x67: return "MOV H, A";
                case 0x68: return "MOV L, B";
                case 0x69: return "MOV L, C";
                case 0x6A: return "MOV L, D";
                case 0x6B: return "MOV L, E";
                case 0x6C: return "MOV L, H";
                case 0x6D: return "MOV L, L";
                case 0x6E: return "MOV L, M";
                case 0x6F: return "MOV L, A";
                case 0x70: return "MOV M, B";
                case 0x71: return "MOV M, C";
                case 0x72: return "MOV M, D";
                case 0x73: return "MOV M, E";
                case 0x74: return "MOV M, H";
                case 0x75: return "MOV M, L";
                case 0x76: return "HLT";
                case 0x77: return "MOV M, A";
                case 0x78: return "MOV A, B";
                case 0x79: return "MOV A, C";
                case 0x7A: return "MOV A, D";
                case 0x7B: return "MOV A, E";
                case 0x7C: return "MOV A, H";
                case 0x7D: return "MOV A, L";
                case 0x7E: return "MOV A, M";
                case 0x7F: return "MOV A, A";
                case 0x80: return "ADD B";
                case 0x81: return "ADD C";
                case 0x82: return "ADD D";
                case 0x83: return "ADD E";
                case 0x84: return "ADD H";
                case 0x85: return "ADD L";
                case 0x86: return "ADD M";
                case 0x87: return "ADD A";
                case 0x88: return "ADC B";
                case 0x89: return "ADC C";
                case 0x8A: return "ADC D";
                case 0x8B: return "ADC E";
                case 0x8C: return "ADC H";
                case 0x8D: return "ADC L";
                case 0x8E: return "ADC M";
                case 0x8F: return "ADC A";
                case 0x90: return "SUB B";
                case 0x91: return "SUB C";
                case 0x92: return "SUB D";
                case 0x93: return "SUB E";
                case 0x94: return "SUB H";
                case 0x95: return "SUB L";
                case 0x96: return "SUB M";
                case 0x97: return "SUB A";
                case 0x98: return "SBB B";
                case 0x99: return "SBB C";
                case 0x9A: return "SBB D";
                case 0x9B: return "SBB E";
                case 0x9C: return "SBB H";
                case 0x9D: return "SBB L";
                case 0x9E: return "SBB M";
                case 0x9F: return "SBB A";
                case 0xA0: return "ANA B";
                case 0xA1: return "ANA C";
                case 0xA2: return "ANA D";
                case 0xA3: return "ANA E";
                case 0xA4: return "ANA H";
                case 0xA5: return "ANA L";
                case 0xA6: return "ANA M";
                case 0xA7: return "ANA A";
                case 0xA8: return "XRA B";
                case 0xA9: return "XRA C";
                case 0xAA: return "XRA D";
                case 0xAB: return "XRA E";
                case 0xAC: return "XRA H";
                case 0xAD: return "XRA L";
                case 0xAE: return "XRA M";
                case 0xAF: return "XRA A";
                case 0xB0: return "ORA B";
                case 0xB1: return "ORA C";
                case 0xB2: return "ORA D";
                case 0xB3: return "ORA E";
                case 0xB4: return "ORA H";
                case 0xB5: return "ORA L";
                case 0xB6: return "ORA M";
                case 0xB7: return "ORA A";
                case 0xB8: return "CMP B";
                case 0xB9: return "CMP C";
                case 0xBA: return "CMP D";
                case 0xBB: return "CMP E";
                case 0xBC: return "CMP H";
                case 0xBD: return "CMP L";
                case 0xBE: return "CMP M";
                case 0xBF: return "CMP A";
                case 0xC0: return "RNZ";
                case 0xC1: return "POP B";
                case 0xC2: return "JNZ 16-bit Address";
                case 0xC3: return "JMP 16-bit Address";
                case 0xC4: return "CNZ 16-bit Address";
                case 0xC5: return "PUSH B";
                case 0xC6: return "ADI 8-bit Data";
                case 0xC7: return "RST 0";
                case 0xC8: return "RZ";
                case 0xC9: return "RET";
                case 0xCA: return "JZ 16-bit Address";
                case 0xCB: return "JMP 16-bit Address";
                case 0xCC: return "CZ 16-bit Address";
                case 0xCD: return "CALL 16-bit Address";
                case 0xCE: return "ACI 8-bit Data";
                case 0xCF: return "RST 1";
                case 0xD0: return "RNC";
                case 0xD1: return "POP D";
                case 0xD2: return "JNC 16-bit Address";
                case 0xD3: return "OUT 8-bit Data";
                case 0xD4: return "CNC 16-bit Address";
                case 0xD5: return "PUSH D";
                case 0xD6: return "SUI 8-bit Data";
                case 0xD7: return "RST 2";
                case 0xD8: return "RC";
                case 0xD9: return "NOP";
                case 0xDA: return "JC 16-bit Address";
                case 0xDB: return "IN 8-bit Data";
                case 0xDC: return "CC 16-bit Address";
                case 0xDD: return "NOP";
                case 0xDE: return "SBI 8-bit Data";
                case 0xDF: return "RST 3";
                case 0xE0: return "RPO";
                case 0xE1: return "POP H";
                case 0xE2: return "JPO 16-bit Address";
                case 0xE3: return "XTHL";
                case 0xE4: return "CPO 16-bit Address";
                case 0xE5: return "PUSH H";
                case 0xE6: return "ANI 8-bit Data";
                case 0xE7: return "RST 4";
                case 0xE8: return "RPE";
                case 0xE9: return "PCHL";
                case 0xEA: return "JPE 16-bit Address";
                case 0xEB: return "XCHG";
                case 0xEC: return "CPE 16-bit Address";
                case 0xED: return "NOP";
                case 0xEE: return "XRI 8-bit Data";
                case 0xEF: return "RST 5";
                case 0xF0: return "RP";
                case 0xF1: return "POP PSW";
                case 0xF2: return "JP 16-bit Address";
                case 0xF3: return "DI";
                case 0xF4: return "CP 16-bit Address";
                case 0xF5: return "PUSH PSW";
                case 0xF6: return "ORI 8-bit Data";
                case 0xF7: return "RST 6";
                case 0xF8: return "RM";
                case 0xF9: return "SPHL";
                case 0xFA: return "JM 16-bit Address";
                case 0xFB: return "EI";
                case 0xFC: return "CM 16-bit Address";
                case 0xFD: return "NOP";
                case 0xFE: return "CPI 8-bit Data";
                case 0xFF: return "RST 7";
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
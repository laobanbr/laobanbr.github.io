<template>
    <div id="app">
        <div id="timer" class="" :class="bgColor">
            <h1>{{ timer }}</h1>
            <h3>Rounds left: {{ nRounds - roundsElapsed }}</h3>
        </div>
        <div id="controls">
            <h1>Tabata Timer</h1>
            <button class="btn btn-success" @click="startRound">Start</button>
            <button class="btn btn-danger" @click="stopRound">Stop</button>
            <button class="btn btn-primary" @click="reset">Reset</button>
            <hr>
            <p>How many rounds?</p>
            <input type="text" v-model="nRounds">
            <hr>
            <h3>Total time: {{ totalTime }}</h3>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                nRounds: 8,
                roundsElapsed: 0,
                bgColor: 'red',
                totalTime: 0,
                timer: 10,
                mode: 'rest',
                running: false,
            }
        },
        methods: {
            startRound(){
                if (!this.running) {
                    this.intervalId = setInterval( () => {
                        this.timer -= 1;
                        this.totalTime += 1;
                        if (this.timer == 0){
                            if (this.mode=='rest'){
                                this.mode = 'active'
                                this.timer = 20;
                                this.bgColor = 'green';
                            } else {
                                this.mode = 'rest';
                                this.timer = 10;
                                this.bgColor = 'red'
                                this.roundsElapsed += 1;
                                if (this.roundsElapsed == this.nRounds){
                                    this.stopRound();
                                    this.reset();
                                }
                            }
                        }

                    }, 1000);
                    this.running = true;
                } else {
                    alert('Already running, reset to restart.');
                }
            },
            stopRound(){
                if(this.running){
                    this.running = false;
                    clearInterval(this.intervalId);
                }
            },
            reset(){
                this.stopRound();
                this.running = false;
                this.mode = 'rest';
                this.timer = 2;
                this.bgColor = 'red';
                this.totalTime = 0;
                this.roundsElapsed = 0;
            }
        }
    }
</script>

<style>
html, body {
    height: 100%;
    padding: 0;
    margin: 0;
}
#app {
    display: grid;
    grid-template-columns: 1fr 4fr;
    grid-template-rows: auto;
    height: 100%;
    text-align: center;
}
#timer {
    grid-column: 2;
    grid-row: 1;
    text-align: center;
    margin: auto;
    color: white;
    height: 100%;
    width: 100%;
    padding-top: 20%;
}
#controls {
    grid-column: 1;

    grid-row: 1;
    height: 100%;
    width: 100%;
    border-right: 1px solid #333;
}

.red {
    background-color: #FE4042;
}

.green {
    background-color: #90EE90;
}
</style>

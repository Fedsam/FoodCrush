<template>
    <div>
        <div v-if="!isStart" id="start-wrapper">
            <button @click.prevent="launchGame">Start</button>
        </div>
        <div id="app">
            <header>
                <h1>FoodCrush</h1>
            </header>
            <main>
                <div id="score">
                    <h2>Score : {{ score }}</h2>
                </div>
                <Board :isStart="isStart" @updateScore="updateScore" />
            </main>
        </div>
    </div>
</template>

<script>
import Board from "./components/Board";

export default {
    name: "App",
    data() {
        return {
            score: 0,
            isStart: false,
        };
    },
    methods: {
        launchGame() {
            this.isStart = true;
        },
        updateScore(newScore) {
            this.score += newScore;
        },
    },
    components: {
        Board,
    },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Pacifico&display=swap");

html,
body {
    height: 100vh;
    margin: 0;
    padding: 0;
    background: linear-gradient(to bottom, #2980b9, #6dd5fa);
}

#start-wrapper {
    height: 100vh;
    width: 100vw;
    position: absolute;
    background-color: #ffffffdd;
    display: grid;
    place-items: center;
}

#app {
    display: flex;
    flex-direction: column;
}

header,
#score {
    text-align: center;
}

main {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
    align-items: center;
}

h1,
h2,
button {
    font-family: "Pacifico", cursive;
    text-shadow: 2px 2px black;
    color: #f9ca24;
}

h1,
button {
    font-size: 10vh;
}

button {
    background-color: #2980b9;
    border-radius: 5%;
    padding: 0 3% 0 2%;
    cursor: pointer;
    transition: all 0.5s ease;
    transform: scale(1);
}

button:hover {
    background-color: #6dd5fa;
    transform: scale(1.5) perspective(1px);
}

h2 {
    font-size: 5vh;
}
</style>

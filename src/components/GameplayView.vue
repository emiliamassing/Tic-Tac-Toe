<script setup lang="ts">
    import { ref } from 'vue';
    import AddPlayer from './AddPlayer.vue';
    import GameResults from './GameResults.vue';
    import Player from '../models/Player';
    import { IGameBoard } from '../models/IGameBoard';

    interface IPlayerProps {
        player: Player[],
    };
    
    const gridTemplate = ref<IGameBoard[]>([
        {position: 0, shape: '', clicked: false},
        {position: 1, shape: '', clicked: false},
        {position: 2, shape: '', clicked: false},
        {position: 3, shape: '', clicked: false},
        {position: 4, shape: '', clicked: false},
        {position: 5, shape: '', clicked: false},
        {position: 6, shape: '', clicked: false},
        {position: 7, shape: '', clicked: false},
        {position: 8, shape: '', clicked: false},
    ]);

    const winningPatterns: number[][] = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ];

    let gameIsRunning = ref(true);
    let winner = ref(false);
    let winningPlayer = ref();
    
    const gridContainer: string = 'gridContainer';
    const gridCell: string = 'gridCell';
    const buttonContainer: string = 'buttonContainer';

    const props = defineProps<IPlayerProps>();

    let activePlayer = ref(props.player[0]);

    console.log(activePlayer);
    
    
    function endGame() {
        gameIsRunning.value = false;
        localStorage.removeItem('playerList');
    };

    function clearBoard() {
        gridTemplate.value.forEach(grid => {
            grid.shape = '';
            grid.clicked = false;
        });
    };

    /*function randomizeFirstPlayer(): Player {
        const randomPlayer = props.player[Math.floor(Math.random()*props.player.length)];
        console.log(randomPlayer);

        return randomPlayer;
    };*/

    function placeShape(position: number) {
        gridTemplate.value[position].shape = activePlayer.value.shape;
        gridTemplate.value[position].clicked = true;


        checkIfWinner();
        swapActivePlayer();
    };

    function checkIfWinner() {
        for(let i = 0; i < winningPatterns.length; i++) {
            let [a, b, c] = winningPatterns[i];
            //console.log([a, b, c]);
            //console.log(gridTemplate.value[a].shape);
            if(
                gridTemplate.value[a].shape && 
                gridTemplate.value[a].shape == gridTemplate.value[b].shape &&
                gridTemplate.value[a].shape == gridTemplate.value[c].shape
            ) {
                winner.value = true;

                function findWinnerShape(correctShape: any) {
                    return correctShape.shape === gridTemplate.value[a].shape;
                };

                const winnerShape = gridTemplate.value.find(findWinnerShape);

                console.log(winnerShape, winningPlayer);

            };
        };
    }

    function swapActivePlayer() {
        if(activePlayer.value === props.player[0]) {
            console.log('X');
            activePlayer.value = props.player[1];
        } else {
            console.log('O');
            activePlayer.value = props.player[0];
        };
    };
</script>

<template>
    <main v-if="gameIsRunning">
        <nav :class="buttonContainer">
            <button>Show Results</button>
            <button @click="clearBoard">Clear Board</button>
            <button @click="endGame">Reset Game</button>
        </nav>
        <h2 v-if="!winner">{{ props.player[0].name }} VS {{  props.player[1].name }}</h2>
        <h2 v-else="winner">Game Over</h2>
        <div :class="gridContainer">
            <div v-for="grid in gridTemplate" :class="gridCell" v-bind:key="grid.position" @click.once="placeShape(grid.position)">
                <span> {{ grid.shape }} </span>
            </div>
        </div>
    </main>
    <template v-else>
        <AddPlayer></AddPlayer>
    </template>
</template>

<style lang="scss">
    h2 {
        margin-bottom: 2em;
    }

    .gridContainer {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(3, 1fr);

        .gridCell {
            height: 15em;
            width: 15em;
            outline: 1px solid #222;
            margin: 5px;
            cursor: pointer;

            span {
                font-size: 10rem;
            }
        }
    }

    .buttonContainer {
        margin-top: 1em;

        button {
            margin: 10px;
        }
    }
</style>
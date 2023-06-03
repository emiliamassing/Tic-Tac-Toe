<script setup lang="ts">
    import { ref } from 'vue';
    import AddPlayer from './AddPlayer.vue';
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
    let hasWon = ref(false);
    let isTie = ref(false);
    let winningPlayer = ref();

    const gridContainer: string = 'gridContainer';
    const gridCell: string = 'gridCell';
    const buttonContainer: string = 'buttonContainer';

    const props = defineProps<IPlayerProps>();

    let activePlayer = ref(props.player[0]);
    
    function endGame() {
        gameIsRunning.value = false;
        localStorage.removeItem('playerList');
        gridTemplate.value.forEach(grid => {
            grid.shape = '';
            grid.clicked = false;
            hasWon.value = false;
            winningPlayer.value = '';
        });
    };

    function clearBoard() {
        gridTemplate.value.forEach(grid => {
            grid.shape = '';
            grid.clicked = false;
            hasWon.value = false;
            winningPlayer.value = '';
            activePlayer.value = props.player[0];
        });
    };

    function placeShape(position: number) {
        if(gridTemplate.value[position].clicked === false) {
            gridTemplate.value[position].shape = activePlayer.value.shape;
            gridTemplate.value[position].clicked = true;

            checkIfWinner();
            swapActivePlayer();
        };
    };

    function checkIfWinner() {
        for(let i = 0; i < winningPatterns.length; i++) {
            let [a, b, c] = winningPatterns[i];
            
            if(
                gridTemplate.value[a].shape && 
                gridTemplate.value[a].shape == gridTemplate.value[b].shape &&
                gridTemplate.value[a].shape == gridTemplate.value[c].shape
                
            ) {                
                hasWon.value = true;
                isTie.value = false;

                if(activePlayer.value === props.player[0]) {
                    const winner = props.player[0];
                    winningPlayer.value = winner;
    
                    markAsClicked();
                } else if(activePlayer.value === props.player[1]) {
                    const winner = props.player[1];
                    winningPlayer.value = winner;

                    markAsClicked();
                };

                activePlayer.value = props.player[0]; 
                return;
            };

            if(gridTemplate.value.every((item) => item.shape != '')) {
                hasWon.value = true;
                isTie.value = true;
            };
        };
    };

    function markAsClicked() {
        for(let i = 0; i < gridTemplate.value.length; i++) {
            gridTemplate.value[i].clicked = true;
        };
    };

    function swapActivePlayer() {
        if(activePlayer.value === props.player[0]) {
            activePlayer.value = props.player[1];
        } else {
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
        <h2>{{ props.player[0].name }} VS {{  props.player[1].name }}</h2>
        <h3 v-if="!hasWon">{{ activePlayer.name }}'s turn - Shape: {{ activePlayer.shape }}</h3>
        <template v-if="hasWon && !isTie">
            <h3>Congratulations, {{ winningPlayer.name }} has won the game!</h3>
        </template>
        <template v-else-if="hasWon && isTie">
            <h3>It's a tie!</h3>
        </template>
        <div :class="gridContainer">
            <div v-for="grid in gridTemplate" :class="gridCell" v-bind:key="grid.position" @click="placeShape(grid.position)">
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
            outline: 2px solid #4b4a4a;
            margin: 5px;
            cursor: pointer;

            span {
                font-size: 10rem;
            }
        }
    }

    .buttonContainer {

        button {
            margin: 10px;
        }
    }
</style>
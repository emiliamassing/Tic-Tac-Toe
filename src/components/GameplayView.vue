<script setup lang="ts">
    import { ref } from 'vue';
    import Player from '../models/Player';
    import { IGameBoard } from '../models/IGameBoard';
    import AddPlayer from './AddPlayer.vue';
    
    const gridTemplate: IGameBoard[] = [
        {position: 1, shape: '', clicked: false},
        {position: 2, shape: '', clicked: false},
        {position: 3, shape: '', clicked: false},
        {position: 4, shape: '', clicked: false},
        {position: 5, shape: '', clicked: false},
        {position: 6, shape: '', clicked: false},
        {position: 7, shape: '', clicked: false},
        {position: 8, shape: '', clicked: false},
        {position: 9, shape: '', clicked: false},
    ];

    const winningPatterns: number[][] = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ]

    let gameRunning = ref(true);

    const gridContainer: string = 'gridContainer';
    const gridCell: string = 'gridCell';
    const buttonContainer: string = 'buttonContainer';

    interface IPlayerProps {
        player: Player[],
    };

    const props = defineProps<IPlayerProps>();

    console.log(props.player);
    
        
    function endGame() {
        gameRunning.value = false;
        localStorage.removeItem('playerList');
    };

    function randomizeFirstPlayer(): Player {
        const randomPlayer = props.player[Math.floor(Math.random()*props.player.length)];
        console.log(randomPlayer);

        return randomPlayer;
    };

    randomizeFirstPlayer();
</script>

<template>
    <main v-if="gameRunning">
        <h2>Gameplay</h2>
        <nav :class="buttonContainer">
            <button>Show Results</button>
            <button>Clear Board</button>
            <button @click="endGame">Reset Game</button>
        </nav>
        <p>{{ props.player[0].name }} VS {{  props.player[1].name }}</p>
        <div :class="gridContainer">
            <div v-for="grid in gridTemplate" :class="gridCell" v-bind:key="grid.position">
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
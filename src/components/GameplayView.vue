<script setup lang="ts">
    import { ref } from 'vue';
    import Player from '../models/Player';
    import { IGameBoard } from '../models/IGameBoard';
    import AddPlayer from './AddPlayer.vue';
    
    const gridTemplate: IGameBoard[] = [
        {position: '1', shape: ''},
        {position: '2', shape: ''},
        {position: '3', shape: ''},
        {position: '4', shape: ''},
        {position: '5', shape: ''},
        {position: '6', shape: ''},
        {position: '7', shape: ''},
        {position: '8', shape: ''},
        {position: '9', shape: ''},
    ];

    let gameRunning = ref(true);

    const gridContainer: string = 'gridContainer';
    const gridCell: string = 'gridCell';
    const buttonContainer: string = 'buttonContainer';

    interface IPlayerProps {
        player: Player[],
    };

    const props = defineProps<IPlayerProps>();
        
    function endGame() {
        gameRunning.value = false;
        localStorage.removeItem('playerList');
    };

    function randomizeFirstPlayer(): void {
        
    };
</script>

<template>
    <main v-if="gameRunning">
        <h2>Gameplay</h2>
        <nav :class="buttonContainer">
            <button>Show Results</button>
            <button>Clear Board</button>
            <button @click="endGame">Reset Game</button>
        </nav>
        <p>{{ props.player }}</p>
        <div :class="gridContainer">
            <div v-for="grid in gridTemplate" :class="gridCell"></div>
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
        }
    }

    .buttonContainer {
        margin-top: 1em;

        button {
            margin: 10px;
        }
    }
</style>
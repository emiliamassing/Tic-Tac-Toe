<script setup lang="ts">
    import Player from '../models/Player';

    const gridPositions: string[] = [
        '1',
        '2',
        '3',
        '4',
        '5',
        '6',
        '7',
        '8',
        '9'
    ];

    const gridContainer: string = 'gridContainer';
    const gridCell: string = 'gridCell';
    const buttonContainer: string = 'buttonContainer';

    interface IPlayerProps {
        player: Player,
    };

    const props = defineProps<IPlayerProps>();

    console.log(props.player);


    function randomizeFirstPlayer(): void {
        
    };

    randomizeFirstPlayer();
    
</script>

<template>
    <h2>Gameplay</h2>
    <main>
        <p>{{ props.player }}</p>
        <div :class="gridContainer">
            <div v-for="grid in gridPositions" :class="gridCell"></div>
        </div>
        <div :class="buttonContainer">
            <button>Show Results</button>
            <button>Clear Board</button>
            <button>Reset Game</button>
        </div>
    </main>
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
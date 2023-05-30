<script setup lang='ts'>
    import { ref } from 'vue';
    import GameplayView from './GameplayView.vue';
    import Player from '../models/Player';

    const addPlayerContainer: string = 'addPlayerContainer';
    const addPlayerBtn: string = 'addPlayerBtn';

    let startGame = ref(false);
    let addedPlayers = ref<string[]>([]);

    let playerNameX = ref('');
    let playerNameO = ref('');

    function addPlayer() {

        if(addedPlayers.value.length === 0) {
            const newPlayer = new Player(playerNameX.value, 'X');
            addedPlayers.value.push(newPlayer.name);
            localStorage.setItem('PlayerList', JSON.stringify(newPlayer));
        } else {
            const newPlayer = new Player(playerNameO.value, 'O');
            addedPlayers.value.push(newPlayer.name);
            localStorage.setItem('PlayerList', JSON.stringify(newPlayer));
        };

        checkAddedPlayersLength();
    };

    function checkAddedPlayersLength() {
        if(addedPlayers.value.length === 2) {
            startGame.value = true;
        };
    };
</script>

<template>
    <div :class="addPlayerContainer" v-if="!startGame">
        <div class="playerX" v-if="addedPlayers.length === 0">
            <span>Player name X: </span>
            <label for="name">
                <input type='text' name='player' v-model="playerNameX">
            </label>
        </div>
        <div class="playerO" v-else-if="addedPlayers.length === 1">
            <span>Player name O: </span>
            <label for="name">
            <input type='text' name='player' v-model="playerNameO">
        </label>
        </div>
        <button :class="addPlayerBtn" @click="addPlayer">Add Player</button>
    </div>
    <template v-else>
        <GameplayView></GameplayView>
    </template>
</template>

<style lang='scss'>
    .addPlayerContainer {
        display: flex;
        flex-direction: column;

        label {
            margin-bottom: 1em;
        }
    }
</style>
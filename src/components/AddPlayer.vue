<script setup lang='ts'>
    import { ref } from 'vue';
    import GameplayView from './GameplayView.vue';
    import Player from '../models/Player';

    const addPlayerContainer: string = 'addPlayerContainer';
    const addPlayerBtn: string = 'addPlayerBtn';

    let startGame = ref(false);
    let players = ref<Player[]>([]);

    let playerNameX = ref('');
    let playerNameO = ref('');

    let localPlayerList = localStorage.getItem('playerList') || [];

    if(typeof localPlayerList === 'string') {
        const parse = JSON.parse(localPlayerList);
    };

    /*function addToLocalStorage(playerObject: Player) {
        let localPlayerList = localStorage.getItem('playerList') || [];

        if(typeof localPlayerList === 'string') {
            const parse = JSON.parse(localPlayerList);
        };

        [localPlayerList].push(JSON.stringify(playerObject));

        localStorage.setItem('playerList', JSON.stringify(playerObject));
    };*/

    function addPlayer() {

        if(players.value.length === 0) {
            const newPlayerX = new Player(playerNameX.value, 'X', true);
            players.value.push(newPlayerX);
            
            [localPlayerList].push(JSON.stringify(newPlayerX));
            localStorage.setItem('playerList', JSON.stringify(newPlayerX));
        } else {
            const newPlayerO = new Player(playerNameO.value, 'O', false);
            players.value.push(newPlayerO);

            [localPlayerList].push(JSON.stringify(newPlayerO));
            localStorage.setItem('playerList', JSON.stringify(newPlayerO));
        };

        checkplayersLength();
    };

    function checkplayersLength() {
        if(players.value.length === 2) {
            startGame.value = true;
        };
    };
</script>

<template>
    <div :class="addPlayerContainer" v-if="!startGame">
        <div class="playerX" v-if="players.length === 0">
            <span>Player name X: </span>
            <label for="name">
                <input type='text' name='player' v-model="playerNameX">
            </label>
        </div>
        <div class="playerO" v-else-if="players.length === 1">
            <span>Player name O: </span>
            <label for="name">
            <input type='text' name='player' v-model="playerNameO">
        </label>
        </div>
        <button :class="addPlayerBtn" @click="addPlayer">Add Player</button>
    </div>
    <template v-else>
        <GameplayView :player="players"></GameplayView>
    </template>
</template>

<style lang='scss'>
    .addPlayerContainer {
        display: flex;
        flex-direction: column;

        label {
            margin-bottom: 1em;
        }

        .addPlayerBtn {
            margin-top: 1em;
        }
    }
</style>
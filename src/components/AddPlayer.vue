<script setup lang='ts'>
    import { ref } from 'vue';
    import GameplayView from './GameplayView.vue';
    import Player from '../models/Player';

    const addPlayerContainer: string = 'addPlayerContainer';
    const addPlayerBtn: string = 'addPlayerBtn';

    let startGame = ref(false);
    const players = ref<Player[]>([]);

    let playerNameX = ref('');
    let playerNameO = ref('');

    let localPlayerList = localStorage.getItem('playerList') || [];

    if(typeof localPlayerList === 'string') {
        const parse = JSON.parse(localPlayerList);
    };

    function addPlayers() {
        players.value.push(new Player(playerNameX.value, 'X', 0), new Player(playerNameO.value, 'O', 0));
        localStorage.setItem('playerList', JSON.stringify(players.value));

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
        <div class="playerX">
            <span>Player name X: </span>
            <label for="name">
                <input type='text' name='player' v-model="playerNameX">
            </label>
        </div>
        <div class="playerO">
            <span>Player name O: </span>
            <label for="name">
            <input type='text' name='player' v-model="playerNameO">
        </label>
        </div>
        <button :class="addPlayerBtn" @click="addPlayers">Start Game</button>
    </div>
    <template v-else>
        <GameplayView :player="players"></GameplayView>
    </template>
</template>

<style lang='scss'>
    .addPlayerContainer {
        display: flex;
        flex-direction: column;

        span {
            font-size: 1.5rem;
        }

        label {
            margin-bottom: 1em;
        }

        input {
            font-family: 'Playfair', serif;
            font-size: 1.2rem;
            border-radius: 4px;
            width: 11rem;
            height: 1.4rem;
            border: 2px solid #333;
        }

        .addPlayerBtn {
            margin-top: 1em;
        }
    }
</style>
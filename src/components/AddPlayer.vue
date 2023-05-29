<script setup lang='ts'>
    const addPlayerForm: string = 'addPlayerForm';
    const addPlayerBtn: string = 'addPlayerBtn';
</script>

<template>
    <form :class='addPlayerForm'>
        <span>Player name: </span>
        <label for="name">
            <input type='text' name='player'>
        </label>
        <span>Play as: </span>
        <label for='name'>
            <input type='radio' name='shape' value='X'>
            X
        </label>
        <label for='shape'>
            <input type='radio' name='shape' value='O'>
            O
        </label>
        <button :class="addPlayerBtn">Add Player</button>
    </form>
</template>

<style lang='scss'>
    .addPlayerForm {
        display: flex;
        flex-direction: column;

        label {
            margin-bottom: 1em;
        }
    }
</style>
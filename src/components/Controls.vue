<template>
    <div class="control-wrapper">
        <Button @add-new='toggle_add_new()'></Button>
    </div>
</template>

<script>

import Button from './Controls/Button.vue'

export default {
    name: 'Controls',
    components: {
        Button
    },
    emits: ['add-new'],
    methods: {
        toggle_add_new() {
            console.log('opening add new window')
            this.$emit('add-new',true);
        }
    }
}
</script>

<style scoped>
    .control-wrapper {
        display:grid;
        place-content: center;
        font-family: Arial, Helvetica, sans-serif;
    }
</style>
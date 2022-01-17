<template>
    <div class="add-button" @click='add_new()'>
        <div class="add-button-inner">
            +
        </div>
    </div>
</template>

<script>
export default {
    name: 'Button',
    methods: {
        add_new() {
            console.log('Add new event fired.')
            this.$emit('add-new');
        }
    }
}


</script>
<style scoped>
.add-button {
        background-color: rgba(15, 184, 57, 0.74);
        width: 50px;
        height:50px;
        border-radius: 100%;
        display:grid;
        place-content: center;
        cursor: pointer;
    }

    .add-button-inner {
        font-size: 25px;
        color: white;
    }

    .add-button:hover {
        animation: 3s button-hover infinite;
    }

    @keyframes button-hover {
        0% {
            box-shadow: 0px 0px 1px grey;
        }

        50% {
            box-shadow: 0px 0px 20px grey;
        }

        100% {
            box-shadow: 0px 0px 1px grey;
        }
    }
</style>
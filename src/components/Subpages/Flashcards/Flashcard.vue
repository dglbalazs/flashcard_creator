<template>
    <div class="flashcard">
        <div class="flashcard-wrapper">
            <div :class="{rotated: clicked, 'card': true}" @click='onClick'>

                <div class="card-front">{{flashcard.description}}</div>

                <div class="card-back">{{flashcard.solution}}</div>

            </div>
        </div>
    </div>

</template>

<script>
export default {
    name: 'Flashcard',
    props: {
        flashcard: Object
    },
    data() {
        return {
            clicked : Boolean
        }
    },
    created() {
        this.clicked = false
    },
    methods: {
        onClick() {
            this.clicked = !this.clicked;
        }
    }
}
</script>

<style scoped>

.flashcard-wrapper {
    position: relative;
    width: 160px;
    height:200px;
    margin: 10px;
}

.card {
    position: absolute;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    -webkit-transform-style: preserve-3d;
    transition: all 0.5s ease;
    border-radius: 10px;
    font-size: 14px;
    cursor:pointer;
}

.card.rotated {
    transform: rotateY(180deg);
    -webkit-transform: rotateY(180deg);
}

.card-front {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius: 10px;
    overflow: auto;
    -webkit-backface-visibility: hidden; 
    background: #ffc728;
    color: #333;
    display:grid;
    padding: 35px 5px 50px 5px;
}

.card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius:10px;
    overflow:hidden;
    -webkit-backface-visibility: hidden;
    background: #fafafa;
    color: #333;
    transform: rotateY(180deg);
    -webkit-transform: rotateY(180deg);
    text-align: center;
    display: grid;
    justify-content: center;
    padding: 75px 5px 50px 5px;
}
</style>
<template>
    <div class="flashcard">
        <div class="flashcard-wrapper">
            <div :class="{rotated: clicked, 'card': true}" @click='onClick'>

                <div class="card-front">{{flashcard.description}}</div>

                <div class="card-back">{{flashcard.solution}}</div>

            </div>
        </div>
        <div class="flashcard-card-buttons" @click='delete_flashcard()'>
            <div class="flashcard-card-delete">
                Törlés
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
        },

        delete_flashcard() {
            console.log('deleting flashcard with id ' + this.flashcard.id)
            this.$emit('delete-flashcard',this.flashcard.id)
        }
    }
}
</script>

<style scoped>

.flashcard {
    margin: 10px;
}

.flashcard-wrapper {
    position: relative;
    width: 160px;
    height:200px;
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
    border-bottom-left-radius: 0px;
    border-bottom-right-radius: 0px;
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
    border-bottom-left-radius: 0px;
    border-bottom-right-radius: 0px;
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

    .flashcard-card-buttons {
        grid-area: buttons;
        display:flex;
        justify-content:center;
        align-items: center;
        font-size: 12px;
        background: var(--negative-color);
        color: white;
        border-bottom-left-radius: 15px;
        border-bottom-right-radius: 15px;
        cursor:pointer;
        transition: .4s;
        opacity: 0.85;
    }

    .flashcard-card-buttons:hover {
        outline: 1px solid grey;
        opacity:1;
        color: black;
    }

</style>
<template>
    <div class="flashcards-wrapper">
        <div class="btn back-button" @click='back()'>Vissza a gyűjteményekhez</div>
        <h3>{{this.flashcards_name}}</h3>

        <div class="flashcard-collection">
            <Flashcard 
                class
                :key="flashcard.id"
                v-for="flashcard in filtered_flashcards"
                :flashcard = "flashcard"
                @delete-flashcard ='$emit("delete-flashcard",flashcard.id)'
            
            ></Flashcard>
        </div>

    </div>
</template>

<script>
import Flashcard from './Flashcard.vue'

export default {
    name:'Flashcards',
    components: {Flashcard},
    props: {
        flashcards_name: String,
        collections: Array,
        flashcards: Array,
        collection_selected: String,
    },
    created() {
        this.fill_flashcards()
    },
    data() {
        return {
            filtered_flashcards : Array
        }
    },
    watch: {
        flashcards:  function() {
            this.fill_flashcards();
        }
    },
    methods: {
        back() {
            console.log('Going back to collections.')
            this.$emit('back-to-collections',true)
        },
        fill_flashcards() {
            this.filtered_flashcards = this.flashcards.filter((flashcard)=> flashcard.parentId == this.collection_selected)
        }
    }
}
</script>

<style scoped>

    .btn {
        color: var(--primary-font-color);
        border: none;
        padding: 10px;
        margin: 5px;
        border-radius: 5px;
        cursor: pointer;
        text-decoration: none;
        font-size: 12px;
        font-family: inherit;
        display:block;
        border-bottom-right-radius: 15px;
        border-top-right-radius: 15px;
        background:var(--negative-secondary-color);
        width:200px;
        transform: translateY(-70%);
        transition: 0.2s;
    }

    .btn:hover {
        box-shadow: 3px 3px 6px grey;
    }

    .flashcard-collection {
    margin-top:50px;
    margin-bottom:25px;
    margin-left: 50px;
    margin-right: 20px;
    display:flex;
    flex-wrap: wrap;
}

</style>
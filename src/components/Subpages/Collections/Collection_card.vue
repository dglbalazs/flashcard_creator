<template>
    <div class="collection-card">
        <div class="collection-card-header">
            <div class='number-of-cards'>Flashcardok: {{number_of_flashcard}} db</div>
        </div>
        <div class="collection-card-description" @click='chosen_collection()'>
            <div class='card-name'>{{collection.name}}</div>
            
        </div>
        <div class="collection-card-buttons" @click='delete_collection()'>
            <div class="collection-card-delete">
                Törlés
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'Collection_card',
    props: {
        collection: Object,
        flashcards: Array,
        currentId: String
    },
    methods: {
        chosen_collection() {
            console.log('chosing : ' + this.collection.id)
            this.$emit('chosen-id',this.collection.id)
        },
        delete_collection() {
            console.log('deleting : ' + this.collection.id)
            this.$emit('delete-collection',this.collection.id)
        },
    },
    data() {
        return {
            flashcard_collection : [],
            number_of_flashcard : 0
        }
    },
    async created() {
        setTimeout(()=>{
            this.flashcard_collection = this.flashcards.filter((flashcard) => flashcard.parentId == this.currentId)
            this.number_of_flashcard = this.flashcard_collection.length ? this.flashcard_collection.length : 0
        }, 500)
    }
}
</script>

<style scoped>

    .collection-card {
        display:grid;
        grid-template-columns: 200px;
        grid-template-rows: 20px 150px 30px;
        grid-template-areas: "header" "name" "buttons";

        height:200px;
        width:200px;
        margin: 25px;

        background: var(--primary-color);
        border-radius: 15px;
        box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.39);

        text-align: center;  
    }

    .collection-card-description {
        grid-area: name;
        margin:15px;
        display:grid;
        grid-template-rows: 90px 40px 20px;
        cursor:pointer;
        color: var(--primary-font-color);
        opacity:0.75;
        transition:.3s ease;
        transform: scale(0.95) translateY(15%)
    }

    .collection-card-description:hover {
        opacity:1;
        transform: scale(1);
        color: #00406B;
        font-weight: bold;
    }

    .collection-card-description .card-name {
        display:flex;
        align-items: center;
        justify-content: center;
        margin:15px;
        font-size: 16px;
    }

    .collection-card-header {
        grid-area: header;
        color: var(--primary-font-color);
    }
    .collection-card-header .number-of-cards {
        display:flex;
        align-items: center;
        justify-content: center;
        font-size:12px;
        font-style: italic;
        margin:5px;
    }

    .collection-card-buttons {
        grid-area: buttons;
        display:flex;
        justify-content:center;
        align-items: center;
        font-size: 12px;
        background: var(--negative-color);
        color: black;
        border-bottom-left-radius: 15px;
        border-bottom-right-radius: 15px;
        cursor:pointer;
        transition: .4s;
        opacity: 0.85;
    }

    .collection-card-buttons:hover {
        outline: 1px solid grey;
        opacity:1
    }
</style>
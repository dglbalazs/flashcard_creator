<template>
    <div class="window-wrapper">
        <Collections 
            
            @chosen-id="chosen_collection"
            @delete-collection="delete_collection"
            v-if="this.collection_flashcard=='collection'"
            :collections = 'collections'
        
        ></Collections>
        
        <Flashcards 
            :collections = 'collections'
            :collection_selected = 'collection_selected'
            :flashcards_name = 'flashcards_name'
            @back-to-collections='back_to_collections'
            v-if="this.collection_flashcard=='flashcard'"
            
        ></Flashcards>
        
        <NewCollection 
        
            v-if='new_collection && this.collection_flashcard=="collection"' 
            class='popup-window' 
            @remove-add-new="$emit('remove-add-new')"
            @add-new-collection="add_new_collection"

        ></NewCollection>

        <NewFlashcard 
        
            v-if='new_collection && this.collection_flashcard=="flashcard"' 
            class='popup-window' 
            @remove-add-new="$emit('remove-add-new')"
            @add-new-flashcard="add_new_flashcard"

        ></NewFlashcard>
    </div>
</template>


<script>

import Collections from './Subpages/Collections/Collections.vue'
import Flashcards from './Subpages/Flashcards/Flashcards.vue'
import NewCollection from './Controls/NewCollection.vue'
import NewFlashcard from './Controls/NewFlashcard.vue'

export default {
    components: {Collections,Flashcards,NewCollection,NewFlashcard},
    name: 'Window',
    data() {
        return {
            collection_flashcard : String,
            collection_selected : Number,
            new_collection: Boolean,
            collections: Array,
            flashcards: Array,
            flashcards_name: String,
        }
    },
    props: {
        add_new : Boolean
    },
    created() {
        this.collection_flashcard = 'collection';
        this.collection_selected = 0;
        this.new_collection = false;
        this.collections = [
            {
                id : 1,
                name: 'Example1',
                innerCard: [
                    {
                        id: 1,
                        description: 'Example description',
                        solution: 'Example solution'
                    }
                ]
            },
            {
                id : 2,
                name: 'Example2',
                innerCard: [
                    {
                        id: 1,
                        description: 'Example description',
                        solution: 'Example solution'
                    }
                ]
            },
            {
                id : 3,
                name: 'Example3',
                innerCard: [
                    {
                        id: 1,
                        description: 'Example description',
                        solution: 'Example solution'
                    },
                    {
                        id: 2,
                        description: 'Example description',
                        solution: 'Example solution'
                    }                    
                ]
            }
        ]
    },
    watch: {
        add_new: function(newVal) {
            if (newVal==true) {
                this.new_collection = true;
            } else {
                this.new_collection = false;
            }
        }
    },
    methods: {
        chosen_collection(id) {
            console.log('Opening id ' + (id) + 'collection.')
            this.flashcards_name = this.collections.filter((collection) => collection.id==id)[0].name
            this.new_collection = false;
            this.$emit('add-new-remove');
            this.collection_flashcard = 'flashcard'
            this.collection_selected = id
        },

        back_to_collections() {
            console.log('Back to collections.')
            this.flashcards_name = ''
            this.collection_flashcard = 'collection'
            this.collection_selected = 0;
        },

        add_new_collection(new_collection) {
            console.log('Adding new collection with name : ' + new_collection.name)
            this.collections = [...this.collections, new_collection]
        },

        add_new_flashcard(new_flashcard) {
            console.log('Adding new flashcard to collection with id ' + this.collection_selected)
            this.collections.filter((collection)=>collection.id==this.collection_selected)[0].innerCard = [...this.collections.filter((collection)=>collection.id==this.collection_selected)[0].innerCard,new_flashcard]
        },
        delete_collection(id) {
            if (confirm('Biztos, hogy törölni szeretnéd?')) {
                console.log('Deleting ' + id)
                this.collections = this.collections.filter((collection) => collection.id !== id)   
            }
        }
    }
}
</script>

<style scoped>

    .popup-window{
        position:fixed;
        display:grid;
        justify-content: center;
        align-items: center;
        place-content: center;
        top:0px;
        left:0px;
        height:100vh;
        width:100vw;
        background-color: rgba(29, 8, 8, 0.185);
    }

</style>
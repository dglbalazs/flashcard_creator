<template>
    <div class="window-wrapper">
        <Collections 
            
            @chosen-id="chosen_collection"
            @delete-collection="delete_collection"
            v-if="this.collection_flashcard=='collection' && (data1_loaded_up && data2_loaded_up)"
            :collections = 'collections'
            :flashcards = 'flashcards'
        
        ></Collections>
        
        <Flashcards 
            :collections = 'collections'
            :flashcards = 'flashcards'
            :collection_selected = 'collection_selected'
            :flashcards_name = 'flashcards_name'
            @back-to-collections='back_to_collections'
            @delete-flashcard = "delete_flashcard"
            v-if="this.collection_flashcard=='flashcard'  && (data1_loaded_up && data2_loaded_up)"
            
        ></Flashcards>
        
        <NewCollection 
        
            v-if='trigger_popup==true && this.collection_flashcard=="collection"' 
            class='popup-window' 
            @remove-add-new="$emit('remove-add-new')"
            @add-new-collection="add_new_collection"

        ></NewCollection>

        <NewFlashcard 
        
            v-if='trigger_popup==true && this.collection_flashcard=="flashcard"' 
            class='popup-window'
            :collection_selected = 'collection_selected'
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
            collection_selected : String,
            collections: Array,
            flashcards: Array,
            flashcards_name: String,
            data1_loaded_up: { 
                type: Boolean,
                default: false
            },
            data2_loaded_up: {
                type: Boolean,
                default: false
            }
        }
    },
    props: {
        trigger_popup : Boolean
    },
    emits: ['remove-add-new'],
    mounted() {
        this.collection_flashcard = 'collection';
        this.collection_selected = '';
        //alert('created ' + this.trigger_popup + ' , ' + this.collection_flashcard)
    },
    async created() {
        this.collections = await this.fetchCollections().then(this.data1_loaded_up==true)
        this.flashcards = await this.fetchFlashcards().then(this.data2_loaded_up == true)
    },
    methods: {

        async fetchCollections() {
            const res = await fetch ('api/collections')

            const data = await res.json()

            return data
        },
        async fetchCollection(id) {
            const res = await fetch (`api/collections/${id}`)

            const data = await res.json()

            return data
        },

        async fetchFlashcards() {
            const res = await fetch ('api/flashcards')

            const data = await res.json()

            return data
        },

        async add_new_collection(new_collection) {
            console.log('Adding new collection with name : ' + new_collection.name)
            const res = await fetch('api/collections', {
                method: 'POST',
                headers: {
                    'Content-type' : 'application/json',
                },
                body: JSON.stringify(new_collection)
            })

            const data = await res.json()

            this.collections = [...this.collections,data]
        },

        async add_new_flashcard(new_flashcard) {
            console.log('Adding new flashcard to collection with id ' + this.collection_selected)
            const res = await fetch('api/flashcards', {
                method: 'POST',
                headers: {
                    'Content-type' : 'application/json',
                },
                body: JSON.stringify(new_flashcard)            
            })

            const data = await res.json()

            this.flashcards = [...this.flashcards,data]
            
        },

        async delete_collection(id) {
            if (confirm('Biztos, hogy törölni szeretnéd?')) {
                console.log('Deleting ' + id)
                const res = await fetch(`api/collections/${id}`, {
                    method: 'DELETE'
                })

                res.status == 200 ?  this.collections = this.collections.filter((collection) => collection.id !== id) : alert('Hiba a törlés közben.')
            }
            await this.delete_flashcards(id)
        },

        async delete_flashcards(parentId) {
            console.log('Deleting flashcards with parentId' + parentId)
            this.flashcards = await this.fetchFlashcards()

            const to_delete = this.flashcards.filter((flashcard) => flashcard.parentId == parentId)

            to_delete.forEach(async flashcard => {
                let id = flashcard.id
                const res = await fetch(`api/flashcards/${id}`, {
                    method: 'DELETE'
                })    

                if (res.status != 200) {
                    console.log('Error while deleting flashcard with id : ' + id)
                }
            });
            
            this.flashcards = await this.fetchFlashcards()
        },

        async delete_flashcard(id) {
            if (confirm('Biztos, hogy törölni szeretnéd?')) {
                console.log('Deleting ' + id)
                const res = await fetch(`api/flashcards/${id}`, {
                    method: 'DELETE'
                })

                res.status == 200 ?  this.flashcards = this.flashcards.filter((flashcard) => flashcard.id !== id) : alert('Hiba a törlés közben.')
            }
        },

        chosen_collection(id) {
            console.log('Opening id ' + (id) + 'collection.')
            this.flashcards_name = this.collections.filter((collection) => collection.id==id)[0].name
            this.$emit('remove-add-new');
            this.collection_flashcard = 'flashcard'
            this.collection_selected = id
        },

        back_to_collections() {
            console.log('Back to collections.')
            this.flashcards_name = ''
            this.collection_flashcard = 'collection'
            this.$emit('remove-add-new');
            this.collection_selected = '';
        },
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
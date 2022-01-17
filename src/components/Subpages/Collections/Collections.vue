<template>
    <div class="collection-wrapper">
        <div class="page-description">
            Itt találhatóak a flashcard gyűjtemények.<br/>A lenti gombbal tudsz új flashcard csoportot létrehozni. 
        </div>
        <div class="collection-cards">
            <Collection_card 
                @chosen-id="$emit('chosen-id',collection.id)"
                @delete-collection="$emit('delete-collection',collection.id)" 
                :key="collection.id" 
                v-for="collection in collections" 
                :collection='collection'>
            </Collection_card>
        </div>
    </div>
</template>

<script>

import Collection_card from './Collection_card.vue'

export default {
    components: {Collection_card},
    name: 'Collections',
    emits: ['chosen-id','delete-collection'],
    props: {
        collections: Array
    }
}
</script>

<style>

.collection-cards {
    margin-top:50px;
    margin-bottom:25px;
    display:flex;
    flex-wrap: wrap;
}
</style>



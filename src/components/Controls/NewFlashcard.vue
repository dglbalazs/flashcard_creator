<template>
    <div class="new-flashcard-wrapper">
        <form class='add-flashcard' @submit='send_form'>
            <div class="form-control">
                <label>Definíció: </label>
                <textarea name="definition" id="definition" cols="30" rows="10" v-model='description'></textarea>
            </div>
            <div class="form-control">
                <label>Megfejtés: </label>
                <input type="text" name="text" placeholder='Megfejtés' v-model='solution' />
            </div>
            <input type='submit' value='Hozzáadás' class='btn btn-block' />
            <div class="btn btn-cancel" @click='remove_add_new'>Mégsem</div>
        </form>
    </div>
</template>

<script>
export default {
    name:'NewFlashcard',
    data() { 
        return {
            description : '',
            solution: ''
        }
    },
    props: {
        collection_selected : String
    },
    methods : {
        send_form(e) {
            e.preventDefault()

            if (!this.description || !this.solution) {
                alert('Kérlek, töltsd ki a mezőket.')
                return
            }

            const new_Flashcard = {
                description: this.description,
                solution: this.solution,
                parentId: this.collection_selected
            }

            this.$emit('add-new-flashcard',new_Flashcard);
            this.description = '';
            this.solution = '';

            
            setTimeout(() => this.remove_add_new(), 500)
        },

        remove_add_new() {
            console.log('removing')
            this.$emit('remove-add-new')
        }
    }
}
</script>

<style scoped>
    form {
        display:grid;
        grid-template-rows: 150px 100px 50px 50px;
        padding: 50px;
    }

    textarea { 
        margin-top: 5px;
        height: 100px;
        resize: none;
        font-family: 'Readex Pro', sans-serif;
    }

    .add-flashcard { 
        background:white;
        border-radius: 18px;
        box-shadow: 5px 5px 50px grey;
    }

    .form-control {
        margin: 20px 0;
    }

    .form-control label{ 
        display: block;
    }

    .form-control input {
        width: 100%;
        height: 30px; 
        margin: 5px 0;
        padding: 10px 10px;
        font-size : 16px;
        border: none;
        outline: 1px solid black;
    }

    .btn {
        color: #fff;
        border: none;
        padding: 10px 20px;
        margin: 5px;
        border-radius: 5px;
        cursor: pointer;
        text-decoration: none;
        font-size: 12px;
        font-family: inherit;
        display:block;
        width:100%;
    }
    .btn:focus {
    outline: none;
    }
    .btn:active {
    transform: scale(0.98);
    }
    .btn-block {
        background: #000;
    }
    .btn-cancel {
        background: var(--negative-secondary-color)
    }
</style>
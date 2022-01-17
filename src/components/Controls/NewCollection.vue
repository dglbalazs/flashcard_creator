<template>
    <div class="new-collection-wrapper">
        <form class='add-collection' @submit='send_form'>
            <div class="form-control">
                <label>Gyűjtemény neve: </label>
                <input type="text" name="text" placeholder='Új gyűjtemény' v-model='name' />
            </div>
            <input type='submit' value='Hozzáadás' class='btn btn-block' />
            <div class="btn btn-cancel" @click='remove_add_new'>Mégsem</div>
        </form>
    </div>
</template>

<script>
export default {
    name:'NewCollection',
    data() { 
        return {
            name : '',
        }
    },
    methods : {
        send_form(e) {
            e.preventDefault()

            if (!this.name) {
                alert('Kérlek, töltsd ki a mezőt.')
                return
            }

            const new_Collection = {
                id: 5,
                name: this.name,
                innerCard: []

            }

            this.$emit('add-new-collection',new_Collection);
            this.name = '';


            
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
        grid-template-rows: 100px 50px;
        padding: 50px;
    }

    .add-collection { 
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
        margin: 10px;
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
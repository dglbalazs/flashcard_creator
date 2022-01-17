<template>

  <Window 
  
    class='window' 
    :trigger_popup="trigger_popup" 
    @remove-add-new='remove_add_new()'
  
  ></Window>
  
  <Controls 
  
    class='controls' 
    @add-new='add_new()'
  
  ></Controls>

</template>

<script>
import Window from './components/Window.vue'
import Controls from './components/Controls.vue'

export default {
  name: 'App',
  components: {
    Window,
    Controls
  },
  data() {
    return {
      trigger_popup : Boolean
    }
  },
  methods: {
    add_new() {
      console.log('akarmi')
      this.trigger_popup = true;
      console.log(this.trigger_popup)
    },
    remove_add_new() {
      this.trigger_popup = false;
    }
  },
  created() {
    document.title = 'Flashcard Cucc';
    this.trigger_popup = false;
  },

}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Readex+Pro:wght@300&display=swap');

:root {
  --primary-color: #fab047;
  --secondary-color: #5F625F;
  --negative-color: #fff;
  --negative-secondary-color: #FF3D88;
  --primary-font-color: #fff;
  --secondary-font-color: #717564;
  --primary-background-color: #f3f0e9;
  --primary-color-card: #00406B;
  --secondary-color-card: #FFBC5D;
}

@media only screen and (max-width: 600px) {
  body:after{
    content:'Ne telón nézd, hanem gépen x) Így nemjó. ';
    position:absolute;
    top:0px;
    left:0px;
    width:100%;
    height:50px;
    display:grid;
    place-content: center;
    color:white;
    font-size:15px;
    text-align:center;
    background-color: red;
    font-family: 'Readex Pro', sans-serif;
  }
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin:0px;
  background-color: var(--primary-background-color);
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}

body::-webkit-scrollbar {
    display: none;
}

#app {
  font-family: 'Readex Pro', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width:100%;
}

.window{
  margin-bottom: 100px;
}

.controls {
  background: rgb(223, 222, 222);
  position:fixed;
  bottom:0px;
  width:100%;
  padding: 15px 0px;
  margin:0px;
}

</style>

<template>
    <!-- Código HTML -->
    <div class="SearchBar">
        <form v-on:submit.prevent>
            <input v-model="nameP" type="text">
            <input @click="ReturnValue" type="submit" value=" buscar">
        </form>
    </div>
</template>

<script>
//Defincion de componente estilo Option API Vue2+ y Vue3
//import NombreComponente from './components/NombreComponente.vue'
export default {

    name:"seeker-p",
    data(){
        return{
            nameP:null
        }
    },
    // Lista de funciones (métodos) disponibles en el componente Vue.
    methods: {
        ReturnValue(){
            this.$emit('search',this.nameP);
        }
    },
};
</script>

<style>
/* Estilos CSS */
</style>
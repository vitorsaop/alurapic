<template>
    <nav>
      <ul>
        
        <li v-for="rota in rotas">
          <router-link :to="rota.path ? rota.path : '/'"> {{ rota.titulo }} </router-link>
        </li>
        
      </ul>      
    </nav>       
</template>

<script>

export default {
    
    data(){
        return {
            count: 0
        }      
    },

    props: {
        rotas : {
            type: Array,
            required: true
        }
    }

}
</script>

<style scoped>

</style>
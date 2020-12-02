<template>
  <div class="corpo">   

    <my-menu :rotas="routes"> </my-menu>

    <transition name="pagina">   
      <router-view></router-view>
    </transition>
  </div>
</template>

<script>

import { routes } from './routes';
import Menu from './components/shared/menu/Menu.vue';

export default { 

  components:{
    'my-menu' : Menu
  },
  
  data() {
    return {
      routes : routes.filter(route => route.menu)     
    }
  }
}
</script>



<style>
  .corpo{
    font-family: Helvetica, sans-serif;
    width: 96%;
    margin: auto;
  }

.pagina-enter, .pagina-leave-active {
  opacity: 0;
}

.pagina-enter-active, .pagina-leave-active {
  transition: opacity .4s;;
}

</style>

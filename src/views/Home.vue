<template>
  <div class="home">
    <h1 id="title">Monster combat</h1>
    <Arena id="arena"/>
    <Hud id="hud" />
  </div>
</template>

<style lang="css">
  .home {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 10px;
    grid-auto-rows: minmax(100px, auto);
  }
  #title {
    text-align: center;
    grid-column: 1 / 3;
    grid-row: 1;
  }

  #arena {
    grid-column: 1 / 3;
    grid-row: 2;
  }

  #hud {
    grid-column: 1 / 3;
    grid-row: 3;
  }
</style>

<script>
  // @ is an alias to /src
  import Arena from '@/components/Arena.vue'
  import Hud from '@/components/Hud.vue'

  export default {
    name: 'home',
    components: {
      Arena,
      Hud
    }
  }
</script>
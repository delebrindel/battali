<template>
    <div>
      <div id="arena-inner">
        <Character id="arena-inner--player" type="player" maxHp="100"/>
        <Character id="arena-inner--monster" type="monster"  maxHp="150"/>
      </div>
    </div>
</template>

<script>
import Character from '@/components/Character.vue'
export default {
  name: 'Arena',
  components: {
    Character
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#arena-inner{
    display: grid;
    grid-template-columns: 2fr 1fr 2fr;
    grid-gap: 10px;
    grid-auto-rows: minmax(100px, auto);
}

  #arena-inner--player {
    grid-column: 1 / 2;
    grid-row: 1;
  }

  #arena-inner--monster {
    grid-column: 3 / 4;
    grid-row: 1;
  }
</style>

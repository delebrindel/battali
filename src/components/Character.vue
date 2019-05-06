<template>
    <div class="character" :class="type">
        {{hp}} / {{maxHp}}
    </div>
</template>

<script>
export default {
  name: 'Character',
  props: {
    type: String,
    maxHp: String
  },
  data:()=>{
    return{
      hp: 0,
      rage: 0
    }
  },
  mounted(){
    this.hp = parseInt(this.maxHp);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>

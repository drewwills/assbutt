<template>
  <div class="assbutt">

    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
import phaser from 'phaser';

export default {
  name: 'Assbutt',
  props: {
    msg: String
  },
  created() {

    const preload = function() {
    }

    const create = function() {
    }

    const update = function() {
    }


    var config = {
      type: Phaser.AUTO,
      width: 600,
      height: 800,
      scene: {
        preload: preload,
        create: create,
        update: update
      }
    };
    var game = new Phaser.Game(config);


  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

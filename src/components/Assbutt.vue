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

    let ground;
    let player;
    let assbutts;
    let cursors;

    const preload = function() {
      this.load.image('background', '/img/background_600x800.png');
      this.load.image('ground', '/img/ground_1000x30.png');
      this.load.image('flower', '/img/flower_100x97.png');
      this.load.image('assbutt', '/img/assbutt_72x75.png');
    }

    const create = function() {
      this.add.image(300, 400, 'background');
      ground = this.physics.add.staticGroup();
      ground.create(200, 725, 'ground');
      player = this.physics.add.sprite(100, 450, 'flower')
          .setBounce(0.2)
          .setCollideWorldBounds(true);
      this.physics.add.collider(player, ground);
      assbutts = this.physics.add.group({
        key: 'assbutt',
        repeat: 0,
        setXY: { x: 12, y: 0 }
      });
      cursors = this.input.keyboard.createCursorKeys();
    }

    const update = function() {
      if (cursors.left.isDown) {
        player.setVelocityX(-160);
      } else if (cursors.right.isDown) {
        player.setVelocityX(160);
      } else {
        player.setVelocityX(0);
      }
//      if (player.body.touching.down) {
//        console.log('I\'m touching it!');
//      }
    }

    var config = {
      type: Phaser.AUTO,
      width: 600,
      height: 800,
      physics: {
        default: 'arcade',
        arcade: {
          gravity: { y: 300 },
          debug: false
        }
      },
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

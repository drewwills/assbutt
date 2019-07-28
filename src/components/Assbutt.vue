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

    const SCENE_WIDTH = 600;
    const SCENE_HEIGHT = 800;

    const PLAYER_VELOCITY = 250;

    const SPAWN_INITIAL_DELAY = 7000;
    const SPAWN_X_POS_MIN = 10;
    const SPAWN_X_POS_RANGE = 580;
    const SPAWN_ACCELERATION = 157;

    const ASSBUTT_VELOCITY_Y = -350;

    let ground;
    let player;
    let cursors;

    let nextSpawn = 7000;

    const playerCollide = function(assbutt, player) {
      const direction = assbutt.body.position.x - player.body.position.x;
      const velocityX = direction * 10;
      assbutt.setVelocityX(velocityX);
      assbutt.setVelocityY(ASSBUTT_VELOCITY_Y);
    }

    const spawnAssbutt = function() {
      // Spawn an assbutt above the scene and at a random point on the X axis.
      const posX = Math.floor(Math.random() * SPAWN_X_POS_RANGE) + SPAWN_X_POS_MIN;
      let assbutt = this.physics.add.sprite(posX, -100, 'assbutt');
      this.physics.add.collider(assbutt, player, playerCollide);

      // Set the next assbutt to spawn sooner than the last
      if (nextSpawn > 0) {
        nextSpawn -= SPAWN_ACCELERATION;
        this.time.addEvent({ delay: nextSpawn, callback: spawnAssbutt, callbackScope: this});
      }
    }

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
      cursors = this.input.keyboard.createCursorKeys();
      this.time.addEvent({ delay: nextSpawn, callback: spawnAssbutt, callbackScope: this});
    }

    const update = function() {
      if (cursors.left.isDown) {
        player.setVelocityX(-PLAYER_VELOCITY);
      } else if (cursors.right.isDown) {
        player.setVelocityX(PLAYER_VELOCITY);
      } else {
        player.setVelocityX(0);
      }
    }

    var config = {
      type: Phaser.AUTO,
      width: SCENE_WIDTH,
      height: SCENE_HEIGHT,
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

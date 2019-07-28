<template>
  <div id="gameContainer" class="assbutt">
    <h1>{{ title }}</h1>
    <h2>{{ subtitle }}</h2>
  </div>
</template>

<script>
import phaser from 'phaser';

export default {
  name: 'Assbutt',
  props: {
    title: String,
    subtitle: String
  },
  created() {

    const SCENE_WIDTH = 600;
    const SCENE_HEIGHT = 800;

    const PLAYER_VELOCITY = 250;

    const SPAWN_INITIAL_DELAY = 7000;
    const SPAWN_ACCELERATION = 227;

    const ASSBUTT_VELOCITY_Y = -350;

    let ground;
    let player;
    let cursors;

    let nextSpawn = 7000;

    let score = 0;
    let scoreText;

    const playerCollide = function(assbutt, player) {
      score += 10;
      scoreText.setText('Score: ' + score);
      const direction = assbutt.body.position.x - player.body.position.x;
      const velocityX = direction * 10;
      assbutt.setVelocityX(velocityX);
      assbutt.setVelocityY(ASSBUTT_VELOCITY_Y);
    }

    const groundCollide = function(assbutt, ground) {
      const posX = assbutt.body.position.x;
      if (posX < 0 || posX > SCENE_WIDTH) {
        // Assbutt bounced to safety...
        assbutt.disableBody(true, true);
      } else {
        // Game over
        this.physics.pause();
        player.setTint(0xff0000);
        this.add.text(SCENE_WIDTH / 2.25, SCENE_HEIGHT / 4, 'Game Over!', { fontSize: '48px', fill: '#f00' });
      }
    }

    const spawnAssbutt = function() {
      // Spawn an assbutt above the scene and at a random point on the X axis.
      let assbutt = this.physics.add.sprite(Phaser.Math.Between(10, SCENE_WIDTH - 10), -100, 'assbutt');
      this.physics.add.collider(assbutt, player, playerCollide, null, this);
      this.physics.add.collider(assbutt, ground, groundCollide, null, this);

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
      scoreText = this.add.text(16, 16, 'score: 0', { fontSize: '24px', fill: '#333' });
      ground = this.physics.add.staticGroup();
      ground.create(200, 750, 'ground');
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

      this.physics.overlap(player, ground, function() {
        player.setVelocityY(-100);
      });
    }

    var config = {
      parent: 'gameContainer',
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
  h1 {
    margin: 0;
  }
  h2 {
    margin: .25rem;
  }
</style>

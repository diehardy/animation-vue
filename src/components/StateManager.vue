<template>
  <div>
    <select name="states" @change="setState" v-model="selectedState">
      <option value="idle">idle</option>
      <option value="jump">jump</option>
      <option value="fall">fall</option>
      <option value="run">run</option>
      <option value="earth">earth</option>
      <option value="lie">lie</option>
      <option value="circle">circle</option>
      <option value="bite">bite</option>
    </select>
    <canvas id="canvas-animation" ref="canvas1" width="600" height="600"></canvas>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      selectedState: 'idle',
      spriteAnimatons: [
        { frames: 6, name: 'idle', positionY: 0 },
        { frames: 6, name: 'jump', positionY: 1 },
        { frames: 6, name: 'fall', positionY: 2 },
        { frames: 8, name: 'run', positionY: 3 },
        { frames: 10, name: 'earth', positionY: 4 },
        { frames: 4, name: 'lie', positionY: 5 },
        { frames: 6, name: 'circle', positionY: 6 },
        { frames: 6, name: 'bite', positionY: 7 },
      ],
      chosenState: null,
      // frames
      frameX: 0,
      frameY: null,
      totalFrames: null,
      // canvas control
      ctx: null,
      playerImage: new Image(),
      // sprite size
      spriteHeight: 525,
      spriteWidth: 575,
      // speed settings
      gameFrame: 0,
      divider: 5

    }
  },
  methods: {
    getState(stateName, statesArray) {
      let chosenState = statesArray.find((state) => state.name == stateName)
      return chosenState
    },


    animate() {
      this.ctx.clearRect(0, 0, 600, 600)
      this.ctx.drawImage(this.playerImage, this.frameX * this.spriteWidth, this.frameY * this.spriteHeight, this.spriteWidth, this.spriteHeight, 0, 0, this.spriteWidth, this.spriteHeight)
      if (this.gameFrame % this.divider === 0) {
        if (this.frameX < this.totalFrames) this.frameX++;
        else this.frameX = 0;
      }
      this.gameFrame++
      requestAnimationFrame(this.animate)
    },


    setState() {
      this.chosenState = this.getState(this.selectedState, this.spriteAnimatons)
      this.frameY = this.chosenState.positionY
      this.totalFrames = this.chosenState.frames
    }

  },
  created() {
    this.chosenState = this.getState('idle', this.spriteAnimatons);
    this.frameY = this.chosenState.positionY;
    this.totalFrames = this.chosenState.frames
  },
  mounted() {
    this.ctx = this.$refs.canvas1.getContext('2d');
    this.playerImage.src = require('../assets/shadow_dog.png')
    this.animate()
  }

}
</script>
<style scoped>
body {
  background-color: aqua;
}

#canvas-animation {
  border: 5px solid black;
  display: block;
  margin: 0 auto;
  margin-top: 20px;
}

select {
  display: block;
  margin: 0 auto;
}
</style>

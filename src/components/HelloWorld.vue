<template>
<div>
  <v-stage ref="stage" :config="{
      width: 411,
      height: 800
    }" >
    <v-layer :config="{name: 'layer',x: 0,y: 0,}">
      <v-image :config="tableBottomBg"></v-image>
      <v-image :config="coinTableImg"></v-image>
    </v-layer>
    <v-layer ref="layer">
      <v-regular-polygon ref="hexagon" :config="{
          x: 200,
          y: 100,
          sides: 6,
          radius: 20,
          fill: 'red',
          stroke: 'black',
          strokeWidth: 4
        }" />
    </v-layer>
    <v-layer>
    <v-regular-polygon :config="{
      x: 100,
      y: 100,
      sides: 5,
      radius: 70,
      fill: 'red',
      stroke: 'black',
      strokeWidth: 4,
      shadowOffsetX : 20,
      shadowOffsetY : 25,
      shadowBlur : 40,
      opacity : 0.5
    }" />
  </v-layer>
  
  </v-stage>
  <BetListPopLayer @betListHide="betListHide" v-if="betListFlag"></BetListPopLayer>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  mounted() {
    const vm = this;
    const amplitude = 100;
    const period = 5000;
    // in ms
    const centerX = vm.$refs.stage.getStage().getWidth() / 2;

    const anim = new Konva.Animation(function(frame) {
      vm.$refs.hexagon
        .getStage()
        .setX(
          amplitude * Math.sin((frame.time * 2 * Math.PI) / period) + centerX
        );
    }, vm.$refs.layer.getStage());

    anim.start();
  },
  methods: {
    handleDragstart(starComponent) {
      const shape = starComponent.getStage();
      const dragLayer = vm.$refs.dragLayer.getStage();
      const stage = vm.$refs.stage.getStage();
      // moving to another layer will improve dragging performance
      shape.moveTo(dragLayer);
      stage.draw();
      starComponent.config.shadowOffsetX = 15;
      starComponent.config.shadowOffsetY = 15;
      starComponent.config.scaleX = starComponent.config.startScale * 1.2;
      starComponent.config.scaleY = starComponent.config.startScale * 1.2;
    },
    betListHide() {
      //投注记录隐藏
      this.betListFlag = false;
    }
  },
  computed: {
    tableBottomBg() {
      let img = new Image();
      img.src = "/static/img/bg_table_long-min.png";
      return {
        y: this.height,
        width: 411,
        height: 731,
        image: img
      };
    },
    coinTableImg() {
      let img = new Image();
      img.src = "/static/img/chips-min.png";
      return {
        y: 600,
        width: 411,
        height: 81,
        image: img
      };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 0px;
}
a {
  color: #42b983;
}
</style>

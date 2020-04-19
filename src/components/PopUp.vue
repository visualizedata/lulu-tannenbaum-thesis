<template>
  <div id="popup">
    <button class="close" v-on:click="onClose">X</button>
    <h3>{{name}}</h3>
    <div class="flex-container">
      <div>
        <video
          id="yang"
          controls
          :src="`/videos/${name}.mp4`"
          type="video/mp4"
          :poster="`thumbnails/${name}.png`"
        />
        <Tags v-bind:tags="tags" />
      </div>
      <AnimatedLineChart
        id="yang"
        v-if="content"
        :brainData="content"
        :reportXScale="reportXScale"
      />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import AnimatedLineChart from './AnimatedLineChart'
import Tags from './Tags'

export default {
  name: 'PopUp',
  components: {
    AnimatedLineChart,
    Tags,
  },
  props: ['content', 'onClose'],
  computed: {
    name: function() {
      return _.get(_.head(this.content), 'name')
    },
    tags: function() {
      return _.split(_.get(_.head(this.content), 'tags'), '-')
    },
  },
  methods: {
    reportXScale(xScale, animationOverlay) {
      this.registerVideoPlayback('yang', xScale, animationOverlay)
    },
    registerVideoPlayback(id, xScale, animationOverlay) {
      const videoElement = document.getElementById(id)
      videoElement.ontimeupdate = e => {
        console.log(e.timeStamp, videoElement.currentTime)
        animationOverlay
          .transition()
          .duration(250)
          .attr('x', xScale(videoElement.currentTime))
      }
    },
  },
}
</script>

<style>
#popup {
  height: 100vh;
  background-color: white;
  /* background: url('../assets/screenshot.png'); */
  color: black;
  text-align: left;
  padding-left: 63px;
  padding-top: 60px;
  position: fixed;
  top: 0px;
  left: 0px;
}

#popup h3 {
  margin-bottom: 4rem;
}
#popup .flex-container {
  display: flex;
}
#popup .flex-container > div {
  flex: 1;
}
#popup video {
  width: 92%;
  height: auto;
  margin-bottom: 2.5rem;
}
</style>

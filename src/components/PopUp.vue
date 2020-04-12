<template>
  <div id="popup">
    <h3>{{name}}</h3>
    <div class="flex-container">
      <div>
        <video id="yang" controls :src="`/videos/${name}.mp4`" type="video/mp4" />
        <div id="tags">
          <span v-for="issue in issues" v-bind:key="issue">{{issue}}</span>
        </div>
      </div>
      <AnimatedLineChart v-if="content" :brainData="content" :reportXScale="reportXScale" />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import AnimatedLineChart from './AnimatedLineChart'

export default {
  name: 'PopUp',
  components: {
    AnimatedLineChart,
  },
  props: ['content'],
  computed: {
    name: function() {
      return _.get(_.head(this.content), 'name')
    },
    issues: function() {
      return _.uniq(
        _.filter(
          _.flatMap(this.content, c => c.issues),
          i => !!i
        )
      )
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
#popup #tags span {
  padding: 0.2rem 0.5rem;
  border-radius: 1rem;
  border: black solid 1px;
  margin: 0.25rem;
}
</style>

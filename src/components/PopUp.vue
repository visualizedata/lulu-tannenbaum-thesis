<template>
  <div id="popup">
    <button class="close" v-on:click="onClose">X</button>
    <h3>{{name}}</h3>
    <div class="flex-container">
      <div>
        <video
          :id="id"
          controls
          :src="`/videos/${videoName}.mp4`"
          type="video/mp4"
          :poster="`thumbnails/${videoName}.png`"
        />
        <Tags v-bind:tags="tags" />
      </div>
      <AnimatedLineChart :id="id" v-if="content" :brainData="content" :reportXScale="reportXScale" />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import AnimatedLineChart from './AnimatedLineChart'
import Tags from './Tags'
import { select, bisector, format } from 'd3'

const bisectOffset = bisector(d => d.offset).left
const findCurrentScore = (time, brainData) => {
  const closesIdx = bisectOffset(brainData, time)
  return _.get(brainData[closesIdx], 'NES')
}
const percentageFormatter = format('+.2')

export default {
  name: 'PopUp',
  components: {
    AnimatedLineChart,
    Tags,
  },
  props: ['content', 'onClose'],
  computed: {
    id: function() {
      return _.get(_.head(this.content), 'contentId')
    },
    name: function() {
      return _.get(_.head(this.content), 'name')
    },
    videoName: function() {
      return _.get(_.head(this.content), 'videoName')
    },
    tags: function() {
      return _.split(_.get(_.head(this.content), 'tags'), '-')
    },
  },
  methods: {
    reportXScale(id, brainData, xScale, animationOverlay) {
      this.registerVideoPlayback(id, brainData, xScale, animationOverlay)
    },
    registerVideoPlayback(id, brainData, xScale, animationOverlay) {
      const videoElement = document.getElementById(id)
      videoElement.ontimeupdate = () => {
        animationOverlay
          .transition()
          .duration(250)
          .attr('x', xScale(videoElement.currentTime))
        const NES = findCurrentScore(videoElement.currentTime, brainData)
        select(`[id="${this.id}"] .time`).html(`${percentageFormatter(NES)}%`)
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

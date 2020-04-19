<template>
  <div id="analysis">
    <h3>
      {{title}}
      <button class="more-info" v-on:click="onMoreInfoClick">i</button>
    </h3>
    <div class="flex-container">
      <div>
        <p>{{description}}</p>
        <video
          :id="id"
          controls
          :src="`/videos/${name}.mp4`"
          type="video/mp4"
          :poster="`thumbnails/${name}.png`"
        />
        <Tags :tags="tags" />
      </div>
      <AnimatedLineChart v-if="content" :brainData="content" :reportXScale="reportXScale" :id="id" />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import AnimatedLineChart from './AnimatedLineChart'
import Tags from './Tags'

export default {
  name: 'Analysis',
  components: {
    AnimatedLineChart,
    Tags,
  },
  computed: {
    name: function() {
      return _.get(_.head(this.content), 'name')
    },
    tags: function() {
      return _.split(_.get(_.head(this.content), 'tags'), '-')
    },
  },
  props: ['content', 'id', 'title', 'description', 'onMoreInfoClick'],
  methods: {
    reportXScale(id, xScale, animationOverlay) {
      this.registerVideoPlayback(id, xScale, animationOverlay)
    },
    registerVideoPlayback(id, xScale, animationOverlay) {
      const videoElement = document.getElementById(id)
      videoElement.ontimeupdate = () => {
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
#analysis {
  height: 100vh;
  background-color: white;
  /* background: url('../assets/screenshot.png'); */
  color: black;
  text-align: left;
  padding-left: 63px;
  padding-top: 60px;
  position: relative;
}
#analysis .more-info {
  font-size: 1.25rem;
  font-weight: bold;
  -webkit-appearance: none;
  border-radius: 50%;
  border: 1px solid black;
  height: 1.75rem;
  width: 1.75rem;
  vertical-align: middle;
}

#analysis h3 {
  margin-bottom: 4rem;
}
#analysis .flex-container {
  display: flex;
}
#analysis .flex-container > div {
  flex: 1;
}
#analysis video {
  width: 92%;
  height: auto;
  margin-bottom: 2.5rem;
}
#analysis #tags span {
  padding: 0.2rem 0.5rem;
  border-radius: 1rem;
  border: black solid 1px;
  margin: 0.25rem;
}
</style>

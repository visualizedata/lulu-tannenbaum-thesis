<template>
  <div id="explore">
    <h3>Explore the Data</h3>
    <div>
      <video
        v-for="ad in iterativeAds"
        v-bind:key="ad.name"
        class="video"
        :src="`/videos/${ad.videoName}.mp4`"
        type="video/mp4"
        v-on:click="onClick(ad.content_id)"
        :poster="`thumbnails/${ad.videoName}.png`"
      />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  name: 'Explore',
  props: ['onClick', 'ads'],
  computed: {
    iterativeAds: function() {
      return _.map(this.ads.entries(), ({ key, value }) => ({
        content_id: key,
        name: _.get(_.head(value), 'name'),
        videoName: _.get(_.head(value), 'videoName'),
      }))
    },
  },
}
</script>

<style>
#explore {
  background-color: white;
  text-align: left;
  padding-top: 200px;
  padding-left: 63px;
  position: relative;
  padding-bottom: 200px;
}

#explore h3 {
  color: #305581;
  margin: 40px 0px;
  width: 90%;
}

#explore video {
  width: 200px;
  height: 110px;
  margin: 10px;
  cursor: pointer;
}
</style>

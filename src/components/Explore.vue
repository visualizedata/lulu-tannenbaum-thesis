<template>
  <div id="explore">
    <h3>Explore the Data</h3>
    <div>
      <video
        v-for="ad in iterativeAds"
        v-bind:key="ad.name"
        class="video"
        :src="`/videos/${ad.name}.mp4`"
        type="video/mp4"
        v-on:click="onClick(ad.content_id)"
        :poster="`thumbnails/${ad.name}.png`"
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

#explore p {
  width: 60%;
  line-height: 125%;
  color: #305581;
}

#explore #img-row {
  position: absolute;
  bottom: 100px;
}

#explore img:nth-of-type(1) {
  width: 115px;
  margin-right: 30px;
}
#explore img:nth-of-type(2) {
  width: 135px;
}

#explore #analysis {
  display: flex;
}

#explore #analysis > div {
  flex: 1;
}

#explore #analysis video {
  width: 100%;
}

#explore video {
  width: 200px;
  height: auto;
  margin: 10px;
}
</style>

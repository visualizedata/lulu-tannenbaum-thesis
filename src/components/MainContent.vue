<template>
  <div id="maincontent">
    <h3>
      <strong>People subconsciously</strong> engage with messaging about The Economy, Healthcare, and Climate Change, but
      <strong>not</strong> to messaging about Immigration or Gun Control.
    </h3>
    <div id="analysis">
      <div>
        <h3>Some Title</h3>
        <video controls src="/videos/Audi Presents- Let It Go.mp4" type="video/mp4" />
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris luctus et nisl ut porttitor. Praesent magna ante, suscipit eget convallis nec, sollicitudin non neque. Duis et tincidunt sem, quis semper elit. Praesent efficitur eleifend mauris a rutrum. In nec sem metus. Mauris dignissim bibendum bibendum. Pellentesque dolor nunc, rhoncus in aliquet vitae, vestibulum vitae nibh. Pellentesque ornare lacus a molestie consequat.</p>
      </div>
      <div>
        <svg />
      </div>
    </div>
    <h3>Explore the Data</h3>
    <div>
      <video
        v-for="videoName in videoNames"
        v-bind:key="videoName"
        class="video"
        :src="`/videos/${videoName}`"
        type="video/mp4"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Main Content',
  data() {
    return {
      videoNames: [
        'Andrew Yang - Right Now.mp4',
        'Audi Presents- Let It Go.mp4',
        'Bones | Joe Biden for President.mp4',
        'Donald J Trump - Stronger, Safer, More Prosperous.mp4',
        'Fight From the Heart (Warren for President Campaign Ad).mp4',
        'George | Mike Bloomberg for President.mp4',
        'Greta Thunberg Tells Davos Delegates to Act as If You Loved Your Children.mp4',
        'Mike Bloomberg - Trump Disrespects Our Troops.mp4',
        'NFL Media Originals - We Are in This Together.mp4',
        'OLAY® #MAKESPACEFORWOMEN | SUPER BOWL LIV OFFICIAL COMMERCIAL.mp4',
        'Pete Buttigieg explains why Trump should be removed from office.mp4',
        "SE Cupp- Trump might be impeached, but he's unleashed.mp4",
        'SodaStream Discovers Water On Mars.mp4',
        'The Courage to Change | Alexandria Ocasio-Cortez.mp4',
        'Trump team uses stock photo models in reelection ads.mp4',
        'VISION | Bernie Sanders 2020.mp4',
        'Watch Nancy Pelosi Rip Up Copy Of President Donald Trump’s State Of The Union Speech | NBC News.mp4',
      ],
    }
  },
}
</script>

<style>
#maincontent {
  background-color: white;
  text-align: left;
  padding-top: 200px;
  padding-left: 63px;
  position: relative;
  padding-bottom: 200px;
}

#maincontent h3 {
  color: #305581;
  margin: 40px 0px;
  width: 90%;
}

#maincontent p {
  width: 60%;
  line-height: 125%;
  color: #305581;
}

#maincontent #img-row {
  position: absolute;
  bottom: 100px;
}

#maincontent img:nth-of-type(1) {
  width: 115px;
  margin-right: 30px;
}
#maincontent img:nth-of-type(2) {
  width: 135px;
}

#maincontent #analysis {
  display: flex;
}

#maincontent #analysis > div {
  flex: 1;
}

#maincontent #analysis video {
  width: 100%;
}

#maincontent video {
  width: 200px;
  height: auto;
  margin: 10px;
}
</style>

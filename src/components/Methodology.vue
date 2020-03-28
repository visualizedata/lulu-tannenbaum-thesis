<template>
  <div id="methodology">
    <h2>Methodology</h2>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris luctus et
      nisl ut porttitor. Praesent magna ante, suscipit eget convallis nec,
      sollicitudin non neque. Duis et tincidunt sem, quis semper elit. Praesent
      efficitur eleifend mauris a rutrum. In nec sem metus. Mauris dignissim
      bibendum bibendum. Pellentesque dolor nunc, rhoncus in aliquet vitae,
      vestibulum vitae nibh. Pellentesque ornare lacus a molestie consequat.
      Suspendisse porta ligula et vulputate fermentum. Vivamus accumsan ornare
      mi non euismod. Pellentesque imperdiet urna quis lobortis cursus. Nunc
      accumsan, diam vitae eleifend sollicitudin, elit elit maximus ligula, eu
      rutrum orci nisi vel neque. Aenean nisl lorem, placerat vitae dolor at,
      pharetra fringilla ligula.
    </p>
    <p>
      Morbi vestibulum dolor quis justo elementum placerat. Duis blandit laoreet
      tellus vel euismod. Donec venenatis leo ut lorem tempus, et rutrum massa
      ornare. Aenean venenatis ante ac neque fringilla, ultrices aliquam nulla
      elementum. Aliquam erat volutpat. Morbi vel mi eget est interdum volutpat
      eu ac est. Donec vitae malesuada nulla, eleifend vulputate lacus. Sed sit
      amet facilisis tellus, eget placerat nisi.Aenean venenatis ante ac neque
      fringilla, ultrices aliquam nulla elementum. Aliquam erat volutpat. Morbi
      vel mi eget est interdum volutpat eu ac est. Donec vitae malesuada nulla,
      eleifend vulputate lacus. Sed sit amet facilisis tellus, eget placerat
      nisi.
    </p>
    <div id="img-row">
      <img src="../assets/brainsightsLogo.png" alt="brainsights logo" />
      <img src="../assets/parsonsLogo.png" alt="parsons logo" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Methodology',
}
</script>

<style>
#methodology {
  text-align: left;
  padding-top: 60px;
  padding-left: 63px;
  height: 100vh;
  position: relative;
}

#methodology h2 {
  margin: 40px 0px;
}

#methodology p {
  width: 60%;
  line-height: 125%;
}

#methodology #img-row {
  position: absolute;
  bottom: 100px;
}

#methodology img:nth-of-type(1) {
  width: 115px;
  margin-right: 30px;
}
#methodology img:nth-of-type(2) {
  width: 135px;
}
</style>

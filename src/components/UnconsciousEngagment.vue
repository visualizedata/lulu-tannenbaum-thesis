<template>
  <div id="unconscious">
    <h3>
      <strong>People subconsciously</strong> engage with messaging about The Economy, Healthcare, and Climate Change, but
      <strong>not</strong> to messaging about Immigration or Gun Control.
    </h3>
    <h3>
      How does the economy
      <em>resonate unconsciously?</em>
    </h3>
    <p>On aggregate...</p>
    <h2>Lulu to-do ...</h2>
  </div>
</template>

<script>
export default {
  name: 'Unconscious',
  data() {
    return {}
  },
}
</script>

<style>
#unconscious {
  background-color: white;
  text-align: left;
  padding-top: 200px;
  padding-left: 63px;
  position: relative;
  padding-bottom: 200px;
}

#unconscious h3 {
  color: #305581;
  margin: 40px 0px;
  width: 90%;
}

#unconscious h2 {
  color: #305581;
  margin: 40px 0px;
  width: 90%;
}

#unconscious p {
  width: 60%;
  line-height: 125%;
  color: #305581;
}

#unconscious #img-row {
  position: absolute;
  bottom: 100px;
}

#unconscious img:nth-of-type(1) {
  width: 115px;
  margin-right: 30px;
}
#unconscious img:nth-of-type(2) {
  width: 135px;
}

#unconscious #analysis {
  display: flex;
}

#unconscious #analysis > div {
  flex: 1;
}

#unconscious #analysis video {
  width: 100%;
}

#unconscious video {
  width: 200px;
  height: auto;
  margin: 10px;
}
</style>

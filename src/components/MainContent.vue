<template>
  <div id="maincontent">
    <h3>
      <strong>People subconsciously</strong> engage with messaging about The Economy, Healthcare, and Climate Change, but
      <strong>not</strong> to messaging about Immigration or Gun Control.
    </h3>
  </div>
</template>

<script>
export default {
  name: 'Main Content',
}
</script>

<style>
#maincontent {
  background-color: white;
  text-align: left;
  padding-top: 60px;
  padding-left: 63px;
  height: 100vh;
  position: relative;
}

#maincontent h3 {
  color: #305581;
  margin: 40px 0px;
  width: 60%;
}

#maincontent p {
  width: 60%;
  line-height: 125%;
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
</style>

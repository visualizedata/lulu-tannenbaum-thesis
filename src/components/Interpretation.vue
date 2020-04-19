<template>
  <div id="interpretation" class="full-height">
    <button class="close" v-on:click="onClose">X</button>
    <div>
      <h3>What does the data mean?</h3>
      <p>Neural Engagement...</p>
    </div>
    <div>
      <img src="../assets/readTheData.png" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Interpretation',
  props: ['onClose'],
}
</script>

<style>
#interpretation {
  background-color: white;
  text-align: left;
  display: flex;
  padding-top: 150px;
  padding-left: 63px;
  position: fixed;
  left: 0px;
  top: 0px;
}

#interpretation > div {
  flex: 50%;
  max-width: 50%;
}

#interpretation img {
  max-width: 100%;
}

#interpretation h3 {
  color: #305581;
  margin: 40px 0px;
  width: 90%;
}
#interpretation p {
  color: #305581;
}
</style>

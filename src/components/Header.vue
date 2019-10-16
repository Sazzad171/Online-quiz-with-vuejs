<template>
  <div>
    <div class="headNav">
      <b-nav>
        <b-nav-item disabled><b>Online Quiz Test</b></b-nav-item>
        <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
      </b-nav>

      <br>

      <h3 :class="[numTotal===10 ? 'displayShow' : 'displayHide']">You got {{ numCorrect }}0% marks</h3>
    </div>
  </div>
</template>


<script>
export default {
  name: 'headNav',
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>


<style scoped>
.headNav {
  margin: 1rem 0;
}
h3 {
  text-align: center;
}
.displayShow {
    display: block;
  }
  .displayHide {
    display: none;
  }
</style>
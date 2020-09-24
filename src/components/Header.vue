<template>
  <div>
    <b-nav tabs class="d-flex justify-content-between">
        <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
        <b-nav-item disabled>Counter: {{indexQuestion + 1}}-10</b-nav-item>
        <b-nav-item disabled>Correct Answers: {{ corrects }}/10</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
  export default{
    props:[
      'indexQuestion',
      'corrects'
    ]  
  }
</script>

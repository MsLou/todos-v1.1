<template>
  <div id="app">
    <todos-head></todos-head>
    <todos-body></todos-body>
  </div>
</template>
<script>
import todosHead from './components/title/title'
import todosBody from './components/todosBody/check'

export default {
  components: {
    todosHead,
    todosBody
  }
}
</script>
<style>
html {
  height: 100%;
}
* {
  margin:0;
  padding:0;
}
li {
  list-style: none;
}
body {
  background-color: #f5f5f5;
  display: -webkit-box;
  display: box;
}

#app {
  margin:50px auto;
  max-height: 550px;
  -webkit-box-align: center;
  color: #2c3e50;
  width: 600px;
  font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

#app a {
  color: #42b983;
  text-decoration: none;
}

.logo {
  width: 100px;
  height: 100px
}
</style>

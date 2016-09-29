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
  height: 100%;
}

#app {
  margin:50px auto;
  color: #2c3e50;
  max-width: 600px;
  position:relative;
  font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1); 
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

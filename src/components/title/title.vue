<template id="todos-head">
	<div class="todos-title">
		<h1>{{text}}</h1>
	</div>
</template>

<script>
export default {
  data () {
    return {
      text: 'todos'
    }
  }
}
</script>

<style>
	.todos-title {
		color: rgba(175, 47, 47, 0.15);
		font-size: 50px;
	    text-align: center;
	}
	.todos-title h1 {
		font-weight: 100;
	}
</style>

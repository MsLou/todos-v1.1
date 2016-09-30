<template>
	<div class="todos-foot">
		<span class="activelength">{{activeList.length}}:Item left</span>
		<ul>
			<li><span :class="handleBtn[0] == true ? 'foot-active' : null" @click.sync="handleAll">All</span></li>
			<li><span :class="handleBtn[1] == true ? 'foot-active' : null" @click.sync="handleActive">Active</span></li>
			<li><span :class="handleBtn[2] == true ? 'foot-active' : null" @click.sync="hendleCompleted">Completed</span></li>
		</ul>
		<button class="clear-completed" @click="clearCompleted($index)">Clear completed</button>
	</div>
</template>
<script>
	export default {
	  props: ['activeList'],
	  data () {
	    return {
	      handleBtn: [true, false, false],
	      activelength: 0
	    }
	  },
	  methods: {
	    clearCompleted (index) {
	      this.$parent.clearComplete(index)
	    },
	    handleAll () {
	      this.handleTive(0)
	      this.$dispatch('setList', null, 'All')
	    },
	    handleActive () {
	      this.handleTive(1)
	      this.$dispatch('setList', this.activeList, 'Active')
	    },
	    hendleCompleted () {
	      this.handleTive(2)
	      this.$dispatch('setList', this.completedList, 'Completed')
	    },
	    handleTive (item) {
	      let list = []
	      this.handleBtn.forEach(function (val, index) {
	        if (index === item) {
	          val = true
	        } else {
	          val = false
	        }
	        list.push(val)
	      })
	      this.handleBtn = list
	    }
	  }
	}
</script>
<style>
	.todos-foot {
		background-color: #fff;
		position: relative;
		line-height: 33px;
		color: #777;
		border-bottom: 1px solid #ebebeb;
	}
	.todos-foot ul {
		position: absolute;
		width: 100%;
		top: 1px;
		text-align: center;
	}
	.todos-foot ul li {
		display: inline-block;
	}
	.todos-foot ul li span {
		padding:3px;
		cursor: pointer;
		border:1px solid #fff;
	}
	.todos-foot ul li span:hover {
		background-color: #ebebeb;
		border:1px solid #ddd;
	}
	.foot-active {
		background-color: #ebebeb;
		border:1px solid #ddd;
	}
	.todos-foot .clear-completed {
		position: absolute;
		right: 5px;
		top: 9px;
		background-color: transparent;
		outline:none;
		border:none;
	}
	.todos-foot .clear-completed:hover {
		text-decoration: underline;
	}

</style>
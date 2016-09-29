<template>
	<li v-for="l in list" v-on:mouseover="handleOver" v-on:mouseout="handleOut" data-index="{{$index}}">
		<span :class="l.toggle == true ? 'toggle bCheck' : 'toggle aCheck'" class="toggle aCheck" v-on:click="check" data-index="{{$index}}"></span>
		<label :class="l.toggle == true ? 'tived' : null" data-index="{{$index}}">{{l.text}}</label>
		<button class="destroy" v-on:click="checkDestroy" v-if="l.show" data-index="{{$index}}">X</button>
	</li>
</template>
<script>
	export default {
	  props: ['list'],
	  data () {
	    return {
	      arr: []
	    }
	  },
	  methods: {
	    handleOver (current, event) {
	      let index = Number(current.target.getAttribute('data-index'))
	      this.$parent.overShow(index)
	    },
	    handleOut (current) {
	      let index = Number(current.target.getAttribute('data-index'))
	      this.$parent.outHide(index)
	    },
	    check (current) {
	      let classes = current.target.className
	      let next = classes.indexOf('aCheck') > -1 ? ' bCheck' : ' aCheck'
	      let index = Number(current.target.getAttribute('data-index'))
	      current.target.className = classes.replace(/\saCheck$|\sbCheck$/gi, next)
	      // this.list[index].toggle = !this.list[index].toggle
	      this.$parent.setToggle(index)
	    },
	    checkDestroy (current) {
	      let index = Number(current.target.getAttribute('data-index'))
	      this.$parent.destroy(index)
	    }
	  }
	}
</script>
<style>
	.tived {
		text-decoration: underline;
		color: #d9d9d9;
	}
	.check-list > li {
		position: relative;
		border-bottom: 1px solid #ededed;
	}
	.check-list .toggle {
		width: 51px;
		height: 65px;
		display: block;
		float: left;
	}
	.check-list label {
		float:left;
		line-height: 65px;
		white-space: pre-line;
    word-break: break-all;
    margin-left: 45px;
    width: 454px;
    text-align: left;
    transition: color 0.4s;
    font-size: 24px;
	}
	.check-list .destroy {
		position: absolute;
		right: 10px;
		top:15px;
		font-size: 24px;
		outline: none;
		border:none;
		background-color: transparent;
	}
</style>
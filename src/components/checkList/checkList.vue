<template>
  <li class="todos-list" v-for="l in list" @dblclick="listDbclick($index)">
    <span :class="l.toggle == true ? 'toggle bCheck' : 'toggle aCheck'" class="toggle aCheck" @click="check($index)"></span>
    <label :class="l.toggle == true ? 'tived' : null">{{l.text}}</label>
    <input type="text" class="editText" v-show="l.isEdit" v-is-focus="l.isEdit" v-model="l.text" @keyup.enter="editEnter($index)" @blur="editEnter($index)" />
    <button class="destroy" @click="checkDestroy($index)">X</button>
  </li>
</template>
<script>
  export default {
    props: ['list'],
    directives: {
      'is-focus' (boo) {
        this.el.focus()
      }
    },
    methods: {
      check (index) {
        this.$dispatch('setToggle', index)
      },
      checkDestroy (index) {
        this.$dispatch('destroy', index)
      },
      listDbclick (index) {
        this.$dispatch('checkEdit', true, index)
      },
      editEnter (index) {
        this.$dispatch('checkEdit', false, index)
      }
    }
  }
</script>
<style>
  .tived {
    text-decoration: line-through;
    color: #d9d9d9;
  }
  .check-list > li {
    position: relative;
    height: 66px;
    border-bottom: 1px solid #ededed;
  }
  .check-list .toggle {
    width: 50px;
    height: 65px;
    position: absolute;
    top: 0;
    left: 0;
  }
  .check-list label, .check-list .editText {
    line-height: 65px;
    text-align: left;
    transition: color 0.4s;
    font-size: 24px;
    padding:0 0 0 55px;
    display: block;
  }
  .check-list .editText {
    line-height: 61px;
    text-indent: 52px;
    margin-left: 1px;
    top:0;
    padding: 0;
    width: 595px;
    position: absolute;
    display: block;
    z-index: 22;
  }
  .check-list .destroy {
    position: absolute;
    right: 10px;
    top:15px;
    font-size: 24px;
    outline: none;
    border:none;
    display: none;
    background-color: transparent;
  }
  .todos-list:hover .destroy {
    display: block;
  }
  .editText {

  }
</style>
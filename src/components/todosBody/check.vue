<template>
  <div>
    <div class="check">
      <span class="check-all {{checkAllType}}" v-on:click="checkAll"></span>
      <div class="check-input">
        <input type="text" id="check-append" placeholder="What needs to be done?" v-on:keyup.enter="checkEnter" />
      </div>
    </div>
    <ul class="check-list">
      <check-list :list="checkList" :active-list="aList" :completed-list="cList" ></check-list>
    </ul>
  </div>
  <handle :type="type" :check-list="checkList" :active-list="aList" :completed-list="cList"></handle>
</template>
<script>
  import checkList from './../checkList/checkList'
  import handle from './../todosFoot/foot'
  let part = function (move, index) {
    let isAB = move.toggle === true ? 'Active' : 'Completed'
    let activeList = []
    let completeList = []
    for (var i = 0; i < index; i++) {
      if (this.checkList[i].toggle) {
        completeList.push(this.checkList[i])
      } else {
        activeList.push(this.checkList[i])
      }
    }
    return {
      completeList: completeList,
      activeList: activeList,
      isAB: isAB
    }
  }
  let convent = function (list, val) {
    if (toString.call(list) === '[object Array]') {
      return list.map(function (current, index) {
        current.toggle = val
        return current
      })
    }
  }
  export default {
    data () {
      return {
        // toggle = false = active; toggle = true = completed
        // checkList: []
        type: 'All',
        // aList: [{text: 'act1', toggle: false}],
        // cList: [{text: 'com1', toggle: true}],
        aList: [],
        cList: [],
        checkList: [],
        checkAllType: 'checkall'
      }
    },
    ready () {
      // this.checkList = this.aList.concat(this.cList)
    },
    watch: {
      aList () {
        return this.aList.filter(function (current, index) {
          if (!current.toggle) {
            return current
          } else {
            return false
          }
        })
      },
      cList () {
        return this.cList.filter(function (current, index) {
          if (current.toggle) {
            return current
          } else {
            return false
          }
        })
      }
    },
    methods: {
      overShow (index) {
        this.checkList[Math.max(0, index)].show = true
      },
      outHide (index) {
        this.checkList[Math.max(0, index)].show = false
      },
      clearComplete (index) {
        console.log('clear!!')
        this.cList = []
        let list = []
        this.checkList.forEach(function (current, index) {
          if (!current.toggle) {
            list.push(current)
          }
        })
        this.checkAllType = 'checkall'
        this.checkList = list
      },
      checkAll () {
        // 如果为空则不切换
        if (this.aList.length === 0 && this.cList.length === 0) return false
        // 切换为Active
        if (this.checkAllType === 'checkalled') {
          this.checkAllType = 'checkall'
          let temporay = convent(this.cList, false)
          this.aList = this.aList.concat(temporay)
          this.cList = []
        } else {  // 切换为Completed
          this.checkAllType = 'checkalled'
          let temporay = convent(this.aList, true)
          this.cList = this.cList.concat(temporay)
          this.aList = []
        }
        if ((this.type === 'Active' && this.checkAllType === 'checkalled') || (this.type === 'Completed' && this.checkAllType === 'checkall')) {
          this.checkList = []
        }
      },
      check () {
        console.log(this)
      },
      checkEnter (current) {
        let obj = {
          text: current.target.value,
          toggle: false,
          show: false
        }
        current.target.value = null
        switch (this.type) {
          case 'All':
            this.aList.push(obj)
            break
          case 'Active':
            this.aList.push(obj)
            break
          case 'Completed':
            this.cList.push(obj)
            break
        }
        // this.activeList.push(this.activeList[this.activeList.length] + 1)
        this.checkList.push(obj)
      },
      destroy (index) {
        let move = this.checkList[index]
        let isAB = move.toggle === true ? 'Completed' : 'Active'
        let activeList = []
        let completeList = []
        this.checkList.splice(index - 1 === -1 ? 0 : index, 1)
        for (var i = 0; i < index; i++) {
          if (this.checkList[i].toggle) {
            completeList.push(this.checkList[i])
          } else {
            activeList.push(this.checkList[i])
          }
        }
        if (isAB === 'Active') {
          this.aList.splice(activeList.length, 1)
        } else {
          this.cList.splice(completeList.length, 1)
        }
      },
      // 切换视图
      setList (checkListObj, type) {
        this.type = type
        switch (this.type) {
          case 'All':
            this.checkList = this.aList.concat(this.cList)
            break
          case 'Active':
            this.checkList = this.aList
            break
          case 'Completed':
            this.checkList = this.cList
            break
        }
      },
      // 点击复选框
      setToggle (index) {
        // console.log(index)
        let move = this.checkList[index]
        move.toggle = !move.toggle
        let activeComplete = part.call(this, move, index)
        // 移动
        if (activeComplete.isAB === 'Active') {
          this.cList.push(move)
          this.aList.splice(activeComplete.activeList.length, 1)
          // console.log(this.aList)
        } else {
          console.log(index)
          this.aList.push(move)
          this.cList.splice(activeComplete.completeList.length, 1)
          // console.log(this.cList)
        }
        if (this.cList.length === this.checkList.length) {
          this.checkAllType = 'checkalled'
        } else {
          this.checkAllType = 'checkall'
        }
      }
    },
    components: {
      checkList,
      handle
    }
  }
</script>
<style>
  .check:after,.check-list > li:after {
    content: "";
    display: block;
    clear: both;
  }
  .check > * {
    float:left;
  }
  .check .check-all {
    width: 51px;
    height: 65px;
    display: block;
  }
  .checkall {
    background: url(./../../assets/checkall.png);
  }
  .checkalled {
    background: url(./../../assets/checkalled.png);
  }
  .aCheck {
    background: url(./../../assets/check.png);
  }
  .bCheck {
    background: url(./../../assets/checked.png);
  }
  .check-input > input {
    color: #4d4d4d;
    width: 549px;
    height: 65px;
    outline: none;
    border:none;
    font-size: 25px;
  }
  .check-list {
    background-color: #fff;
    border-top: 1px solid #ededed;
  }
</style>
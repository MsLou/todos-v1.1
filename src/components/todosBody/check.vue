<template>
  <div class="todosBody">
    <div class="check">
      <span class="check-all {{checkAllType}}" @click="checkAll"></span>
      <div class="check-input">
        <input type="text" id="check-append" placeholder="What needs to be done?" @keyup.enter="checkEnter" />
      </div>
    </div>
    <ul class="check-list">
      <check-list :list="checkList"></check-list>
    </ul>
    <handle v-if="isHandle" :type="type" :check-list="checkList" :active-list="activeList"></handle>
  </div>
</template>
<script>
  import checkList from './../checkList/checkList'
  import handle from './../todosFoot/foot'
  let currentIndex = function (type, index) {
    let a = 0
    // type的索引
    let b = 0
    let totalCheck = this.totalCheck
    for (var h = 0; h < totalCheck.length; h++) {
      if (totalCheck[h].toggle === type) {
        if (b === index) break
        b++
      } else {
        a++
      }
    }
    return a + b
  }
  let appiontFilter = function () {
    return this.totalCheck.filter(function (current, index) {
      if (!current.toggle) {
        return current
      }
    })
  }
  export default {
    data () {
      return {
        // toggle = false = active; toggle = true = completed
        type: 'All',
        checkAllType: 'checkall',
        activeList: [],
        totalCheck: [],
        isHandle: false
      }
    },
    computed: {
      checkList () {
        let list = []
        switch (this.type) {
          case 'All':
            list = this.totalCheck
            break
          case 'Active':
            list = this.totalCheck.filter(function (current, index) {
              if (!current.toggle) {
                return current
              }
            })
            break
          case 'Completed':
            list = this.totalCheck.filter(function (current, index) {
              if (current.toggle) {
                return current
              }
            })
            break
        }
        return list
      }
    },
    events: {
      // 切换视图
      setList (checkListObj, type) {
        this.type = type
        switch (this.type) {
          case 'All':
            this.checkList = this.totalCheck
            break
          case 'Active':
            this.checkList = this.checkList.filter(function (current, index) {
              if (!current.toggle) {
                return current
              } else {
                return false
              }
            })
            break
          case 'Completed':
            this.checkList = this.checkList.filter(function (current, index) {
              if (current.toggle) {
                return current
              } else {
                return false
              }
            })
            break
        }
        this.countActiveList()
      },
      // 点击复选框
      setToggle (index) {
        let a = 0
        switch (this.type) {
          case 'All':
            this.totalCheck[index].toggle = !this.totalCheck[index].toggle
            break
          case 'Active':
            a = currentIndex.call(this, false, index)
            this.totalCheck[a].toggle = !this.totalCheck[a].toggle
            break
          case 'Completed':
            a = currentIndex.call(this, true, index)
            this.totalCheck[a].toggle = !this.totalCheck[a].toggle
            break
        }
        this.countCheckAllType()
        this.countActiveList()
      },
      checkEdit (type, index) {
        this.totalCheck[index].isEdit = type
      },
      destroy (index) {
        let a = 0
        switch (this.type) {
          case 'All':
            this.totalCheck.splice(index, 1)
            break
          case 'Active':
            a = currentIndex.call(this, false, index)
            this.totalCheck.splice(a, 1)
            break
          case 'Completed':
            a = currentIndex.call(this, true, index)
            this.totalCheck.splice(a, 1)
            break
        }
        if (this.totalCheck.length === 0) {
          this.isHandle = false
        }
        this.countActiveList()
      }
    },
    methods: {
      countActiveList () {
        this.activeList = appiontFilter.call(this)
      },
      countCheckAllType () {
        for (var t = 0; t < this.totalCheck.length; t++) {
          if (!this.totalCheck[t].toggle) {
            this.checkAllType = 'checkall'
            return
          }
        }
        if (t === this.totalCheck.length) {
          this.checkAllType = 'checkalled'
        }
      },
      clearComplete (index) {
        this.totalCheck = appiontFilter.call(this)
        this.checkAllType = 'checkall'
        this.countActiveList()
      },
      checkAll () {
        let toggled
        if (this.checkAllType === 'checkall') {
          toggled = true
          this.checkAllType = 'checkalled'
        } else if (this.checkAllType === 'checkalled') {
          toggled = false
          this.checkAllType = 'checkall'
        }
        this.checkList = this.totalCheck.map(function (current, index) {
          current.toggle = toggled
          return current
        })
        this.countActiveList()
      },
      checkEnter (current) {
        let val = current.target.value
        if (val === '' || val === null) return
        let obj = {
          text: val,
          toggle: false,
          isEdit: false
        }
        current.target.value = null
        this.isHandle = true
        this.totalCheck.push(obj)
        this.checkList = this.totalCheck
        this.countActiveList()
      }
    },
    components: {
      checkList,
      handle
    }
  }
</script>
<style>
  .todosBody {
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1)
  }
  .check:after,.check-list > li:after {
    content: "";
    display: block;
    clear: both;
  }
  .check > * {
    float:left;
  }
  .check .check-all {
    width: 10%;
    height: 65px;
    display: block;
  }
  .check .check-input {
    width: 90%;
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
    width: 100%;
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
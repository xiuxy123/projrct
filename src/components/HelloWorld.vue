<template>
  <div class="hello" >
    <table class="Table">
      <tr>
        <th class="th0"> <input type="checkbox" :checked='ischeckBox' @click="changecheck" /></th>
        <th  v-for="(item, index) in columns" v-bind:key="index" v-bind:class="item.class">
          {{ item.text }}</th>
      </tr>
      <tr v-for="item in arrList" v-bind:key="item.num">
        <td><input type="checkbox" :key='item.num' :checked='item.ischecked' v-model="item.ischecked" /></td>
        <td  @click="listOnclick" > {{ item.Date }}</td>
        <td  @click="listOnclick" > {{ item.num }}</td>
        <td  @click="listOnclick" > {{ item.Description }}</td>
        <td  @click="listOnclick" > {{ item.Total }}</td>
        <td v-bind:class="{'delete': isdelete, 'textcolor': !isdelete}" @click="deleteItem(item.num)">删除</td>
      </tr>
      <tr  v-bind:class="{'delete': checkOnclick, 'textcolor': !checkOnclick}">
        <td colspan="6" @click="deleteItems">删除</td>
      </tr>
    </table>
    <div class="footer">
    <ul class="pagination">
        <li @click="addPage('<')" class="pagination_li">&lt;</li>
        <li
        v-for ="n in list"
        :key="n" @click="addPage(n)"
        class="pagination_li">
        {{ n }}
        </li>
        <li @click="addPage('>')"  class="pagination_li">&gt;</li>
      </ul>
      <input class="search" placeholder="Search" v-model="search" />
    </div>
    <div class="add">
        &nbsp; Date: &nbsp; <input type="date" v-model="invalue.Date" />
        &nbsp; Description: &nbsp; <input type="text" v-model="invalue.Description" />
        &nbsp; Total: &nbsp; <input type="number" step="0.01" v-model="invalue.Total" />
        <input type="button" value="添加" @click = "addlist" />
    </div>
  </div>
</template>

<script>
let moment = require('moment')
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      columns: [
        {
          text: 'Date',
          class: 'th1'
        },
        {
          text: '#',
          class: 'th2'
        },
        {
          text: 'Description',
          class: 'th3'
        },
        {
          text: 'Total',
          class: 'th4'
        }
      ],
      arr: [
        {
          Date: moment(Date()).format('DD/MM/YYYY'),
          num: 1,
          Description: 'Alpha Bravo Charlie ',
          Total: 26.00,
          ischecked: false
        },
        {
          Date: '15/02/2021',
          num: 2,
          Description: 'Alpha Bravo Charlie ',
          ischecked: false,
          Total: 26.00
        },
        {
          Date: '14/02/2021',
          num: 3,
          Description: 'Alpha Bravo Charlie ',
          ischecked: false,
          Total: 26.00
        },
        {
          Date: '13/02/2021',
          num: 4,
          Description: 'Alpha Bravo Charlie ',
          ischecked: false,
          Total: 26.00
        },
        {
          Date: '12/02/2021',
          num: 5,
          Description: 'Alpha Bravo Charlie ',
          ischecked: false,
          Total: 26.00
        }
      ],
      ischeckBox: false,
      isdelete: true,
      page: 1,
      list: [1, 2],
      search: '',
      // arrList: [],
      invalue: { Date: '', num: 0, Description: '', Total: '' }
    }
  },
  computed: {
    arrList: function () {
      var list = []
      var count = (this.page - 1) * 5
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.arr.sort((a, b) => b.Date.localeCompare(a.Date))
      if (this.search === '') {
        for (var i = count; i < count + 5; i++) {
          if (this.arr[i]) {
            list.push(this.arr[i])
          }
        }
        return list
      } else {
        // eslint-disable-next-line no-redeclare
        for (var i = 0; i < this.arr.length; i++) {
          if (this.arr[i].Description.indexOf(this.search) !== -1 ||
          this.arr[i].Total.toString().indexOf(this.search) !== -1) {
            list.push(this.arr[i])
          }
        }
        return list
      }
    },
    checkOnclick: function () {
      for (var i = 0; i < this.arr.length; i++) {
        if (this.arr[i].ischecked === true) {
          return false
        }
      }
      return true
    }
  },

  methods: {
    addlist () {
      if (this.invalue.Date === '') {
        this.invalue.Date = Date()
      }
      this.invalue.Date = moment(this.invalue.Date).format('DD/MM/YYYY')
      this.invalue.num = this.arr[this.arr.length - 1].num + 1
      this.arr.push({ ...this.invalue, ischecked: false })
      this.arr.sort((a, b) => b.Date.localeCompare(a.Date))

      for (var j = 0; j < this.arr.length; j++) {
        this.arr[j].num = j + 1
      }

      this.invalue = { Date: '', Description: '', num: 0, Total: '' }
    },

    listOnclick () {
      this.isdelete = !this.isdelete
      if (this.isdelete === false && this.columns.length < 5) {
        this.columns.push({ text: 'action', class: 'th4' })
      }
      if (this.isdelete && this.columns.length === 5) {
        this.columns.pop()
      }
    },

    deleteItem (item) {
      this.arr.splice(item - 1, 1)
      for (let i = item - 1; i < this.arr.length; i++) {
        this.arr[i].num--
      }
    },
    deleteItems () {
      for (var i = this.arr.length - 1; i >= 0; i--) {
        if (this.arr[i].ischecked === true) {
          this.arr.splice(i, 1)
        }
      }
      for (var j = 0; j < this.arr.length; j++) {
        this.arr[j].num = j + 1
      }
    },

    addPage (str) {
      if (str === '<' && this.page > 1) {
        this.page--
        for (var i = 0; i < 2; i++) {
          this.list[i]--
        }
      } else if (str === '>' && this.page < ((this.arr.length) / 5) + 1) {
        this.page++
        for (var j = 0; j < 2; j++) {
          if (this.list[1] < ((this.arr.length) / 5) + 1) {
            this.list[j]++
          }
        }
      } else if (!isNaN(str)) {
        this.page = Number(str)
      }
    },

    arrlength (number) {
      if (number) {
        if (this.arr) {
          this.list.push(parseInt(number / 5))
          this.list.push(parseInt(number / 5) + 1)
        }
      } else {
        if (this.arr) {
          this.list.push(parseInt(this.arr.length / 5))
          this.list.push(parseInt(this.arr.length / 5) + 1)
        }
      }
    },
    changecheck () {
      this.ischeckBox = !this.ischeckBox
      this.isallDelete = !this.isallDelete
      for (var i = 0; i < this.arr.length; i++) {
        this.arr[i].ischecked = this.ischeckBox
      }
    }
    // eslint-disable-next-line vue/no-dupe-keys
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello{
  width: 60%;
  margin-left: 20%;
  box-shadow: 5px 5px 5px rgb(214, 214, 214) ;
}
.footer{
  display: flex;
  border: 2px solid rgb(214, 214, 214);
  border-top: 1px solid rgb(214, 214, 214);
}
.search{
  width: 60%;
  border: 0;
  border-left: 1px solid rgb(214, 214, 214);
}
.search:focus{
  outline: none;
}
.add{
  display: flex;
  border: 2px solid rgb(214, 214, 214);
  border-top: 1px solid  rgb(214, 214, 214);
}
.add input:focus{
  outline: none;
}
ul{
  padding: 0;
  margin: 0;
}
ul li{
  list-style-type: none;
  color: black;
  float: left;
  padding: 8px 16px;
}
.pagination_li{
  cursor: pointer;
}
.pagination_li:hover{
  background-color: coral;
}
.pagination_li:active{
  background-color: coral;
}
tr{
  height: 40px;
  border: 1px solid rgb(222, 222, 222);
}
.th0{
  width: 5%;
}
.th1{
  width: 15%;
  font-family: '楷体';
  color: rgb(158, 140,133);
}
.th2{
  width: 10%;
  font-family: '楷体';
  color: rgb(158, 140,133);
}
.th3{
  width: 50%;
  font-family: '楷体';
  color: rgb(158, 140,133);
}
.th4{
  width: 15%;
  font-family: '楷体';
  color: rgb(158, 140,133);
}
.delete{
  display: none;
}
.textcolor{
 cursor: pointer;
 color: crimson;
}
.textcolor:hover{
  color: coral;
}

.Table{
  width: 100%;
  border: 2px solid  rgb(214, 214, 214);
  border-bottom: 1px solid  rgb(214, 214, 214);
  border-collapse: collapse;
}
</style>

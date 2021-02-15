<template>
  <div class="hello" >
    <table class="Table">
      <tr>
       <th  v-for="(item, index) in columns" v-bind:key="index" v-bind:class="item.class"> {{ item.text }}</th>
      </tr>
      <tr v-for="(item,index) in arrList" v-bind:key="index" >
        <td> {{ item.Date }} </td>
        <td> {{ item.num }}</td>
        <td> {{ item.Description }} </td>
        <td> {{ item.Total }} </td>
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
          Total: 26.00
        },
        {
          Date: moment(Date()).format('DD/MM/YYYY'),
          num: 2,
          Description: 'Alpha Bravo Charlie ',
          Total: 26.00
        },
        {
          Date: moment(Date()).format('DD/MM/YYYY'),
          num: 3,
          Description: 'Alpha Bravo Charlie ',
          Total: 26.00
        },
        {
          Date: moment(Date()).format('DD/MM/YYYY'),
          num: 4,
          Description: 'Alpha Bravo Charlie ',
          Total: 26.00
        },
        {
          Date: moment(Date()).format('DD/MM/YYYY'),
          num: 5,
          Description: 'Alpha Bravo Charlie ',
          Total: 26.00
        }
      ],
      page: 1,
      list: [1, 2],
      search: '',
      // arrList: [],
      invalue: { Date: '', num: 0, Description: '', Total: '' },
      index: 0
    }
  },
  computed: {
    reversedMessage: function () {
      return (
        this.msg.split('').reverse().join('')
      )
    },
    arrList: function () {
      var list = []
      var count = (this.page - 1) * 5
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
    }
  },

  methods: {
    addlist () {
      if (this.invalue.Date === '') {
        this.invalue.Date = Date()
      }
      this.invalue.Date = moment(this.invalue.Date).format('DD/MM/YYYY')
      this.invalue.num = this.arr[this.arr.length - 1].num + 1
      this.arr.push(this.invalue)
      this.invalue = { Date: '', Description: '', num: 0, Total: '' }
    },

    deleteItem (item) {
      for (var i = 0; i < this.arr.length; i++) {
        var index = this.arr[i].Description.indexOf(item)
      }
      if (index > 1) {
        this.arr.splice(index, 1)
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
  width: 60%;
  font-family: '楷体';
  color: rgb(158, 140,133);
}
.th4{
  width: 10%;
  font-family: '楷体';
  color: rgb(158, 140,133);
}
.Table{
  width: 100%;
  border: 2px solid  rgb(214, 214, 214);
  border-bottom: 1px solid  rgb(214, 214, 214);
  border-collapse: collapse;
}
</style>

<template>
  <div>
    <el-row>
      <el-input-number v-model="num" :min="1" label="预算"></el-input-number>
      <el-button type="primary" icon="el-icon-edit" circle @click="addRow"></el-button>
    </el-row>
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="name" label="产品名称" width="120">
        <template slot-scope="scope">
          <el-input @blur="scope.row.nameShow = !scope.row.nameShow" v-if="scope.row.nameShow" v-model="scope.row.name"></el-input>
          <span v-else @click="scope.row.nameShow = !scope.row.nameShow">{{scope.row.name}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="price" label="产品单价" width="120">
        <template slot-scope="scope">
          <el-input @blur="scope.row.priceShow = !scope.row.priceShow" v-if="scope.row.priceShow" v-model="scope.row.price"></el-input>
          <span v-else @click="scope.row.priceShow = !scope.row.priceShow">{{scope.row.price}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="count" label="数量">
        <template slot-scope="scope">
          <el-slider v-model="scope.row.count" show-stops :max="num/scope.row.price" @input="changeCount(scope.row)" show-input></el-slider>
        </template>
      </el-table-column>
      <el-table-column prop="amount" label="产品总额" width="120"></el-table-column>
    </el-table>
    <span>总花费{{cost}}</span>
  </div>

</template>

<script>
export default {
  name: 'HelloWorld',
  data: function () {
    return {
      value: 10,
      num: 0,
      cost:0,
      tableData:[
        {
          name:"default",
          price:1,
          count:0,
          amount:0,
          priceShow: false,
          nameShow: false
        }
      ]
    }
  },
  methods: {
    changeCount: function (scope) {
      this.max = this.num
      scope.amount=(scope.price*scope.count);
      this.cost=0
      for (let index in this.tableData) {
        this.cost = this.cost+this.tableData[index].amount
      }
      if (this.cost>this.num) {
        scope.count = scope.count-1
      }
    },
    addRow() {
      let newRow = {
        name:"new",
        price:1,
        count:0,
        amount:0,
        priceShow: false,
        nameShow: false
      }
      this.tableData.push(newRow)
    }
  },
  props: {
    msg: String
  }
}
</script>


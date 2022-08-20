<template>
  <div class="manageUserList">
    <el-table
      ref="multipleTable"
      :data="tableData"
      tooltip-effect="dark"
      style="width: 100%"
    >
      <el-table-column type="selection" width="55"> </el-table-column>
      <el-table-column label="姓名" width="120" prop="name"> </el-table-column>
      <el-table-column prop="age" label="年龄" width="80"> </el-table-column>
      <el-table-column prop="gender" label="性别" width="80"></el-table-column>
      <el-table-column prop="tel" label="联系电话" width="160">
      </el-table-column>
      <el-table-column
        prop="address"
        label="详细地址"
        width="500"
        show-overflow-tooltip
      >
      </el-table-column>
      <el-table-column label="操作" width="300">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)"
            >编辑</el-button
          >

          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: "ManageList",
  data() {
    return {
      tableData: [],
    };
  },
  methods: {
    sendSelected() {
      let selected = this.$refs.multipleTable.selection
      let tempArr = [];
      if (selected.length > 0) {
        for (
          let index = 0;
          index < selected.length;
          index++
        ) {
            tempArr.push(selected[index].id)
        }
      }
      this.$parent.selectionUser = tempArr;
      
    },
    // 编辑
    handleEdit(index, row) {
      this.$parent.form = {
        name: row.name,
        age: row.age,
        address: row.address,
        tel: row.tel,
        gender: row.gender,
        id: row.id,
      };
      this.$parent.isEditDisplay = true;
      console.log(this.$parent.form);
    },
    handleDelete(index, row) {
      console.log(index, row);
      localStorage.removeItem(row.id);
      location.reload();
    },
  },
  //   获取localStorage数据
  created() {
    console.log(this.$parent.input);

    for (let i = 0; i < localStorage.length; i++) {
      let key = localStorage.key(i); //获取本地存储的Key
      console.log(key);
      if (key.indexOf("User") !== -1) {
        let UserMsg = JSON.parse(localStorage.getItem(key));
        if (
          this.$parent.input == "" ||
          UserMsg.name.indexOf(this.$parent.input) !== -1
        ) {
          this.$data.tableData.push(UserMsg);
        }
      }
    }
  },
};
</script>

<style>
</style>
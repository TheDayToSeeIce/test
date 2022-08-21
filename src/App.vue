<template>
  <div id="app">
    <h1 class="title">用户管理</h1>
    <div class="headerBar">
      <el-button type="primary" round id="newUserBtn" @click="dialog = true"
        >新建</el-button
      >

      <el-input
        placeholder="按关键字查询"
        v-model="input"
        clearable
        id="searchInput"
        ref="UserList"
        @keyup.enter.native="searchUser()"
      >
      </el-input>
       <el-tooltip class="item" effect="dark" content="未实现！！" placement="top-start">
       <el-button type="warning" round id="cancelBtn" @click="cancelSubmit()"
        >撤销</el-button
      >
    </el-tooltip>
     
    </div>
    <!-- 子组件 -->
    <div>
      <manage-list ref="child" v-if="isShowChild"></manage-list>
    </div>
    <div class="footerBar">
      <el-button type="danger" round id="batchDeleteBtn" @click="deleteOnbetch()">批量删除</el-button>
    </div>
    <!-- 新建弹出框 -->
    <el-dialog-form
      :visible.sync="dialog"
      title="表单标题"
      width="500px"
      :items="items"
      :form="form"
      @submit="onSubmit"
    >
    </el-dialog-form>
    <div class="editForm" v-if="isEditDisplay">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="名字">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="性别">
          <el-select v-model="form.gender" placeholder="选择性别">
            <el-option label="男" value="男"></el-option>
            <el-option label="女" value="女"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="年龄">
          <el-input v-model="form.age"></el-input>
        </el-form-item>
        <el-form-item label="电话">
          <el-input v-model="form.tel"></el-input>
        </el-form-item>
        <el-form-item label="地址">
          <el-input v-model="form.address"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="saveChange()">修改</el-button>
          <el-button @click="isEditDisplay = false">取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import ManageList from "./components/ManageList.vue";
import { v4 as uuidv4 } from "uuid";
import elDialogForm from "el-dialog-form";

export default {
  name: "App",
  data() {
    return {
      msg: "hello",
      selectionUser:[],
      isShowChild:true,
      form:{},
      keyWord:'',
      isEditDisplay:false,
      input: "",
      dialog: false,
      form: {
        input: "",
        inputNumber: 0,
        switch: false,
        timePicker: "",
        datePicker: "",
        radioGroup: 1,
        checkboxGroup: [],
        select: [],
        checkbox: true,
      },
      items: [
        {
          type: "input",
          label: "姓名",
          prop: "name",
          rules: [
            {
              required: true,
              message: "请输入姓名",
              trigger: "blur",
            },
          ],
          attrs: {
            placeholder: "请输入姓名",
          },
          on: {
            blur: (e) => {
              console.log(e);
            },
          },
        },
        {
          type: "select",
          label: "性别",
          prop: "gender",
          options: [
            {
              label: "男",
              value: "男",
            },
            {
              label: "女",
              value: "女",
            },
          ],
          attrs: {
            placeholder: "请选择性别",
          },
          on: {
            change: (e) => {
              console.log("-----------");
              console.log(e);
            },
          },
        },
        {
          type: "input",
          label: "联系电话",
          prop: "tel",
          rules: [
            {
              required: true,
              message: "请输入电话",
              trigger: "blur",
            },
          ],
          attrs: {
            placeholder: "请输入电话",
          },
          on: {
            blur: (e) => {
              console.log(e);
            },
          },
        },
        {
          type: "input",
          label: "年龄",
          prop: "age",
          rules: [
            {
              required: true,
              message: "请输入年龄",
              trigger: "blur",
            },
          ],
          attrs: {
            placeholder: "请输入年龄",
          },
          on: {
            blur: (e) => {
              console.log(e);
            },
          },
        },

        {
          type: "input",
          label: "地址",
          prop: "address",
          rules: [
            {
              required: true,
              message: "请输入地址",
              trigger: "blur",
            },
          ],
          attrs: {
            placeholder: "请输入地址",
          },
          on: {
            blur: (e) => {
              console.log(e);
            },
          },
        },
      ],
    };
  },
  components: {
    // HelloWorld
    ManageList,
    elDialogForm,
  },
  methods: {
    onSubmit(form) {
      let id = "User-" + uuidv4();
      let newPerson = {
        name: form.name,
        tel: form.tel,
        gender: form.gender,
        age: form.age,
        address: form.address,
        id,
      };
      console.log(newPerson);
      newPerson = JSON.stringify(newPerson);
      localStorage.setItem(id, newPerson);
      location.reload();
    },
    cancelSubmit() {
      
    },
    saveChange() {
      localStorage[this.form.id] = JSON.stringify(this.form)
      this.form={}
      confirm('修改成功')
      location.reload()
    },
    searchUser() {
      this.isShowChild = !this.isShowChild
      setTimeout(() => {
        this.isShowChild = !this.isShowChild
      }, 100);
    },
    deleteOnbetch() {
     console.log('',this.$refs.child);
     this.$refs.child.sendSelected()
     for (let i = 0; i < this.selectionUser.length; i++) {
      localStorage.removeItem(this.selectionUser[i])
     }
     location.reload()
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#searchInput {
  max-width: 500px;
  position: relative;
  left: -388px;
  top: -38px;
}

.title {
  background-color: gray;
}
#newUserBtn {
  float: left;
}
#cancelBtn {
  position: relative;
  top: -88px;
  left: 600px;
}

#batchDeleteBtn {
  position: relative;
  top: 20px;
  left: -700px;
}

.editForm {
  background-color: rgb(224, 235, 15);
  width: 600px;
  height: 400px;
  border-radius: 20px;
  padding: 20px;
  z-index: 999;
  position: relative;
  display: inline-block;
  top: -200px;
}
</style>

<template>
  <div>
    <form @submit.prevent>
      <fieldset>
        <legend>员工信息录入系统</legend>
        <div>
          <span>姓名：</span>
          <input maxlength="20" type="text" placeholder="请输入姓名" v-model="newUser.name">
        </div>
        <div>
          <span>性别：</span>
          <select v-model="newUser.sex">
            <option value="男">男</option>
            <option value="女">女</option>
          </select>
        </div>
        <div>
          <span>年龄：</span>
          <input type="text" placeholder="请输入年龄" v-model="newUser.age">
        </div>
        <div>
          <span>手机：</span>
          <input type="text" placeholder="请输入手机号码"  v-model="newUser.phone">
        </div>
        <input id="createID" type="submit" @click="subUser" value="创建用户">
      </fieldset>
    </form>
    <table>
      <thead>
      <tr>
        <td>姓名</td>
        <td>性别</td>
        <td>年龄</td>
        <td>手机</td>
        <td>操作</td>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(user, index) in users">
        <td>{{user.name}}</td>
        <td>{{user.sex}}</td>
        <td>{{user.age}}</td>
        <td>{{user.phone}}</td>
        <td><button @click="deleteUser(index)" id="deleteBtn">删除</button></td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name: "Users",
    data(){
      return {
        newUser: { name: '', sex: '男', age: '', phone: '' },
        users: []
      }
    },
    methods: {
      // 添加数据
      subUser(){
        if(this.newUser.name === ""){
          alert("请输入姓名！");
          return;
        }
        if(this.newUser.sex === ""){
          alert("请选择性别！");
          return;
        }
        if(this.newUser.age === "" || !(/^[1-9]\d?$|^1[0-4]\d$|^0$|^150$/).test(this.newUser.age)){
          alert("请输入正确的年龄");
          return;
        }
        if(this.newUser.phone === "" || !(/^1[34578]\d{9}$/.test(this.newUser.phone))){
          alert("请输入正确的手机号码！");
          return;
        }
        // 添加数据方法
        axios.post("https://wd5771262827xbwryk.wilddogio.com/posts.json", this.newUser).then(() => {
          // 自刷新
          axios.get("https://wd5771262827xbwryk.wilddogio.com/posts.json").then((data) => {
            return data.data
          }).then((data) => {
            let adminArr = [];
            for(let key in data){
              data[key].id = key;
              adminArr.unshift(data[key]);
            }
            this.users = adminArr;
            this.newUser = { name: '', sex: '男', age: '', phone: '' };
          });
        });
      },
      // 删除数据
      deleteUser(index){
        axios.get("https://wd5771262827xbwryk.wilddogio.com/posts.json").then((data) => {
          return data.data
        }).then(() => {
          axios.delete("https://wd5771262827xbwryk.wilddogio.com/posts/" + this.users[index].id + ".json").then(() => {
            this.users.splice(index, 1);
          });
        });
      }
    },
    // 显示数据
    created(){
      this.$http.get("https://wd5771262827xbwryk.wilddogio.com/posts.json").then((data) => {
        return data.data
      }).then(function (data) {
        let adminArr = [];
        for(let key in data){
          data[key].id = key;
          adminArr.unshift(data[key]);
        }
        this.users = adminArr;
      });
    }
  }
</script>

<style scoped>
  fieldset{
    border: 1px solid orange;
    margin-bottom: 20px;
  }
  fieldset input, fieldset select{
    width: 200px;
    height: 30px;
    margin: 10px 0;
    outline: none;
    padding: 0 5px;
  }
  fieldset select{
    width: 214px;
    height: 34px;
  }
  table{
    width: 800px;
    border: 2px solid orange;
    text-align: center;
  }
  thead{
    background-color: orange;
  }
  #createID{
    width: 100%;
    height: 30px;
    background-color: orange;
    border: none;
    color: white;
    border-radius: 10px;
    cursor: pointer;
  }
  #createID:hover{
    background-color: orangered;
  }
  #deleteBtn{
    background-color: orange;
    border: none;
    color: white;
    cursor: pointer;
    outline: none;
  }
  #deleteBtn:hover{
    background-color: orangered;
  }
</style>

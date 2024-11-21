<template>
  <a-row class="parent">
    <a-col :span="12"> left </a-col>
    <a-col :span="12">
      <a-tabs v-model:activeKey="activeKey" class="rightbox" centered>
        <a-tab-pane key="1" tab="Sign Up">
          <a-form
            class="form"
            :model="formState"
            name="basic"
            autocomplete="off"
            @finish="onFinish"
            @finishFailed="onFinishFailed"
            :label-col="{ span: 8 }"
            :wrapper-col="{ span: 16 }"
          >
            <a-form-item
              class="item1"
              v-model="signUpUsername"
              label="ID"
              name="username"
              :rules="[{ required: true, message: 'Please input your username!' }]"
            >
              <a-input v-model:value="signUpState.username" class="input" />
            </a-form-item>

            <a-form-item
              class="item2"
              v-model="signUpPassword"
              label="Password"
              name="signUpPassword"
              :rules="[{ required: true, message: 'Please input your password!' }]"
            >
              <a-input-password v-model:value="signUpState.password" class="input" />
            </a-form-item>

            <a-form-item class="item4">
              <a-button type="primary" class="signupbtn" @click="onSignup">Sign Up</a-button>
            </a-form-item>
          </a-form>
        </a-tab-pane>
        <a-tab-pane key="2" tab="Sign In">
          <a-form
            class="form"
            :model="formState"
            name="basic"
            autocomplete="off"
            @finish="onFinish"
            @finishFailed="onFinishFailed"
            :label-col="{ span: 8 }"
            :wrapper-col="{ span: 16 }"
          >
            <a-form-item
              class="item1"
              v-model="username"
              label="ID"
              name="username"
              :rules="[{ required: true, message: 'Please input your username!' }]"
            >
              <a-input v-model:value="loginState.username" class="input" />
            </a-form-item>

            <a-form-item
              class="item2"
              v-model="password"
              label="Password"
              name="password"
              :rules="[{ required: true, message: 'Please input your password!' }]"
            >
              <a-input-password v-model:value="loginState.password" class="input" />
            </a-form-item>

            <a-form-item name="remember" class="item3">
              <a-checkbox v-model:checked="loginState.remember">Remember me</a-checkbox>
            </a-form-item>

            <a-form-item class="item4">
              <a-button type="primary" class="btn" @click="login()">Login</a-button>
            </a-form-item>
          </a-form>
        </a-tab-pane>
      </a-tabs>
    </a-col>
  </a-row>
</template>

<script setup>
import { reactive, ref } from 'vue'
import _ from 'lodash'
import { getUserData, setUserList } from './api/api'
import axios from 'axios'
const activeKey = ref('1')
const loginState = reactive({
  username: '',
  password: '',
  remember: true,
})
const signUpState = reactive({
  username: '',
  password: '',
})

const login = () => {
  getUserData().then((res) => {
    const arr = res.data.userList
    const user = arr.find(
      (user) => user.username === loginState.username && user.password === loginState.password,
    )
    if (user) {
      alert('login success!')
    } else {
      alert('login faild!')
    }
  })
}

const onFinishFailed = (errorInfo) => {
  console.log('Failed:', errorInfo)
}
const onSignup = () => {
  let newUser = {
    username: signUpState.username,
    password: signUpState.password,
    role: 'user',
    name: 'LEON',
    tableNumber: '0'
  }
  axios.get('http://localhost:5177/profile/user').then((res) => {
    let userList = Array.isArray(res.data.userList) ? res.data.userList : [];
    userList.push(newUser);
    console.log("userList:",userList);
    axios
      .patch('http://localhost:5177/profile/user', { userList: userList})
      .then(() => {
        alert('success!')
      })
  })
}
</script>

<style scoped lang="css">
.form {
  width: 800px;
  padding-top: 10%;
  padding-right: 50%;
}

.rightbox {
  margin-top: 100px;
  margin-left: 220px;
  width: 900px;
}
.btn {
  width: 180px;
  position: relative;
  left: 330px;
  font-size: 20px;
}
.signupbtn {
  width: 150px;
  position: relative;
  left: 340px;
  top: 20px;
  font-size: 20px;
}
.item1 {
  position: relative;
  left: 200px;
  transform: scale(1.2);
}
.item2 {
  position: relative;
  left: 200px;
  transform: scale(1.2);
}
.input {
  width: 200px;
}
.item3 {
  position: relative;
  right: -215px;
  top: -15px;
  transform: scale(1.2);
}
</style>

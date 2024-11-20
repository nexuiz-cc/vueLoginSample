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
              <a-button type="primary" html-type="submit" class="signupbtn">Sign Up</a-button>
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
              <a-button type="primary"class="btn" @click="login()">Login</a-button>
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
import { getUserData } from './api/api'
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
</script>

<style scoped lang="css">
.form {
  width: 800px;
  padding-top: 10%;
  padding-right: 50%;
}

.parent {
}
.rightbox {
  margin-top: 100px;
  margin-left: 220px;
  width: 500px;
}
.btn {
  width: 150px;
  position: relative;
  left: 200px;
  top: -20px;
}
.signupbtn{
  width: 150px;
  position: relative;
  left: 200px;
  top: 10px;
}
.item1 {
  font-size: 20px;
}
.input {
  width: 200px;
}
.item3 {
  position: relative;
  right: -75px;
  top: -20px;
}
</style>
